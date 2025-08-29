cd C:\qemu
qemu-img create -f qcow2 ubuntu2404.qcow2 40G


qemu-system-x86_64 ^
  -m 4096 ^
  -smp 4 ^
  -cdrom C:\qemu\isos\ubuntu-24.04-desktop-amd64.iso ^
  -drive file=C:\qemu\ubuntu2404.qcow2,format=qcow2 ^
  -boot d ^
  -nic tap,ifname=TAP-Windows Adapter V9,script=no,downscript=no ^
  -net nic,model=virtio ^
  -enable-kvm ^
  -cpu host ^
  -display sdl



qemu-system-x86_64 ^
  -m 4096 ^
  -smp 4 ^
  -drive file=C:\qemu\ubuntu2404.qcow2,format=qcow2 ^
  -net nic,model=virtio ^
  -nic tap,ifname=TAP-Windows Adapter V9,script=no,downscript=no ^
  -display sdl


