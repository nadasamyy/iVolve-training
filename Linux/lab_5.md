# Lab 5: Attach a 15GB disk to your VM, partition it into 5GB, 5GB, 3GB, and 2GB sections. Use the first 5GB partition as a file system, configure the 2GB partition as swap, initialize the second 5GB as a Volume Group (VG) with a Logical Volume (LV), then extend the LV by adding the 3GB partition.

## steps

### 1. First, verify the new disk:
```bash
# List block devices
lsblk
sudo parted -l
```

### 2. Create GPT partition table and partitions:
```bash
# Create GPT partition table
sudo parted -s /dev/xvdf mklabel gpt

# Create partitions with names
sudo parted -s /dev/xvdf mkpart data1 ext4 1MiB 5GiB
sudo parted -s /dev/xvdf mkpart lvm1 5GiB 10GiB
sudo parted -s /dev/xvdf mkpart lvm2 10GiB 13GiB
sudo parted -s /dev/xvdf mkpart swap1 13GiB 100%

# Set LVM flags
sudo parted -s /dev/xvdf set 2 lvm on
sudo parted -s /dev/xvdf set 3 lvm on
```

### 3. Create filesystem on first partition:
Purpose of Mounting:
- Makes the filesystem accessible in the Linux directory tree
- Allows users and applications to read/write files
- Integrates the new storage into the existing file hierarchy

Think of it like this:
- A filesystem is like a book with content
- Mount point is like a shelf in your library
- Mounting is placing the book (filesystem) on a specific shelf (directory)

```bash
# Create ext4 filesystem
sudo mkfs.ext4 /dev/xvdf1

# Create mount point and mount
sudo mkdir -p /mnt/data1
sudo mount /dev/xvdf1 /mnt/data1

# Add to fstab using UUID
UUID=$(sudo blkid -s UUID -o value /dev/xvdf1)
echo "UUID=$UUID /mnt/data1 ext4 defaults 0 0" | sudo tee -a /etc/fstab
```

### 4. Configure swap partition:
```bash
# Create swap
sudo mkswap /dev/xvdf4      # Format partition as swap space
sudo swapon /dev/xvdf4      # Activate the swap space

# Add to fstab using UUID
UUID=$(sudo blkid -s UUID -o value /dev/xvdf4)
echo "UUID=$UUID none swap sw 0 0" | sudo tee -a /etc/fstab
```

### 5. Set up LVM:
```bash
# Create physical volumes
sudo pvcreate /dev/xvdf2
sudo pvcreate /dev/xvdf3

# Create volume group with first PV
sudo vgcreate vg_data /dev/xvdf2

# Create logical volume
sudo lvcreate -n lv_data -l 100%FREE vg_data

# Create filesystem on LV
sudo mkfs.ext4 /dev/vg_data/lv_data

# Mount LV
sudo mkdir -p /mnt/lvdata
sudo mount /dev/vg_data/lv_data /mnt/lvdata

# Add to fstab using device mapper path
echo '/dev/mapper/vg_data-lv_data /mnt/lvdata ext4 defaults 0 0' | sudo tee -a /etc/fstab
```

### 6. Extend LVM with third partition:
```bash
# Extend volume group
sudo vgextend vg_data /dev/xvdf3

# Extend logical volume
sudo lvextend -l +100%FREE /dev/vg_data/lv_data

# Resize filesystem within the logical volume
sudo resize2fs /dev/vg_data/lv_data
```

### 7. Verify setup:
```bash
# Check partitions and GPT table
sudo parted /dev/xvdf print
lsblk
sudo gdisk -l /dev/xvdf

# Check filesystems
df -h

# Check LVM
sudo pvs
sudo vgs
sudo lvs

# Check swap
swapon --show

# Check UUIDs
sudo blkid
```

---

## 📄 License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## ✍️ Author
**King Memo**

## 🙏 Thank You!
Thank you for using this project. Your support and feedback are greatly appreciated!
