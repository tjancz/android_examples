powershell.exe -ExecutionPolicy Bypass -File .\antiidle.ps1

# ======================================================================
# disable-vbs.ps1
# Wyłącza Hyper-V, Virtualization Based Security i Credential Guard
# dla pełnej zgodności z QEMU + WHPX na Windows 11.
# ======================================================================

Write-Host "[*] Wyłączanie Hyper-V..." -ForegroundColor Cyan
dism.exe /Online /Disable-Feature:Microsoft-Hyper-V-All /NoRestart

Write-Host "[*] Wyłączanie Virtualization Based Security (VBS)..." -ForegroundColor Cyan
reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard" /v EnableVirtualizationBasedSecurity /t REG_DWORD /d 0 /f

Write-Host "[*] Wyłączanie Credential Guard (LsaCfgFlags=0)..." -ForegroundColor Cyan
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /v LsaCfgFlags /t REG_DWORD /d 0 /f

Write-Host "[*] Wyłączanie HypervisorLaunchType (BCDEdit)..." -ForegroundColor Cyan
bcdedit /set hypervisorlaunchtype off

Write-Host "`n[*] Zmiany zostały wprowadzone. Zrestartuj system, aby weszły w życie." -ForegroundColor Green

Write-Host "`n[*] Aktualny status (przed restartem):" -ForegroundColor Yellow
systeminfo | findstr /i "Virtualization"


Set-ExecutionPolicy Bypass -Scope Process -Force


C:\Scripts\disable-vbs.ps1


systeminfo | findstr /i "Virtualization"

