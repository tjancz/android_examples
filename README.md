powershell.exe -ExecutionPolicy Bypass -File .\antiidle.ps1

# --- HYPERVISOR / FUNKCJE OPCJONALNE ---
dism /Online /Disable-Feature:Microsoft-Hyper-V-All /NoRestart
dism /Online /Disable-Feature:HypervisorPlatform /NoRestart
dism /Online /Disable-Feature:VirtualMachinePlatform /NoRestart
dism /Online /Disable-Feature:IsolatedUserMode /NoRestart

# --- BCDEDIT: nie uruchamiaj hypervisora ---
bcdedit /set hypervisorlaunchtype off

# --- DEVICE GUARD / VBS ---
reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard" /v EnableVirtualizationBasedSecurity /t REG_DWORD /d 0 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard" /v RequirePlatformSecurityFeatures /t REG_DWORD /d 0 /f
reg delete "HKLM\SOFTWARE\Policies\Microsoft\Windows\DeviceGuard" /f 2>nul

# --- HVCI (Memory Integrity) ---
reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard\Scenarios\HypervisorEnforcedCodeIntegrity" /v Enabled /t REG_DWORD /d 0 /f

# --- CREDENTIAL GUARD ---
reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard\Scenarios\CredentialGuard" /v Enabled /t REG_DWORD /d 0 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /v LsaCfgFlags /t REG_DWORD /d 0 /f

# --- WYŁĄCZ szybkie uruchamianie (żeby był pełny, "zimny" start) ---
powercfg -h off

Write-Host "`n[INFO] Wykonaj TERAZ pełny restart: shutdown /s /t 0" -ForegroundColor Yellow







Set-ExecutionPolicy Bypass -Scope Process -Force


C:\Scripts\disable-vbs.ps1


systeminfo | findstr /i "Virtualization"
Get-CimInstance -ClassName Win32_DeviceGuard | Select SecurityServicesRunning,VirtualizationBasedSecurityStatus



"C:\Program Files\qemu\qemu-system-x86_64.exe" `
  -machine type=pc,accel=whpx -cpu host -m 8192 -smp 4 `
  -display sdl `
  -drive "if=none,file=C:\qemu\ubuntu2404.qcow2,format=qcow2,id=vd0,cache=writeback" `
  -device virtio-blk-pci,drive=vd0 `
  -drive "if=none,media=cdrom,file=C:\qemu\ubuntu-24.04.3-desktop-amd64.iso,id=cd0" `
  -device ide-cd,drive=cd0 `
  -netdev user,id=n0 -device virtio-net-pci,netdev=n0 `
  -boot order=d

