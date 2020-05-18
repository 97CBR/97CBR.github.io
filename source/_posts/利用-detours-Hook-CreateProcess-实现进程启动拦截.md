---
title: 利用 Detours Hook CreateProcess 实现进程启动拦截
tags:
  - Hook
  - 进程启动拦截
categories: C++
keywords:
  - Hook
  - detours
  - 程启动拦截
description: 利用 detours Hook CreateProcess 实现进程启动拦截
cover: >-
  https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-2/BogImages/1585814702432.png
abbrlink: 79a50542
date: 2020-04-02 16:00:57
---

## 前言

为什么要去利用 detours 进行 Hook CreateProcess 实现进程启动拦截
因为学弟问到了这个问题，就结合之前发出来的那个 Hook OpenProcess 的手法一起给做出来了

进程启动拦截的作用

- 监控启动的进程，最近有什么进程启动过
- 在进程启动前可以被我进行一些操作，比如**扫描是否有毒**，有问题劳资就不给他启动了，对吧

所以，就做了一下这个~

我们知道 detours 是需要 一个和原函数声明一样的假冒函数的，所以，可以大部分参考[之前那篇文章](https://www.marxcbr.cn/archives/1dd9aa1.html)进行处理

这次代码贴的有点多，所以，就直接先给大家看看效果
想先看代码建议直接拖到最后看代码。

## 效果

Gif文件过大，不适用放到博客中，请点击打开github查看
[效果GIF](https://github.com/97CBR/VS2017/blob/master/%E5%90%AF%E5%8A%A8%E7%9B%91%E6%8E%A7.gif)

## 安装detours

我在这里使用的办法，虽然是最不硬核的办法，但是可能是最节省时间的办法
那就是在**NuGet**里面安装

![enter description here](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-31/BogImages/1585663612306.png)

![enter description here](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-31/BogImages/1585663665739.png)

## 创建准备注入的dll项目

创建新项目去创建一个dll项目

![enter description here](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-31/BogImages/1585663805600.png)

## 声明我们需要的自己的 新-旧 CreateProcess 函数

注意Windows有两个CreateProcess，
- 一个是 CreateProcessA
- 一个是 CreateProcessW

``` c++
static BOOL(WINAPI* pCreateProcessA)(
	_In_opt_ LPCSTR lpApplicationName,
	_Inout_opt_ LPSTR lpCommandLine,
	_In_opt_ LPSECURITY_ATTRIBUTES lpProcessAttributes,
	_In_opt_ LPSECURITY_ATTRIBUTES lpThreadAttributes,
	_In_ BOOL bInheritHandles,
	_In_ DWORD dwCreationFlags,
	_In_opt_ LPVOID lpEnvironment,
	_In_opt_ LPCSTR lpCurrentDirectory,
	_In_ LPSTARTUPINFOA lpStartupInfo,
	_Out_ LPPROCESS_INFORMATION lpProcessInformation
) = CreateProcessA;

static BOOL(WINAPI* pCreateProcessW)(
    _In_opt_ LPCWSTR lpApplicationName,
    _Inout_opt_ LPWSTR lpCommandLine,
    _In_opt_ LPSECURITY_ATTRIBUTES lpProcessAttributes,
    _In_opt_ LPSECURITY_ATTRIBUTES lpThreadAttributes,
    _In_ BOOL bInheritHandles,
    _In_ DWORD dwCreationFlags,
    _In_opt_ LPVOID lpEnvironment,
    _In_opt_ LPCWSTR lpCurrentDirectory,
    _In_ LPSTARTUPINFOW lpStartupInfo,
    _Out_ LPPROCESS_INFORMATION lpProcessInformation
) = CreateProcessW;

BOOL WINAPI newCreateProcessA(
	_In_opt_ LPCSTR lpApplicationName,
	_Inout_opt_ LPSTR lpCommandLine,
	_In_opt_ LPSECURITY_ATTRIBUTES lpProcessAttributes,
	_In_opt_ LPSECURITY_ATTRIBUTES lpThreadAttributes,
	_In_ BOOL bInheritHandles,
	_In_ DWORD dwCreationFlags,
	_In_opt_ LPVOID lpEnvironment,
	_In_opt_ LPCSTR lpCurrentDirectory,
	_In_ LPSTARTUPINFOA lpStartupInfo,
	_Out_ LPPROCESS_INFORMATION lpProcessInformation) {

    char text[128];
    sprintf(text, "HOOKing！: %s \n", lpApplicationName);
    MessageBox(0, text, TEXT("提示"), MB_OK);

// 假设我们只拦截禁止 cmd 启动
    if (strcmp(lpApplicationName, "C:\\Windows\\System32\\cmd.exe") == 0) {

        TCHAR  text[128] = { 0 };
        wsprintf(text, "HOOKing！禁止启动: %s \n", lpApplicationName);
        MessageBox(0, text, TEXT("提示"), MB_OK);
        SetLastError(5);
        return false;
    }

    return pCreateProcessA(
               lpApplicationName,
               lpCommandLine,
               lpProcessAttributes,
               lpThreadAttributes,
               bInheritHandles,
               dwCreationFlags,
               lpEnvironment,
               lpCurrentDirectory,
               lpStartupInfo,
               lpProcessInformation
           );
}

BOOL WINAPI newCreateProcessW(
    _In_opt_ LPCWSTR lpApplicationName,
    _Inout_opt_ LPWSTR lpCommandLine,
    _In_opt_ LPSECURITY_ATTRIBUTES lpProcessAttributes,
    _In_opt_ LPSECURITY_ATTRIBUTES lpThreadAttributes,
    _In_ BOOL bInheritHandles,
    _In_ DWORD dwCreationFlags,
    _In_opt_ LPVOID lpEnvironment,
    _In_opt_ LPCWSTR lpCurrentDirectory,
    _In_ LPSTARTUPINFOW lpStartupInfo,
    _Out_ LPPROCESS_INFORMATION lpProcessInformation) {

    TCHAR  text[128] = { 0 };
    wsprintf(text, "HOOKing！: %s \n", lpApplicationName);
    MessageBox(0, text, TEXT("提示"), MB_OK);

	// 假设我们只拦截禁止 cmd 启动
    if (_wcsicmp(lpApplicationName, L"C:\\Windows\\System32\\cmd.exe") == 0) {
        TCHAR  text[128] = { 0 };
        wsprintf(text, "HOOKing！禁止启动: %s \n", lpApplicationName);
        MessageBox(0, text, TEXT("提示"), MB_OK);
        SetLastError(5);
        return false;
    }

    return pCreateProcessW(
               lpApplicationName,
               lpCommandLine,
               lpProcessAttributes,
               lpThreadAttributes,
               bInheritHandles,
               dwCreationFlags,
               lpEnvironment,
               lpCurrentDirectory,
               lpStartupInfo,
               lpProcessInformation
           );
}

```

## 声明DLL共享节，做 Hook 的控制

``` c++
//共享代码段
#pragma data_seg("SHARED")

HHOOK g_hOpenProcessHook = NULL;

BOOL g_bStopHook = FALSE;
BOOL g_bHookInstalled = FALSE;

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
                                          CreateProcessHookProc,
                                          ModuleFromAddress(CreateProcessHookProc), 0);

    if (g_hOpenProcessHook == NULL) {
        char text[30];
        wsprintf(text, "HOOK失败！error : %d n", GetLastError());
        MessageBox(0, text, TEXT("错误"), MB_OK);
        return false;
    }
    return TRUE;
}
```

## 调用这个 拦截启动HOOK的方法

声明两个函数、一个是进行HOOK函数、一个是取消HOOK的函数，可以灵活处理

**此代码处于另一软件项目**

``` c++
void forLong::myhook() {
    ui.label->setText("hooking");
    HINSTANCE hDLL = ::LoadLibrary(L"MarxHook.dll");

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
    HINSTANCE hDLL = ::LoadLibrary(L"MarxHook.dll");
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

## 整个dll项目代码一起给出来吧

``` c++
// dllmain.cpp : 定义 DLL 应用程序的入口点。
#include "pch.h"
#include <stdlib.h>
#include <iostream>
#include <detours.h>
#include <Windows.h>
#include <TlHelp32.h>
#include <fstream>
#include <algorithm>
#pragma warning(disable:4996)

static HMODULE s_hDll;
//共享代码段
#pragma data_seg("SHARED")
HHOOK g_hMessageHook = NULL;
HHOOK g_hOpenProcessHook = NULL;

BOOL g_bStopHook = FALSE;
BOOL g_bHookInstalled = FALSE;
DWORD g_pid = 74144;

#pragma data_seg()
#pragma comment(linker, "/section:SHARED,RWS")
using namespace std;

static HANDLE(WINAPI* pOpenProcess)(DWORD dwDesiredAccess, BOOL bInheritHandle, DWORD dwProcessId) = OpenProcess;


static BOOL(WINAPI* pCreateProcessA)(
	_In_opt_ LPCSTR lpApplicationName,
	_Inout_opt_ LPSTR lpCommandLine,
	_In_opt_ LPSECURITY_ATTRIBUTES lpProcessAttributes,
	_In_opt_ LPSECURITY_ATTRIBUTES lpThreadAttributes,
	_In_ BOOL bInheritHandles,
	_In_ DWORD dwCreationFlags,
	_In_opt_ LPVOID lpEnvironment,
	_In_opt_ LPCSTR lpCurrentDirectory,
	_In_ LPSTARTUPINFOA lpStartupInfo,
	_Out_ LPPROCESS_INFORMATION lpProcessInformation
) = CreateProcessA;

static BOOL(WINAPI* pCreateProcessW)(
    _In_opt_ LPCWSTR lpApplicationName,
    _Inout_opt_ LPWSTR lpCommandLine,
    _In_opt_ LPSECURITY_ATTRIBUTES lpProcessAttributes,
    _In_opt_ LPSECURITY_ATTRIBUTES lpThreadAttributes,
    _In_ BOOL bInheritHandles,
    _In_ DWORD dwCreationFlags,
    _In_opt_ LPVOID lpEnvironment,
    _In_opt_ LPCWSTR lpCurrentDirectory,
    _In_ LPSTARTUPINFOW lpStartupInfo,
    _Out_ LPPROCESS_INFORMATION lpProcessInformation
) = CreateProcessW;


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
//CreateProcess
BOOL WINAPI newCreateProcessA(
	_In_opt_ LPCSTR lpApplicationName,
	_Inout_opt_ LPSTR lpCommandLine,
	_In_opt_ LPSECURITY_ATTRIBUTES lpProcessAttributes,
	_In_opt_ LPSECURITY_ATTRIBUTES lpThreadAttributes,
	_In_ BOOL bInheritHandles,
	_In_ DWORD dwCreationFlags,
	_In_opt_ LPVOID lpEnvironment,
	_In_opt_ LPCSTR lpCurrentDirectory,
	_In_ LPSTARTUPINFOA lpStartupInfo,
	_Out_ LPPROCESS_INFORMATION lpProcessInformation) {

    char text[128];
    sprintf(text, "HOOKing！: %s \n", lpApplicationName);
    MessageBox(0, text, TEXT("提示"), MB_OK);

    if (strcmp(lpApplicationName, "C:\\Windows\\System32\\cmd.exe") == 0) {

        TCHAR  text[128] = { 0 };

        wsprintf(text, "HOOKing！禁止启动: %s \n", lpApplicationName);
        MessageBox(0, text, TEXT("提示"), MB_OK);

        SetLastError(5);
        return false;
    }

    return pCreateProcessA(
               lpApplicationName,
               lpCommandLine,
               lpProcessAttributes,
               lpThreadAttributes,
               bInheritHandles,
               dwCreationFlags,
               lpEnvironment,
               lpCurrentDirectory,
               lpStartupInfo,
               lpProcessInformation
           );
}

BOOL WINAPI newCreateProcessW(
    _In_opt_ LPCWSTR lpApplicationName,
    _Inout_opt_ LPWSTR lpCommandLine,
    _In_opt_ LPSECURITY_ATTRIBUTES lpProcessAttributes,
    _In_opt_ LPSECURITY_ATTRIBUTES lpThreadAttributes,
    _In_ BOOL bInheritHandles,
    _In_ DWORD dwCreationFlags,
    _In_opt_ LPVOID lpEnvironment,
    _In_opt_ LPCWSTR lpCurrentDirectory,
    _In_ LPSTARTUPINFOW lpStartupInfo,
    _Out_ LPPROCESS_INFORMATION lpProcessInformation) {

    TCHAR  text[128] = { 0 };

    wsprintf(text, "HOOKing！: %s \n", lpApplicationName);
    MessageBox(0, text, TEXT("提示"), MB_OK);

    if (_wcsicmp(lpApplicationName, L"C:\\Windows\\System32\\cmd.exe") == 0) {

        TCHAR  text[128] = { 0 };
        wsprintf(text, "HOOKing！禁止启动: %s \n", lpApplicationName);
        MessageBox(0, text, TEXT("提示"), MB_OK);

        SetLastError(5);
        return false;
    }

    return pCreateProcessW(
               lpApplicationName,
               lpCommandLine,
               lpProcessAttributes,
               lpThreadAttributes,
               bInheritHandles,
               dwCreationFlags,
               lpEnvironment,
               lpCurrentDirectory,
               lpStartupInfo,
               lpProcessInformation
           );
}


//查找函数地址
HMODULE WINAPI ModuleFromAddress(PVOID pv) {
    MEMORY_BASIC_INFORMATION mbi;
    if (::VirtualQuery(pv, &mbi, sizeof(mbi)) != 0)
        return (HMODULE)mbi.AllocationBase;
    else
        return NULL;
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



BOOL APIENTRY SetHook() {
    //如果已经安装就return
    if (g_bHookInstalled)
        return TRUE;

    DetourTransactionBegin();
    DetourUpdateThread(GetCurrentThread());

    DetourAttach((PVOID*)&pOpenProcess, hOpenProcess);
    DetourAttach((PVOID*)&pCreateProcessA, newCreateProcessA);
    DetourAttach((PVOID*)&pCreateProcessW, newCreateProcessW);

	DetourTransactionCommit();

    while (pubgHandle != NULL) {
        char buffer[3];

        ReadProcessMemory(pubgHandle, (LPVOID)GetModuleBaseAddress(g_pid, processName), buffer, 2, NULL);
        buffer[3] = '\0';

        cout << buffer << endl;
    }

    g_bHookInstalled = TRUE;
    return g_bHookInstalled;
}

BOOL APIENTRY DropHook() {
    //如果已经卸载就return
    if (!g_bHookInstalled)return TRUE;

    DetourTransactionBegin();
    DetourUpdateThread(GetCurrentThread());
    DetourDetach((PVOID*)&pOpenProcess, hOpenProcess);
    DetourDetach((PVOID*)&pCreateProcessA, newCreateProcessA);
    DetourDetach((PVOID*)&pCreateProcessW, newCreateProcessW);

    LONG ret = DetourTransactionCommit();
    g_bHookInstalled = FALSE;
    return ret == NO_ERROR;
}





static LRESULT CALLBACK CreateProcessHookProc(int nCode, WPARAM wParam, LPARAM lParam) {
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

    //C:\Windows\explorer.exe
    //C:\Windows\System32\Taskmgr.exe

    if ((_wcsicmp(Filename, L"explorer") == 0) || (_wcsicmp(Filename, L"taskmgr") == 0)) {
        //if (true) {

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
                                          CreateProcessHookProc,
                                          ModuleFromAddress(CreateProcessHookProc), 0);

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






BOOL APIENTRY DllMain(HMODULE hModule,
                      DWORD  ul_reason_for_call,
                      LPVOID lpReserved
                     ) {
    switch (ul_reason_for_call) {
    case DLL_PROCESS_ATTACH:
        s_hDll = hModule;
        DisableThreadLibraryCalls(hModule);
        //SetGoableHook();
        break;
    case DLL_THREAD_ATTACH:
        break;
    case DLL_THREAD_DETACH:
        break;
    case DLL_PROCESS_DETACH:
        //DropGoableHook();
        UnhookWindowsHookEx(g_hOpenProcessHook);
        break;
    }
    return TRUE;
}


```

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