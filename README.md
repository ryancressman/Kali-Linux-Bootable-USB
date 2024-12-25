# Kali-Linux-Bootable-USB
# Bootable USB with Kali Linux and Persistence Using BalenaEtcher

This guide explains how to create and use a bootable USB drive with Kali Linux, configured for persistence, using BalenaEtcher. Persistence allows you to save changes and files across reboots, making it an ideal tool for portable penetration testing and system administration.

---

## Requirements

1. A USB drive with at least **8GB** of storage (recommended: 16GB or more).
2. A **Kali Linux ISO** file. Download the latest version from the [Kali Linux official website](https://www.kali.org/get-kali/).
3. The **BalenaEtcher** application for creating bootable drives. Download it from [balena.io](https://www.balena.io/etcher/).

---

## Steps to Create a Bootable USB with Persistence

### 1. Download Necessary Tools
- Download the **Kali Linux ISO**.
- Install **BalenaEtcher** on your system.

### 2. Create the Bootable USB
1. Open **BalenaEtcher**.
2. Click **Flash from File** and select the Kali Linux ISO file.
3. Insert your USB drive and select it under the "Select Target" section.
4. Click **Flash** to start the process.
5. Wait for the flashing process to complete. Once done, your USB drive will be ready to boot into Kali Linux.

---

## Setting Up Persistence

### Step 1: Partition the USB Drive
1. Boot into the **Live USB** mode (without persistence).
2. Open a terminal and identify your USB drive using:
`lsblk`
Look for the device name (e.g., `/dev/sdb`).

3. Create a new partition for persistence using **fdisk** or **gparted**:
`sudo fdisk /dev/sdb`
- Create a new partition using the remaining unallocated space.
- Format the partition as ext4:
  ```
  sudo mkfs.ext4 -L persistence /dev/sdb2
  ```

### Step 2: Configure Persistence
1. Mount the new partition:
`sudo mount /dev/sdb2 /mnt`
2. Create a persistence configuration file:
`echo "/ union" | sudo tee /mnt/persistence.conf`
3. Unmount the partition:
`sudo umount /mnt`

### Step 3: Boot into Persistence Mode
1. Restart your system and boot from the USB.
2. Select **Live USB Persistence** from the boot menu.
3. Your changes will now be saved across reboots.

---

## Using Your Persistent USB

1. **To Save Files**: Any files or settings saved in the persistence partition will remain after reboot.
2. **Updates and Installations**: You can install new tools and packages, and they will be retained.
3. **Limitations**: Be mindful of the available space on the persistence partition.

---

## Troubleshooting

1. **USB Not Booting**: Ensure your BIOS/UEFI settings are configured to boot from USB.
2. **Persistence Not Working**: Verify that the persistence partition is correctly configured with the `/union` entry in `persistence.conf`.
3. **Insufficient Space**: Resize the persistence partition or use a larger USB drive.

---

## Resources

- [Kali Linux Documentation](https://www.kali.org/docs/)
- [BalenaEtcher Official Website](https://www.balena.io/etcher/)

---

## Notes

- This setup is ideal for testing and learning in a portable environment. Avoid using it as a primary operating system for critical tasks.
- Always back up important data before modifying the USB drive or its partitions.
