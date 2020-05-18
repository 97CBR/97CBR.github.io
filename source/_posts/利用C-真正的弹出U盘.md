---
title: 利用C++真正的弹出U盘
tags:
  - C++
keywords:
  - C++
  - 弹出U盘
abbrlink: 7fc2b650
date: 2020-04-09 21:26:08
categories: C++
description: 利用C++真正的弹出U盘
cover: https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-9/BogImages/1586442451921.png
---

## 前言

为啥我要用C++来弹出我的U盘呢~
还是我那让人可爱的毕设里面的一个朴素无华的需求
在那个场景里面，我将U盘插进去，然后对其MBR分区写入一个特定的标签后，再从那个特定的位置读取数据进行验证是否已经将标签写入。
完成了这件事之后，我就需要将U盘进行弹出，以便于后续的U盘插入并进行打标签流程

## C++ 弹出U盘有两个方案

本来我不知道U盘弹出有两个方案的，查阅资料和MSDN，最容易看到，也最常见的就是利用 **CreateFile** 打开设备的句柄，利用 **DeviceIoControl** 来对设备句柄执行 **IOCTL_STORAGE_EJECT_MEDIA** 这个操作码

当然前提是你句柄获取都是正确的，然后利用 FSCTL_LOCK_VOLUME、FSCTL_DISMOUNT_VOLUME、IOCTL_STORAGE_MEDIA_REMOVAL 这些操作码来排除其他进程的干扰和锁定U盘

可是这样子，居然弹出来的设备会变成这样子

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-9/BogImages/1586439632184.png)

你管这叫弹出？这不是我以前认识的弹出~

虽然也打开不了磁盘了，提示叫我插入磁盘

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-9/BogImages/1586439737115.png)

但是你这样子在设备管理器里面也还是可见的，即使使用不了，我利用 **GetLogicalDrives** **CreateFileA** **DeviceIoControl** 这仨函数还是可以将你枚举出来就木得意思了，对吧。

接着，我可爱的火绒给了我灵感

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-9/BogImages/1586440096994.png)

弹出前
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-9/BogImages/1586440108669.png)

弹出后
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-9/BogImages/1586440131724.png)

这才是我想要的效果啊，喵的。用**IOCTL_STORAGE_EJECT_MEDIA**弹出之后还剩一个图标给我一点也不帅好吧
**以下提供到的方案均已测试通过，实现效果如上文说过一致，按需求自取**

## 第一种解决方案，会剩一个无法访问到的盘符

解决方案这种东西，还是直接上代码说的更清楚，都是写代码的人嘛。
用代码说清楚它

``` c++
/**
 * \brief 弹出指定盘符存储设备
 * \param drive_volume_name 盘符，E，F
 * \return
 */
BOOL UsbDrivesManager::RemoveUsbByName(const char drive_volume_name) const {
    QString paths = "";
    const auto drive_name = QString("%1:").arg(drive_volume_name);
    paths = QString(R"(\\.\%1:)").arg(drive_volume_name);
    DWORD dw_ret;
    DWORD dw_error;
    WCHAR cs_volume[256] = { 0 };
    paths.toWCharArray(cs_volume);
    //打开设备
    const auto h_device = CreateFile(cs_volume,
                                     GENERIC_READ | GENERIC_WRITE,
                                     FILE_SHARE_READ | FILE_SHARE_WRITE,
                                     NULL,
                                     OPEN_EXISTING,
                                     0,
                                     NULL);

    if (h_device == INVALID_HANDLE_VALUE) {
        // can't open the drive
        dw_error = GetLastError();
        return FALSE;
    }

    if (!DeviceIoControl(h_device, FSCTL_LOCK_VOLUME, 0, 0, 0, 0, &dw_ret, 0))
        return FALSE;

    if (!DeviceIoControl(h_device, FSCTL_DISMOUNT_VOLUME, 0, 0, 0, 0, &dw_ret, 0))
        return FALSE;

    PREVENT_MEDIA_REMOVAL pmr_buffer;
    pmr_buffer.PreventMediaRemoval = FALSE;

    if (!DeviceIoControl(h_device, IOCTL_STORAGE_MEDIA_REMOVAL, &pmr_buffer, sizeof(PREVENT_MEDIA_REMOVAL), NULL, 0, &dw_ret, NULL))
        qDebug("DeviceIoControl IOCTL_STORAGE_MEDIA_REMOVAL failed:%d\n", GetLastError());

    auto b_result = DeviceIoControl(
                        h_device,
                        IOCTL_STORAGE_EJECT_MEDIA, //eject USB
                        NULL,
                        0,
                        NULL,
                        0,
                        &dw_ret,
                        static_cast<LPOVERLAPPED>(NULL));
    if (!b_result) {
        dw_error = GetLastError();
        return FALSE;
    }

    b_result = CloseHandle(h_device);
    if (!b_result) {
        dw_error = GetLastError();
        return FALSE;
    }
    return TRUE;
}
```

{% note info %}
这段代码用到了 Qt 的QString和Qdebug的库，实际使用时，可以根据需求自己稍微加以更改
{% endnote %}

## 弹出U盘，不会留下盘符图标

还是直接放代码吧

