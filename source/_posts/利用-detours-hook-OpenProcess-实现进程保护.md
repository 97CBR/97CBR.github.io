---
title: 利用 detours hook OpenProcess 实现指定进程保护
abbrlink: 1dd9aa1
date: 2020-03-31 22:00:57
tags:
    - Hook
    - 进程保护
categories: C++
keywords:
    - Hook
    - detours
    - 指定进程保护
description: 利用 detours hook OpenProcess 实现指定进程保护
cover: https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-31/BogImages/1585666062216.png
---

## 前言

为什么要去利用 detours 进行 hook OpenProcess 实现指定进程保护呢
因为我的毕设是移动存储设备的安全监控
所以，要求软件在电脑上运行的时候，不允许用户自由的关闭程序。得将自己保护起来，就像杀软一样，要先将自己保护好了，再去保护别人

## 效果

这次代码贴的有点多，所以，就直接先给大家看看效果

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/Blog/%E6%95%88%E6%9E%9C.gif)

## 安装detours

我在这里使用的办法，虽然是最不硬核的办法，但是可能是最节省时间的办法
那就是在**NuGet**里面安装

![enter description here](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-31/BogImages/1585663612306.png)

![enter description here](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-31/BogImages/1585663665739.png)

## 创建准备注入的dll项目

创建新项目去创建一个dll项目

![enter description here](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-31/BogImages/1585663805600.png)

## 声明我们需要的自己的 新-旧 OpenProcess函数

``` c++
static HANDLE(WINAPI* pOpenProcess)(DWORD dwDesiredAccess, BOOL bInheritHandle, DWORD dwProcessId) = OpenProcess;

HANDLE WINAPI hOpenProcess(DWORD dwDesiredAccess, BOOL bInheritHandle, DWORD dwProcessId) {
	//if (GetProcID(processName) == dwProcessId) {
	if (g_pid == dwProcessId) {
		dwDesiredAccess = PROCESS_VM_READ | PROCESS_VM_WRITE | PROCESS_QUERY_INFORMATION;
		pubgHandle = pOpenProcess(dwDesiredAccess, bInheritHandle, dwProcessId);
		SetLastError(5);
		return false;
	}

	return pOpenProcess(dwDesiredAccess, bInheritHandle, dwProcessId);
}

```

## 声明DLL共享节，将我们需要保护的进程pid什么的放进去

``` c++
//共享代码段
#pragma data_seg("SHARED")

HHOOK g_hOpenProcessHook = NULL;

BOOL g_bStopHook = FALSE;
BOOL g_bHookInstalled = FALSE;
DWORD g_pid = 0;

#pragma data_seg()
#pragma comment(linker, "/section:SHARED,RWS")
```

## 利用SetWindowsHookEx进行全局HOOK

在OpenProcessHookProc这个回调函数中进行处理我们的HOOK

``` c++

//导出函数 加载全局钩子
extern "C" __declspec(dllexport) BOOL SetGoableHook() {
	g_bStopHook = FALSE;
	g_hOpenProcessHook = SetWindowsHookEx(WH_GETMESSAGE,
		OpenProcessHookProc,
		ModuleFromAddress(OpenProcessHookProc), 0);

	if (g_hOpenProcessHook == NULL) {
		char text[30];
		wsprintf(text, "HOOK失败！error : %d n", GetLastError());
		MessageBox(0, text, TEXT("错误"), MB_OK);
		return false;
	}
	return TRUE;
}
```

## 调用这个保护HOOK的方法

声明两个函数、一个是进行HOOK函数、一个是取消HOOK的函数，可以灵活处理

**此代码处于另一软件项目**

