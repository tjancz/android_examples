powershell.exe -ExecutionPolicy Bypass -File .\antiidle.ps1

& "C:\Program Files\qemu\qemu-system-x86_64.exe" `
  -machine type=pc,accel=whpx `
  -cpu host `
  -m 8192 -smp 4 `
  -display sdl `
  -drive "if=none,file=C:\qemu\ubuntu2404.qcow2,format=qcow2,id=vd0,cache=writeback" `
  -device virtio-blk-pci,drive=vd0 `
  -drive "if=none,media=cdrom,file=C:\qemu\ubuntu-24.04.3-desktop-amd64.iso,id=cd0" `
  -device ide-cd,drive=cd0 `
  -netdev user,id=n0 `
  -device virtio-net-pci,netdev=n0 `
  -boot order=d



& "C:\Program Files\qemu\qemu-system-x86_64.exe" `
  -machine type=pc,accel=whpx `
  -cpu host `
  -m 8192 -smp 4 `
  -display sdl `
  -drive "if=none,file=C:\qemu\ubuntu2404.qcow2,format=qcow2,id=vd0,cache=writeback" `
  -device virtio-blk-pci,drive=vd0 `
  -netdev user,id=n0 `
  -device virtio-net-pci,netdev=n0 `
  -boot order=c



  dism.exe /online /Get-Features | findstr Hyper

  & "C:\Program Files\qemu\qemu-system-x86_64.exe" `
  -m 4096 -smp 2 -cpu host `
  -drive "if=none,file=C:\qemu\ubuntu2404.qcow2,format=qcow2,id=vd0" `
  -device virtio-blk-pci,drive=vd0 `
  -drive "if=none,media=cdrom,file=C:\qemu\ubuntu-24.04.3-desktop-amd64.iso,id=cd0" `
  -device ide-cd,drive=cd0 `
  -netdev user,id=n0 -device virtio-net-pci,netdev=n0 `
  -boot order=d




New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard" -Name EnableVirtualizationBasedSecurity -PropertyType DWord -Value 0 -Force
New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Lsa" -Name LsaCfgFlags -PropertyType DWord -Value 0 -Force
