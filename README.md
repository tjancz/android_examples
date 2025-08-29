powershell.exe -ExecutionPolicy Bypass -File .\antiidle.ps1

Get-NetAdapter -IncludeHidden |
  Where-Object {$_.InterfaceDescription -like "*TAP-Windows Adapter*"} |
  Format-Table Name, InterfaceDescription, Status, InterfaceGuid


  & "C:\Program Files\qemu\qemu-system-x86_64.exe" `
  -machine type=pc,accel=whpx -cpu host -m 8192 -smp 4 -display sdl `
  -drive "if=none,file=C:\qemu\ubuntu2404.qcow2,format=qcow2,id=vd0,cache=writeback" `
  -device virtio-blk-pci,drive=vd0 `
  -drive "if=none,media=cdrom,file=C:\qemu\ubuntu-24.04.3-desktop-amd64.iso,id=cd0" `
  -device ide-cd,drive=cd0 `
  -netdev "tap,id=n0,ifname=Ethernet 3,script=no,downscript=no" `
  -device virtio-net-pci,netdev=n0 `
  -boot order=d

  
