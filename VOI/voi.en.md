# <p align="center"> VOI Network </p>
<p align="center">
  <img height="100" height="auto" src="https://github.com/vnbnode/binaries/blob/main/Projects/VOI/voi.jpg?raw=true">
</p>

### Recommended Hardware Requirements

|   SPEC      |        Recommend          |
| :---------: | :-----------------------: |
|   **CPU**   | 4 Cores 8 threads (ARM64 or x86-64)                                   |
|   **RAM**   |        16 GB (DDR4)       |
|   **SSD**   |    100 GB SSD or NVME     |
| **NETWORK** |        100 Mbps           |

## Option 1: Automatic
### STEP 1: Install
```
echo "export ALGORAND_DATA=/var/lib/algorand/" >> $HOME/.bashrc && source $HOME/.bashrc
```
```
cd $HOME && curl -o voi-auto.sh https://raw.githubusercontent.com/vnbnode/binaries/main/Projects/VOI/voi-auto.sh && bash voi-auto.sh
```
- Wait Catchpoint downloaded blocks = Catchpoint total blocks

![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/f309da6d-6801-44db-9618-f53dc48ff686)

### STEP 2: Create or Recovery Wallet, Account Address
```
cd $HOME && curl -o voi-create.sh https://raw.githubusercontent.com/vnbnode/binaries/main/Projects/VOI/voi-create.sh && bash voi-create.sh
```
### STEP 3: Set Node Name
```
sudo ALGORAND_DATA=/var/lib/algorand diagcfg telemetry name -n Nodename

sudo ALGORAND_DATA=/var/lib/algorand diagcfg telemetry enable &&\
sudo systemctl restart voi
```
- Save GUID