``` c++

DEVINST UsbDrivesManager::GetDrivesDevInstByDiskNumber(const long disk_number) {

    const auto h_dev_info = SetupDiGetClassDevs(&GUID_DEVINTERFACE_DISK, NULL, NULL,
                                              DIGCF_PRESENT | DIGCF_DEVICEINTERFACE);

    if (h_dev_info == INVALID_HANDLE_VALUE)
        return 0;
    DWORD dw_index = 0;
    SP_DEVICE_INTERFACE_DATA dev_interface_data = { 0 };
    dev_interface_data.cbSize = sizeof(SP_DEVICE_INTERFACE_DATA);
    auto b_ret = FALSE;


    SP_DEVICE_INTERFACE_DATA sp_did;
    SP_DEVINFO_DATA sp_dd;
    DWORD dw_size;

    sp_did.cbSize = sizeof(sp_did);

    while (true) {
        b_ret = SetupDiEnumDeviceInterfaces(h_dev_info, NULL, &GUID_DEVINTERFACE_DISK, dw_index,
                                            &dev_interface_data);
        if (!b_ret)
            break;

        SetupDiEnumInterfaceDevice(h_dev_info, NULL, &GUID_DEVINTERFACE_DISK, dw_index, &sp_did);

        dw_size = 0;
        SetupDiGetDeviceInterfaceDetail(h_dev_info, &sp_did, NULL, 0, &dw_size, NULL);

        if (dw_size) {
            auto psp_did = static_cast<PSP_DEVICE_INTERFACE_DETAIL_DATA>(HeapAlloc(
                               GetProcessHeap(), HEAP_ZERO_MEMORY, dw_size));
            if (psp_did == NULL) {
                continue; // autsch
            }
            psp_did->cbSize = sizeof(*psp_did);
            ZeroMemory(static_cast<PVOID>(&sp_dd), sizeof(sp_dd));
            sp_dd.cbSize = sizeof(sp_dd);


            long res = SetupDiGetDeviceInterfaceDetail(h_dev_info, &sp_did, psp_did, dw_size, &dw_size, &sp_dd);
            if (res) {
                const auto h_drive = CreateFile(psp_did->DevicePath, 0,
                                                FILE_SHARE_READ | FILE_SHARE_WRITE, NULL, OPEN_EXISTING, NULL, NULL);
                if (h_drive != INVALID_HANDLE_VALUE) {
                    STORAGE_DEVICE_NUMBER sdn;
                    DWORD dw_bytes_returned = 0;
                    res = DeviceIoControl(h_drive, IOCTL_STORAGE_GET_DEVICE_NUMBER, NULL, 0, &sdn, sizeof(sdn), &dw_bytes_returned, NULL);
                    if (res) {
                        if (disk_number == static_cast<long>(sdn.DeviceNumber)) {
                            CloseHandle(h_drive);
                            SetupDiDestroyDeviceInfoList(h_dev_info);
                            return sp_dd.DevInst;
                        }
                    }
                    CloseHandle(h_drive);
                }
            }
            HeapFree(GetProcessHeap(), 0, psp_did);
        }
        dw_index++;
    }

    SetupDiDestroyDeviceInfoList(h_dev_info);

    return 0;
}


int UsbDrivesManager::EjectUsbDisk(const int disk_number) {

    auto dev_inst = GetDrivesDevInstByDiskNumber(disk_number);
    if (dev_inst == 0) {
        qDebug("GetDrivesDevInstDiskNumber failed\n");
        return 0;
    }

    ULONG status = 0;
    ULONG problem_number = 0;
    auto veto_type = PNP_VetoTypeUnknown;
    wchar_t veto_name[MAX_PATH];
    auto b_success = false;

    auto res = CM_Get_Parent(&dev_inst, dev_inst, 0);
	if (res != CR_SUCCESS)
	{
		return 0;
	}
	res = CM_Get_DevNode_Status(&status, &problem_number, dev_inst, 0);
	if (res != CR_SUCCESS)
	{
		return 0;
	}
    const auto is_removable = ((status & DN_REMOVABLE) != 0);

    qDebug("is removable:%d\n", is_removable);
    // try 3 times
    for (auto i = 0; i < 3; i++) {
        veto_name[0] = '\0';
        if (is_removable)
            res = CM_Request_Device_Eject(dev_inst, &veto_type, veto_name, MAX_PATH, 0);
        else
            res = CM_Query_And_Remove_SubTree(dev_inst, &veto_type, veto_name, MAX_PATH, 0);
        b_success = (res == CR_SUCCESS && veto_name[0] == '\0');
        if (b_success)
            break;
        else
            Sleep(300);
    }
    if (b_success)
        qDebug("Success\n\n");
    else
        return 0;
    return 1;
}
```


{% note info %}
上面代码的函数用到的参数可以根据 **STORAGE_DEVICE_NUMBER**下的**DeviceNumber**进行获取
使用 **CM_Request_Device_Eject** 函数，请加上 **#pragma comment(lib, "setupapi.lib")** 以及 **#include <cfgmgr32.h>**
否则会编译失败，提示 **无法解析的外部符号**
{% endnote %}

## 彩蛋

其实我用IDA和procexp企图看火绒的U盘弹出函数或者调用的API的

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-9/BogImages/1586441793216.png)

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-9/BogImages/1586442039050.png)

结果就是失败了，哈哈哈，果然**菜是原罪**啊
不过由此得知了火绒的界面是用DuiLib开发的，哈哈哈

<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>
