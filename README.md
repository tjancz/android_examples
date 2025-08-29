Add-Type -AssemblyName System.Windows.Forms
Add-Type -AssemblyName System.Drawing

while ($true) {
    # 1. Delikatny ruch myszki (o 1 piksel w lewo i prawo)
    $pos = [System.Windows.Forms.Cursor]::Position
    [System.Windows.Forms.Cursor]::Position = New-Object System.Drawing.Point ($pos.X + 1), $pos.Y
    Start-Sleep -Milliseconds 200
    [System.Windows.Forms.Cursor]::Position = $pos

    # 2. Symulacja naciśnięcia klawisza SHIFT (nieszkodliwe)
    $signature = @'
    [DllImport("user32.dll")]
    public static extern void keybd_event(byte bVk, byte bScan, uint dwFlags, int dwExtraInfo);
'@
    Add-Type -MemberDefinition $signature -Name "Win32Keyboard" -Namespace Win32Functions
    $VK_SHIFT = 0x10
    $KEYEVENTF_KEYUP = 0x02

    [Win32Functions.Win32Keyboard]::keybd_event($VK_SHIFT, 0, 0, 0)       # wciśnięcie
    [Win32Functions.Win32Keyboard]::keybd_event($VK_SHIFT, 0, $KEYEVENTF_KEYUP, 0) # puszczenie

    # Poczekaj 60 sekund i powtórz
    Start-Sleep -Seconds 60
}

powershell.exe -ExecutionPolicy Bypass -File .\antiidle.ps1
