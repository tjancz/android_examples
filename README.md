# Włącz RDP
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server' -Name fDenyTSConnections -Value 0

# Włącz Network Level Authentication (NLA)
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name UserAuthentication -Value 1

# Dodaj użytkownika do grupy "Remote Desktop Users" (opcjonalnie)
net localgroup "Remote Desktop Users" "$env:USERNAME" /add

# Upewnij się, że usługa RDP działa
Set-Service -Name TermService -StartupType Automatic
Start-Service -Name TermService
