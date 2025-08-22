# Wyłącz standardowe szerokie reguły RDP
Get-NetFirewallRule -DisplayGroup "Remote Desktop" | Disable-NetFirewallRule

# Utwórz regułę dla RDP (TCP 3389) tylko na interfejsie ZeroTier
New-NetFirewallRule `
  -DisplayName "RDP over ZeroTier (TCP)" `
  -Direction Inbound -Action Allow -Enabled True `
  -Protocol TCP -LocalPort 3389 `
  -Profile Private `
  -InterfaceAlias "ZeroTier One*"

# (opcjonalnie) jeśli używasz też UDP dla RDP (nowe wersje to obsługują)
New-NetFirewallRule `
  -DisplayName "RDP over ZeroTier (UDP)" `
  -Direction Inbound -Action Allow -Enabled True `
  -Protocol UDP -LocalPort 3389 `
  -Profile Private `
  -InterfaceAlias "ZeroTier One*"




Get-NetIPConfiguration -InterfaceAlias "ZeroTier One*"
