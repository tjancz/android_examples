& "C:\Program Files\qemu\qemu-system-x86_64.exe" `
  -machine type=pc,accel=whpx `
  -cpu host `
  -m 8192 -smp 4 `
  -display sdl `
  -drive "if=none,file=C:\qemu\ubuntu2404.qcow2,format=qcow2,id=vd0,cache=writeback" `
  -device virtio-blk-pci,drive=vd0 `
  -drive "if=none,media=cdrom,file=C:\qemu\ubuntu-24.04.3-desktop-amd64.iso,id=cd0" `
  -device ide-cd,drive=cd0 `
  -netdev "tap,id=n0,ifname=TAP-Windows Adapter V9,script=no,downscript=no" `
  -device virtio-net-pci,netdev=n0 `
  -boot order=d





& "C:\Program Files\qemu\qemu-system-x86_64.exe" `
  -machine type=pc,accel=whpx `
  -cpu host `
  -m 8192 -smp 4 `
  -display sdl `
  -drive "if=none,file=C:\qemu\ubuntu2404.qcow2,format=qcow2,id=vd0,cache=writeback" `
  -device virtio-blk-pci,drive=vd0 `
  -netdev "tap,id=n0,ifname=TAP-Windows Adapter V9,script=no,downscript=no" `
  -device virtio-net-pci,netdev=n0 `
  -boot order=c



  & "C:\Program Files\qemu\qemu-system-x86_64.exe" -m 4096 -smp 2 `
  -display sdl `
  -drive "if=none,file=C:\qemu\ubuntu2404.qcow2,format=qcow2,id=vd0" `
  -device virtio-blk-pci,drive=vd0 `
  -drive "if=none,media=cdrom,file=C:\qemu\ubuntu-24.04.3-desktop-amd64.iso,id=cd0" `
  -device ide-cd,drive=cd0 `
  -boot order=d
