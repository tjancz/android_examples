powershell.exe -ExecutionPolicy Bypass -File .\antiidle.ps1

& "C:\Program Files\qemu\qemu-system-x86_64.exe" `
  -machine type=pc,accel=whpx -cpu host -m 4096 -smp 2 -display sdl `
  -drive "if=none,file=C:\qemu\ubuntu2404.qcow2,format=qcow2,id=vd0" `
  -device virtio-blk-pci,drive=vd0 `
  -netdev user,id=n0 -device virtio-net-pci,netdev=n0


  