### STEP 4: Create Participation
```
cd $HOME && curl -o participation.sh https://raw.githubusercontent.com/vnbnode/binaries/main/Projects/VOI/participation.sh && bash participation.sh
```
### STEP 5: Faucet
- [Faucet Here]([https://discord.gg/voinetwork](https://discord.gg/voi-network-1055863853633785857))

- Get Role

![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/16166601-63da-4b0b-8e62-1f725ee11cf4)

- Use /voi-testnet-faucet

![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/e76d238a-e35f-495f-aacd-5685409d40ee)

### STEP 6: Register and Active
```
cd $HOME && curl -o register-active-auto.sh https://raw.githubusercontent.com/vnbnode/binaries/main/Projects/VOI/register-active-auto.sh && bash register-active-auto.sh
```
### STEP 7: Faucet again

![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/2d667095-ecc9-45d8-9904-b1468ee37682)

- Fill the GUID you saved in Step 3
- `NOTE`: Node must run for more than 4-6 hours to be able to faucet
## Option 2: Manual

### 1/ Need install software and its updates
```
sudo apt install -y jq bc gnupg2 curl software-properties-common
curl -o - https://releases.algorand.com/key.pub | sudo tee /etc/apt/trusted.gpg.d/algorand.asc
sudo add-apt-repository "deb [arch=amd64] https://releases.algorand.com/deb/ stable main"
```
![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/1fb9bf1d-8d3c-4ee8-9def-ba151d1b56b9)
- Select ENTER
### 2/ Install Node Algorand
```
sudo apt update && sudo apt install -y algorand && echo OK
```
![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/207725f8-405f-4ba6-80b4-a222599d7655)

### 3/ Stop Node Algorand
```
sudo systemctl stop algorand && sudo systemctl disable algorand && echo OK
```
![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/dd2c268e-e2d9-4da5-b7cd-6be4a1f0265e)

### 4/ Setup goal
```
echo -e "\nexport ALGORAND_DATA=/var/lib/algorand/" >> ~/.bashrc && source ~/.bashrc && echo OK
```
![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/dac3b36d-fd09-48c4-9f11-56bb43d697b1)

### 5/ Configure Node VOI
```
sudo algocfg set -p DNSBootstrapID -v "<network>.voi.network" -d /var/lib/algorand/ &&\
sudo algocfg set -p EnableCatchupFromArchiveServers -v true -d /var/lib/algorand/ &&\
sudo chown algorand:algorand /var/lib/algorand/config.json &&\
sudo chmod g+w /var/lib/algorand/config.json &&\
echo OK
```
![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/f244f008-459b-4897-b982-a58e83700c68)

```
sudo curl -s -o /var/lib/algorand/genesis.json https://testnet-api.voi.nodly.io/genesis &&\
sudo chown algorand:algorand /var/lib/algorand/genesis.json &&\
echo OK
```
![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/dcdb4b0b-62a3-4037-b336-940f83bf7253)

```
sudo cp /lib/systemd/system/algorand.service /etc/systemd/system/voi.service &&\
sudo sed -i 's/Algorand daemon/Voi daemon/g' /etc/systemd/system/voi.service &&\
echo OK
```
![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/f11b9f70-1593-4ca6-9ff5-381646675908)

### 6/ Start Node VOI
```
sudo systemctl start voi && sudo systemctl enable voi && echo OK
```
![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/a2df2cd1-eee5-40c1-83ea-270ab8ba08be)

### 7/ Check Status
```
goal node status
```
![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/1bce48a2-7bd7-4633-8005-10ee659c59bc)

### 8/ Get catch up with the network
```
goal node catchup $(curl -s https://testnet-api.voi.nodly.io/v2/status|jq -r '.["last-catchpoint"]') &&\
echo OK
```
![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/9821e6d3-7ae1-4260-8732-c45ec0a02559)

### 9/ Wait for catchup to complete:
```
goal node status -w 1000
```
![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/0d3175e8-62bc-44ad-a525-fee67ad490f1)

### 10/ Enable Telemetry Node Name
```
sudo ALGORAND_DATA=/var/lib/algorand diagcfg telemetry name -n Nodename
```
![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/e7200b6c-edf9-4b9a-991c-ace43e3b8068)
```
sudo ALGORAND_DATA=/var/lib/algorand diagcfg telemetry enable &&\
sudo systemctl restart voi
```
![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/2146fb6a-a1f0-4d3d-a40c-75ce471b33eb)
- Save GUID
### 11/ Create Wallet
```
goal wallet new voi
```
![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/5e6f59ac-6665-4714-acfd-98a139aecff4)
- Save Your backup phrase
### 12/ To create a new account address
```
goal account new
```
![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/f3e82408-1206-4164-be25-f04e2bea5c13)
- Save Address
### 13/ Goal account export
```
echo -ne "\nEnter your voi address: " && read addr &&\
goal account export -a $addr
```
- Fill Address

![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/401f8fa0-8eca-4c49-a30c-d637bc36d475)
- Save Exported key
### 14/ Generate your participation keys:
```
getaddress() {
  if [ "$addr" == "" ]; then echo -ne "\nNote: Completing this will remember your address until you log out. "; else echo -ne "\nNote: Using previously entered address. "; fi; echo -e "To forget the address, press Ctrl+C and enter the command:\n\tunset addr\n";
  count=0; while ! (echo "$addr" | grep -E "^[A-Z2-7]{58}$" > /dev/null); do
    if [ $count -gt 0 ]; then echo "Invalid address, please try again."; fi
    echo -ne "\nEnter your voi address: "; read addr;
    addr=$(echo "$addr" | sed 's/ *$//g'); count=$((count+1));
  done; echo "Using address: $addr"
}
getaddress &&\
echo -ne "\nEnter duration in rounds [press ENTER to accept default)]: " && read duration &&\
start=$(goal node status | grep "Last committed block:" | cut -d\  -f4) &&\
duration=${duration:-2000000} &&\
end=$((start + duration)) &&\
dilution=$(echo "sqrt($end - $start)" | bc) &&\
goal account addpartkey -a $addr --roundFirstValid $start --roundLastValid $end --keyDilution $dilution
```
- Fill: 8000000

![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/4d1815d1-d2c6-49ed-b37c-03b980acd439)
- Wait Create Key Your Participation
### 15/ Check your participation status
```
getaddress() {
  if [ "$addr" == "" ]; then echo -ne "\nNote: Completing this will remember your address until you log out. "; else echo -ne "\nNote: Using previously entered address. "; fi; echo -e "To forget the address, press Ctrl+C and enter the command:\n\tunset addr\n";
  count=0; while ! (echo "$addr" | grep -E "^[A-Z2-7]{58}$" > /dev/null); do
    if [ $count -gt 0 ]; then echo "Invalid address, please try again."; fi
    echo -ne "\nEnter your voi address: "; read addr;
    addr=$(echo "$addr" | sed 's/ *$//g'); count=$((count+1));
  done; echo "Using address: $addr"
}
getaddress &&\
goal account dump -a $addr | jq -r 'if (.onl == 1) then "You are online!" else "You are offline." end'
```
![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/1ee723fd-da78-449e-bfb2-c083e5d55ba8)

### 16/ Faucet
- [Faucet Here]([https://discord.gg/voinetwork](https://discord.gg/voi-network-1055863853633785857))
- Get Role

![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/16166601-63da-4b0b-8e62-1f725ee11cf4)
- Use /voi-testnet-faucet

![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/e76d238a-e35f-495f-aacd-5685409d40ee)
### 17/ Register and Active Participating
```
getaddress() {
  if [ "$addr" == "" ]; then echo -ne "\nNote: Completing this will remember your address until you log out. "; else echo -ne "\nNote: Using previously entered address. "; fi; echo -e "To forget the address, press Ctrl+C and enter the command:\n\tunset addr\n";
  count=0; while ! (echo "$addr" | grep -E "^[A-Z2-7]{58}$" > /dev/null); do
    if [ $count -gt 0 ]; then echo "Invalid address, please try again."; fi
    echo -ne "\nEnter your voi address: "; read addr;
    addr=$(echo "$addr" | sed 's/ *$//g'); count=$((count+1));
  done; echo "Using address: $addr"
}
getaddress &&\
goal account changeonlinestatus -a $addr -o=1 &&\
sleep 1 &&\
goal account dump -a $addr | jq -r 'if (.onl == 1) then "You are online!" else "You are offline." end'
```
### 18/ Faucet again

![image](https://github.com/vnbnode/VNBnode-Guides/assets/76662222/2d667095-ecc9-45d8-9904-b1468ee37682)

- Fill the GUID you saved in Step 10
- `NOTE`: Node must run for more than 4-6 hours to be able to faucet
### 19/ Check logs
```
tail -f /var/lib/algorand/node.log
```
### 20/ Check
- Explorer: https://voi.observer/explorer/home    
- https://cswenor.github.io/voi-proposer-data/health.html
- https://voi-node-info.boeieruurd.com/
- https://voirewards.com/

## Upgrade new version
```
source ~/.bashrc
sudo apt update
sudo systemctl stop voi
sudo apt install algorand
sudo systemctl start voi
goal version -v
goal node status
sudo systemctl status voi
```

## Thank to support VNBnode.
### Visit us at:

<img src="https://user-images.githubusercontent.com/50621007/183283867-56b4d69f-bc6e-4939-b00a-72aa019d1aea.png" width="30"/> <a href="https://t.me/VNBnodegroup" target="_blank">VNBnodegroup</a>

<img src="https://user-images.githubusercontent.com/50621007/183283867-56b4d69f-bc6e-4939-b00a-72aa019d1aea.png" width="30"/> <a href="https://t.me/Vnbnode" target="_blank">VNBnode News</a>

<img src="https://github.com/vnbnode/binaries/blob/main/Logo/VNBnode.jpg" width="30"/> <a href="https://VNBnode.com" target="_blank">VNBnode.com</a>
