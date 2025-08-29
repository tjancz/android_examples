powershell.exe -ExecutionPolicy Bypass -File .\antiidle.ps1

# ======================================================================
# disable-vbs.ps1
# Wyłącza VBS, Hyper-V/Hypervisor, Credential Guard i HVCI (Memory Integrity)
# Cel: uruchamianie QEMU z WHPX (-machine accel=whpx) bez crashy.
# ======================================================================

# 0) Wymuś admina
if (-not ([Security.Principal.WindowsPrincipal] [Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole([Security.Principal.WindowsBuiltInRole] "Administrator")) {
  Write-Host "[!] Uruchom ten skrypt jako Administrator." -ForegroundColor Red
  exit 1
}

Write-Host "`n=== [1/6] Wyłączam wszystkie składniki Hyper-V... ===" -ForegroundColor Cyan
dism.exe /Online /Disable-Feature:Microsoft-Hyper-V-All /NoRestart | Out-Null

Write-Host "=== [2/6] Wyłączam autostart hypervisora (BCDEdit) ... ===" -ForegroundColor Cyan
bcdedit /set hypervisorlaunchtype off | Out-Null

Write-Host "=== [3/6] Wyłączam VBS (Device Guard) ... ===" -ForegroundColor Cyan
reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard" /v EnableVirtualizationBasedSecurity /t REG_DWORD /d 0 /f | Out-Null
reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard" /v RequirePlatformSecurityFeatures /t REG_DWORD /d 0 /f | Out-Null

Write-Host "=== [4/6] Wyłączam Credential Guard ... ===" -ForegroundColor Cyan
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /v LsaCfgFlags /t REG_DWORD /d 0 /f | Out-Null

Write-Host "=== [5/6] Wyłączam HVCI (Memory Integrity) ... ===" -ForegroundColor Cyan
reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard\Scenarios\HypervisorEnforcedCodeIntegrity" /v Enabled /t REG_DWORD /d 0 /f | Out-Null

Write-Host "=== [6/6] (Opcjonalnie) Usuwam polityki Device Guard z HKLM\\SOFTWARE\\Policies ... ===" -ForegroundColor Cyan
reg delete "HKLM\SOFTWARE\Policies\Microsoft\Windows\DeviceGuard" /f 2>$null | Out-Null

# Podgląd stanu przed restartem
Write-Host "`n[Info] Stan Device Guard / VBS (przed restartem):" -ForegroundColor Yellow
Get-CimInstance -ClassName Win32_DeviceGuard | Select-Object SecurityServicesConfigured,SecurityServicesRunning,VirtualizationBasedSecurityStatus | Format-List

Write-Host "`n[Ważne] Zmiany wymagają pełnego restartu systemu." -ForegroundColor Green
$ans = Read-Host "Zrestartować teraz? (T/N)"
if ($ans -match '^[TtYy]') {
  Write-Host "Restart za 5 sekund..." -ForegroundColor Green
  shutdown /r /t 5
} else {
  Write-Host "Pamiętaj, aby zrestartować komputer ręcznie, zanim uruchomisz QEMU z WHPX." -ForegroundColor Yellow
}






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

