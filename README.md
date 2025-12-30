# Project Nebula

A high-performance, minimal Linux distribution built from scratch for the Alienware x16 R2 platform.

## 🚀 CURRENT STATUS: Sprint 1 Complete
- Kernel: Linux 6.18.2-zen (Custom "Nebula-v1" build)
- Environment: Minimal BusyBox static userland
- Boot Performance: ~1.9s (QEMU/KVM)
- Hardware Target: Intel i9-185H (22-core / 20-thread utilized) + RTX 4090

## 📂 PROJECT ARCHITECTURE
- linux-zen/: Optimized Zen kernel source code
- busybox/: Minimalist toolset source (Static build)
- dist/: Root Filesystem (RootFS) skeleton
  - bin/: Static binaries (BusyBox)
  - init: Custom Boot sequence script (PID 1)
- nebula-v1.iso: Bootable "Live" image for physical hardware/VMs
- nebula_storage.img: 2GB Persistent Virtual SSD (RAW Image)
- env_setup.sh: Environment exports for i9 thread count (-j20)

## 🛠 DEPLOYMENT & TESTING COMMANDS

### 1. Virtual Test (WSL2 / QEMU)
To test the kernel and initramfs directly in your terminal:
qemu-system-x86_64 -kernel linux-zen/arch/x86/boot/bzImage -initrd initramfs.cpio.gz -m 1G -cpu host -enable-kvm -nographic -append "console=ttyS0"

### 2. Physical Test (Live USB via Rufus)
- Step A (Generate ISO): 
  grub-mkrescue -o nebula-v1.iso iso_root
- Step B (Copy to Windows Desktop): 
  cp nebula-v1.iso /mnt/c/Users/<Windows_User>/Desktop/
- Step C (Flash): 
  Use Rufus (GPT/UEFI). Disable "Secure Boot" in BIOS (F2). Boot via F12.

## 🔐 SECURITY & PERSISTENCE (Sprint 2 Goal)
- Encryption: LUKS-based volume encryption for user data partitions.
- Auth: Multi-factor passphrase required at boot sequence.
- Mount Logic: Automount routine for /nebula/storage via init script.

## ⏭ FUTURE ROADMAP
1. Sprint 2: Format nebula_storage.img and implement volume encryption.
2. Sprint 3: Build specialized compatibility layer (Wine/Proton) for gaming.
3. Sprint 4: Implement hardware-accelerated GUI (Wayland).