``` c++
void forLong::myhook() {
    ui.label->setText("hooking");
    HINSTANCE hDLL = ::LoadLibrary(L"HOOK.dll");

    if (hDLL) {

        PFN_setData setData = (PFN_setData)::GetProcAddress(hDLL, "setData");
        PFN_getData getData = (PFN_getData)::GetProcAddress(hDLL, "getData");
        SetGoableHook func = (SetGoableHook)GetProcAddress(hDLL, "SetGoableHook");
        qDebug() << (int)getpid() << endl;
        setData((int)getpid());
        int s = getData();
        func();
        /*qDebug() <<  << endl;*/
    }

}

void forLong::unhook() {
    ui.label->setText("unhook");
    HINSTANCE hDLL = ::LoadLibrary(L"HOOK.dll");
    if (hDLL) {
        DropGoableHook func = (DropGoableHook)GetProcAddress(hDLL, "DropGoableHook");
        func();
        PFN_setData setData = (PFN_setData)::GetProcAddress(hDLL, "setData");
        PFN_getData getData = (PFN_getData)::GetProcAddress(hDLL, "getData");
        setData(0);
        //int s = getData();
        func();
        /*   qDebug() << getData() << endl;*/
    }

}

```

{% note warning %}
其实，单纯这样处理，如果单hook一次，任务管理器在重启的时候就会将这个HOOK给绕过了。
这样子就办法达到继续HOOK OpenProcess 的目的了
{% endnote %}

所以，我们得处理一下

## fix掉重启任务管理器绕过HOOK的方案

其实很简单，我们只要监控任务管理器是否启动就可以了
只要启动了就即可进行API拦截注入 进行HOOK
如果关闭了，则停止HOOK

代码可以这样写

``` c++

bool forLong::CheckAppRunningStatus(const QString& appName) {
#ifdef Q_OS_WIN
    QProcess* process = new QProcess;
    process->start("tasklist", QStringList() << "-fi" << "imagename eq " + appName);
    process->waitForFinished();
    QString outputStr = QString::fromLocal8Bit(process->readAllStandardOutput());
    qDebug() << outputStr << endl;
    if (outputStr.contains(appName))
        return true;
    else
        return false;
#endif
}

bool forLong::watchTaskmgr(){
    while (true) {
        if (CheckAppRunningStatus("Taskmgr.exe")) {
            qDebug() << "Taskmgr" << endl;
            myhook();
        }
        else {
            qDebug() << "no  Taskmgr" << endl;
            unhook();
        }
        Sleep(1000);

    }
}
```

## 整个dll项目代码一起给出来吧

