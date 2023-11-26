import winim

var 
  processId: DWORD = 0
  hProcess: HMODULE = 0
  attached: bool
  moduleAddr: int32 = 0

type 
  StatusCode* = enum
    succeed, failed_processid, failed_hprocess, failed_module

proc toString(chars: openArray[WCHAR]): string =
    result = ""
    for c in chars:
        if cast[char](c) == '\0':
            break
        result.add(cast[char](c))

proc GetProcessID(procName: string): int32 =
    var 
        processInfoPE: PROCESSENTRY32
        hSnapshot: HANDLE

    processInfoPE.dwSize = cast[DWORD](sizeof(PROCESSENTRY32))
    hSnapshot = CreateToolhelp32Snapshot(15, 0)

    if Process32First(hSnapshot, processInfoPE.addr):
        while Process32Next(hSnapshot, processInfoPE.addr):
            if processInfoPE.szExeFile.toString == procName:
                CloseHandle(hSnapshot)
                return processInfoPE.th32ProcessID
    CloseHandle(hSnapshot)
    return 0


proc GetProcessModuleHandle*(moduleName: string): HMODULE =
  var 
      moduleInfoPE: MODULEENTRY32
      hSnapshot: HANDLE

  moduleInfoPE.dwSize = cast[DWORD](sizeof(MODULEENTRY32))
  hSnapshot = CreateToolhelp32Snapshot(TH32CS_SNAPMODULE, processId)

  if Module32First(hSnapshot, moduleInfoPE.addr):
      while Module32Next(hSnapshot, moduleInfoPE.addr):
          if moduleInfoPE.szModule.toString == moduleName:
              CloseHandle(hSnapshot)
              return moduleInfoPE.hModule
  CloseHandle(hSnapshot)
  return 0
