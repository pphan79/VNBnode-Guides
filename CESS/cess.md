# Storage Miner
<img src="https://github.com/vnbnode/VNBnode-Guides/assets/76662222/23c3d2d7-f8e2-493b-bbc3-7c348fde2d6e" width="30"/> <a href="https://discord.gg/cess" target="_blank">Discord</a>
## Mining  preparation
The OS requirement of the Storage server is as following: 
|   SPEC	     |        Recommend          |
| :---------: | :-----------------------: |
|   **CPU**   |        ≥ 4 Cores          |
|   **RAM**   |        ≥ 8 GB             |
| **Storage** |        ≥ 100 GB SSD       |
| **NETWORK** |        ≥ 50 Mbps          |
## Firewall configuration
```
ufw allow 4001
```
## Disk mounting (If VPS please skip formatting the hard drive. Go to Install cess-nodeadm)
Check the hard disk status using the df -h command:
```
df -h
```
Note: `Min 300GB HDD or SSD`

The disk is not mounted If the hard drive for storage mining cannot be found. Use the command below to view unmounted hard disks:
```
fdisk -l
```
```
Disk /dev/sdb: 900 GiB, 214748364800 bytes, 419430400 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x331195d1
```
The unmounted disk is /dev/sdb in the demonstration above. We will be using /dev/sdb to demonstrate the mounting operation.
Allocate the /dev/sdb disk：
```
fdisk /dev/sdb
Enter and press Enter:
n
p
1
2048
the value after default
w
```
Format the newly divided disk into ext4 format：
```
mkfs.ext4 /dev/sdb
```
Enter "y" to continue if the system asks to proceed:
```
Proceed anyway? (y,N) y
```
Create /cess directory to mount the disk. Using/cess as an example:
```
mkdir /cess
echo "/dev/sdb /cess ext4 defaults 0 0" >> /etc/fstab
```
Mount /cess :
```
mount -a
```
Check the disk mounting status:
```
df -h
```
If the "/cess" appers, the disk has been successfully mounted.
## Prepare CESS Account
Please refer to the [CESS Account](https://docs.cess.cloud/cess-build-book/cess-accounts) section above for creating a CESS account. 
Please go to the [CESS faucet](https://testnet-faucet.cess.cloud/) to get TCESS. 
## Install cess-nodeadm
### Option VPS:
```
mkdir /cess
```
### 1. Install
```
wget https://github.com/CESSProject/cess-nodeadm/archive/v0.5.3.tar.gz
tar -xvf v0.5.3.tar.gz
cd cess-nodeadm-0.5.3
./install.sh
```
### 2. Set up Configuration
Set up bucket configuration
```
cess config set
```
```
# cess config set 
Enter cess node mode from 'authority/storage/watcher' (current: authority, press enter to skip): storage
Enter cess storage listener port (current: 15001, press enter to skip): enter
Enter cess storage earnings account: cXiHsw32kT3Fzw6YeXDTECCfFNKjDVg85eg......
Enter cess storage staking signature phrase: shoe ...... creek metal avoid
Enter cess storage disk path (default: /opt/cess/storage/disk): /cess
Enter cess storage space, by GB unit (current: 300, press enter to skip): 1000
Enter the number of CPU cores used for mining; Your CPU cores are (current: 0, 0 means all cores are used; press enter to skip): enter
Enter the staker's payment account if you have one (press enter to skip): enter
Enter the reserved TEE worker endpoints (separate multiple values with commas, press enter to skip): enter
Set configurations successfully
```
### 3. Start bucket
```
cess start
```
The following should be seen:
```
[+] Running 3/0
 ✔ Container chain       Running                                                0.0s 
 ✔ Container bucket      Running                                                0.0s 
 ✔ Container watchtower  Running                                                0.0s 
```
### 4. Check Chain Sync Status
```
docker logs -f chain
```
![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/eb94274b-d003-449e-83d5-9a1375b3265c)

```
docker logs -f bucket
```
![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/8379f2f0-8be6-461f-8ab2-d53758007dff)

### 5. View bucket status
```
cess bucket stat
```
![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/9774b7c6-5adf-4836-9d21-191532853604)

## [Check Stat](https://substats.cess.cloud/)
## Thank to support VNBnode.
### Visit us at:

<img src="https://user-images.githubusercontent.com/50621007/183283867-56b4d69f-bc6e-4939-b00a-72aa019d1aea.png" width="30"/> <a href="https://t.me/VNBnodegroup" target="_blank">VNBnodegroup</a>

<img src="https://user-images.githubusercontent.com/50621007/183283867-56b4d69f-bc6e-4939-b00a-72aa019d1aea.png" width="30"/> <a href="https://t.me/Vnbnode" target="_blank">VNBnode News</a>

<img src="https://github.com/vnbnode/binaries/blob/main/Logo/VNBnode.jpg" width="30"/> <a href="https://VNBnode.com" target="_blank">VNBnode.com</a>