``` c++

// dllmain.cpp : Defines the entry point for the DLL application.
#include "pch.h"
#include <stdlib.h>
#include <iostream>
#include <detours.h>
#include <Windows.h>
#include <TlHelp32.h>
#include <fstream>
#include <algorithm>

#pragma warning(disable:4996)
using namespace std;

//共享代码段
#pragma data_seg("SHARED")
HHOOK g_hMessageHook = NULL;
HHOOK g_hOpenProcessHook = NULL;

BOOL g_bStopHook = FALSE;
BOOL g_bHookInstalled = FALSE;
DWORD g_pid = 0;

#pragma data_seg()
#pragma comment(linker, "/section:SHARED,RWS")
//__declspec(allocate("SHARED"))int g_pid=0;

static HMODULE s_hDll;

HINSTANCE g_hInstance = NULL;

/*显示GetLastError的信息*/
void showerr(const char* m) {
	char message[255];
	FormatMessage(FORMAT_MESSAGE_FROM_SYSTEM, 0, GetLastError()
		, MAKELANGID(LANG_NEUTRAL, SUBLANG_DEFAULT), message, 255, 0);
	MessageBox(NULL, message, m, MB_OK);
}

DWORD MyGetProcessId(char* ProcessName) {
	PROCESSENTRY32 pe32;
	pe32.dwSize = sizeof(pe32);
	//获得系统内所有进程快照
	HANDLE hProcessSnap = CreateToolhelp32Snapshot(TH32CS_SNAPPROCESS, 0);
	if (hProcessSnap == INVALID_HANDLE_VALUE)
		return FALSE;
	//枚举列表中的第一个进程
	BOOL bProcess = Process32First(hProcessSnap, &pe32);
	while (bProcess) {
		//比较得到的进程名和要保护的进程名是否相同
		if (stricmp(pe32.szExeFile, ProcessName) == 0) {
			//相同则返回此进程ID
			return pe32.th32ProcessID;
		}
		bProcess = Process32Next(hProcessSnap, &pe32);
	}
	CloseHandle(hProcessSnap);
	//否则返回0
	return 0;
}

typedef ULONG_PTR  DWORD_PTR;

static HANDLE(WINAPI* pOpenProcess)(DWORD dwDesiredAccess, BOOL bInheritHandle, DWORD dwProcessId) = OpenProcess;

DWORD GetProcID(char* procName);

HANDLE pubgHandle = NULL;
char* processName = (char*)"forLong.exe";

HANDLE WINAPI hOpenProcess(DWORD dwDesiredAccess, BOOL bInheritHandle, DWORD dwProcessId) {
	//if (GetProcID(processName) == dwProcessId) {
	if (g_pid == dwProcessId) {
		dwDesiredAccess = PROCESS_VM_READ | PROCESS_VM_WRITE | PROCESS_QUERY_INFORMATION;
		pubgHandle = pOpenProcess(dwDesiredAccess, bInheritHandle, dwProcessId);
		SetLastError(5);
		return false;
	}

	return pOpenProcess(dwDesiredAccess, bInheritHandle, dwProcessId);
}

DWORD GetProcID(char* procName) {
	PROCESSENTRY32 pe32;
	HANDLE hSnapshot = NULL;

	pe32.dwSize = sizeof(PROCESSENTRY32);
	hSnapshot = CreateToolhelp32Snapshot(TH32CS_SNAPPROCESS, 0);

	if (Process32First(hSnapshot, &pe32)) {
		do {
			if (strcmp(pe32.szExeFile, procName) == 0)
				break;
		} while (Process32Next(hSnapshot, &pe32));
	}

	if (hSnapshot != INVALID_HANDLE_VALUE)
		CloseHandle(hSnapshot);

	return pe32.th32ProcessID;
}

DWORD_PTR GetModuleBaseAddress(DWORD pid, TCHAR* name) {
	DWORD_PTR baseAddress = NULL;
	HANDLE hSnapshot = CreateToolhelp32Snapshot(TH32CS_SNAPMODULE, pid);

	if (hSnapshot == INVALID_HANDLE_VALUE)
		return NULL;

	MODULEENTRY32 entry = { NULL };

	entry.dwSize = sizeof(MODULEENTRY32);

	if (!Module32First(hSnapshot, &entry)) {
		CloseHandle(hSnapshot);
		return NULL;
	}

	do {
		if (!strcmp(entry.szModule, name)) {
			baseAddress = (DWORD_PTR)entry.modBaseAddr;
			break;
		}
	} while (Module32Next(hSnapshot, &entry));

	CloseHandle(hSnapshot);

	return baseAddress;
}

void hookOpenProcess() {
	//AllocConsole();
	DetourTransactionBegin();
	DetourUpdateThread(GetCurrentThread());
	DetourAttach((PVOID*)&pOpenProcess, hOpenProcess);
	DetourTransactionCommit();

	while (pubgHandle != NULL) {
		char buffer[3];

		ReadProcessMemory(pubgHandle, (LPVOID)GetModuleBaseAddress(g_pid, processName), buffer, 2, NULL);
		buffer[3] = '\0';

		cout << buffer << endl;
	}
}

BOOL APIENTRY SetHook() {
	//如果已经安装就return
	if (g_bHookInstalled)
		return TRUE;

	CreateThread(NULL, NULL, reinterpret_cast<LPTHREAD_START_ROUTINE>(hookOpenProcess), NULL, NULL, NULL);

	//CreateThread(NULL, NULL, reinterpret_cast<LPTHREAD_START_ROUTINE>(watch_task), NULL, NULL, NULL);

	//char text[30];
	//wsprintf(text, "HOOK 启动！: %ld", GetCurrentThread());
	//MessageBox(0, text, TEXT("OK"), MB_OK);

	g_bHookInstalled = TRUE;
	return g_bHookInstalled;
}

BOOL APIENTRY DropHook() {
	//如果已经卸载就return
	if (!g_bHookInstalled)return TRUE;

	//char text[30];
	//wsprintf(text, "HOOK 解除！: %d", GetLastError());
	//MessageBox(0, text, TEXT("错误"), MB_OK);

	DetourTransactionBegin();
	DetourUpdateThread(GetCurrentThread());
	DetourDetach((PVOID*)&pOpenProcess, hOpenProcess);

	LONG ret = DetourTransactionCommit();
	g_bHookInstalled = FALSE;
	return ret == NO_ERROR;
}

//查找函数地址
HMODULE WINAPI ModuleFromAddress(PVOID pv) {
	MEMORY_BASIC_INFORMATION mbi;
	if (::VirtualQuery(pv, &mbi, sizeof(mbi)) != 0)
		return (HMODULE)mbi.AllocationBase;
	else
		return NULL;
}

static LRESULT CALLBACK OpenProcessHookProc(int nCode, WPARAM wParam, LPARAM lParam) {
	PKBDLLHOOKSTRUCT p = (PKBDLLHOOKSTRUCT)lParam;

	wchar_t Dir[_MAX_DIR];
	wchar_t FullPath[MAX_PATH]; // [sp+200h] [bp-614h]@1
	wchar_t Ext[_MAX_EXT];
	wchar_t Filename[_MAX_FNAME];
	wchar_t Drive[_MAX_DRIVE];
	GetModuleFileNameW(0, FullPath, MAX_PATH);
	_wsplitpath_s(FullPath, Drive, _MAX_DRIVE, Dir, _MAX_DIR, Filename, _MAX_FNAME, Ext, _MAX_EXT);

	wstring tar = Filename;

	transform(tar.begin(), tar.end(), tar.begin(), towlower);

	if (_wcsicmp(Filename, L"Taskmgr") == 0) {
		//安装钩子
		//MessageBox(NULL, "DLL_PROCESS_ATTACH taskmgr", "info", MB_OK);

		if (!g_bStopHook)
			SetHook();
		//卸载钩子
		else
			DropHook();
	}

	return CallNextHookEx(g_hOpenProcessHook, nCode, wParam, lParam);
}

//导出函数 加载全局钩子
extern "C" __declspec(dllexport) BOOL SetGoableHook() {
	g_bStopHook = FALSE;
	g_hOpenProcessHook = SetWindowsHookEx(WH_GETMESSAGE,
		OpenProcessHookProc,
		ModuleFromAddress(OpenProcessHookProc), 0);

	if (g_hOpenProcessHook == NULL) {
		char text[30];
		wsprintf(text, "HOOK失败！error : %d n", GetLastError());
		MessageBox(0, text, TEXT("错误"), MB_OK);
		return false;
	}
	return TRUE;
}

//导出函数 卸载全局钩子
extern "C" __declspec(dllexport) BOOL DropGoableHook() {
	g_bStopHook = TRUE;
	return TRUE;
}

extern "C"  __declspec(dllexport) void setData(int target_pid) {
	g_pid = target_pid;
}

extern "C"  __declspec(dllexport) int getData() {
	return g_pid;
}

HMODULE WINAPI Detoured() {
	return s_hDll;
}

BOOL APIENTRY DllMain(HMODULE hModule,
	DWORD  ul_reason_for_call,
	LPVOID lpReserved
) {
	g_hInstance = (HINSTANCE)hModule;

	switch (ul_reason_for_call) {
	case DLL_PROCESS_ATTACH:
		s_hDll = hModule;
		DisableThreadLibraryCalls(hModule);
		break;
	case DLL_THREAD_ATTACH:
		break;
	case DLL_THREAD_DETACH:
		break;
	case DLL_PROCESS_DETACH:
		UnhookWindowsHookEx(g_hOpenProcessHook);
		break;
	}
	return TRUE;
}

```

<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>