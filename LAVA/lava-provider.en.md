### Before start install Lava provider, you must ensure you have:
* Your own domain (If you do not have yet, consider to buy one: [namecheap](https://www.namecheap.com/)
* Lava node running
* Lavap binaries installed
#### Step 1: Setup A record for your domain

##### Go to DNS setting:
* Create A record: Host = LAVA; Value = Your server public IP.

#### Step 2: Install Required Dependencies
```php
sudo apt update
sudo apt install certbot net-tools nginx python3-certbot-nginx -y
```
#### Step 3: Generate Certificate
```php
sudo certbot certonly -d yourdomain.com -d LAVA.yourdomain.com
```
***Select 1 for Nginx Web Server Plugin***

***Enter your valid email***
#### Step 4: Validate Certificate
```php
sudo certbot certificates
```
![image](https://github.com/vnbnode/VNBnode-Guides/assets/128967122/daaf691f-8ed4-46f2-b22d-563176743bee)
#### Step 5: Create Nginx server
```php
cd /etc/nginx/sites-available/
```
```php
sudo nano lava_server
```
***Copy & Paste***
```php
server {
    listen 443 ssl http2;
    server_name lava.your-domain.com;

    ssl_certificate /etc/letsencrypt/live/your-domain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/your-domain.com/privkey.pem;
    error_log /var/log/nginx/debug.log debug;

    location / {
        proxy_pass http://127.0.0.1:2224;
        grpc_pass 127.0.0.1:2224;
    }
}
```
#### Step 6: Create a link
```php
sudo ln -s /etc/nginx/sites-available/lava_server /etc/nginx/sites-enabled/lava_server
```
#### Step 7: Test Nginx Configuration
```php
sudo nginx -t
```
***Expected result***
```php
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
#### Step 8: Run Nginx
```php
sudo systemctl restart nginx
```
#### Step 9: Create Docker file
```php
cd ~/lava/config
nano lava-provider.yml
```
***Copy & Paste***
```php
endpoints:
    - api-interface: tendermintrpc
      chain-id: LAV1
      network-address:
        address: 127.0.0.1:2224
        disable-tls: true
      node-urls:
        - url: ws://127.0.0.1:26657/websocket
        - url: http://127.0.0.1:26657
    - api-interface: grpc
      chain-id: LAV1
      network-address:
        address: 127.0.0.1:2224
        disable-tls: true
      node-urls: 
        url: 127.0.0.1:9090
    - api-interface: rest
      chain-id: LAV1
      network-address:
        address: 127.0.0.1:2224
        disable-tls: true
      node-urls: 
        url: http://127.0.0.1:1317
```
#### Step 10: Create reward storage
```php
mkdir /root/.lava/config/reward/ 
```
### Step 10: Run Docker
```php
screen -S lava-provider
```
```php
lavap rpcprovider lava-provider.yml --from Adam_VNBnode  --geolocation 1 --chain-id lava-testnet-2 --reward-server-storage /root/.lava/config/reward/ --log_level debug
```
***Expected result***
![image](https://github.com/vnbnode/VNBnode-Guides/assets/128967122/8f853a72-4520-41ff-829c-692c47731c81)
#### Step 11: Test RPC provider
```php
lavap test rpcprovider --from $MONIKER  --endpoints "lava.your-domain.com:443,LAV1"
```
***Expected result***
![image](https://github.com/vnbnode/VNBnode-Guides/assets/128967122/756bfec8-d9ca-43f2-8ba3-3e518478c49a)
#### Step 12: Stake the Provider on Chain
***Make sure your wallet have enough token lava***
```php
lavap tx pairing stake-provider LAV1 "50000000000ulava" "lava.your-domain.com:443,1" 1 --from $MONIKER --provider-moniker $MONIKER --keyring-backend "test" --chain-id "lava-testnet-2" --gas="auto" --gas-adjustment "1.5" --gas "auto" --gas-prices "0.0001ulava"
```
#### Step 13: Test RPC provider again
```php
lavap test rpcprovider --from $MONIKER  --endpoints "lava.your-domain.com:443,LAV1"
```
#### Step 14: Check the status of RPC provider
[https://info.lavanet.xyz/provider/](https://info.lavanet.xyz/provider/)
***Enter your wallet address & Enjoy***
### Upgradable Lava binaries
```php
sudo systemctl stop cosmovisor
sudo systemctl stop lavap
```
```php
cd $HOME
rm -rf lava
git clone https://github.com/lavanet/lava.git
cd lava
```
```php
# This is just sample command, Please change to the correct version
git checkout v0.32.x
```
```php
export LAVA_BINARY=lavad
make install-all
```
```php
# check version
lavad version && lavap version
```
```php
sudo systemctl restart cosmovisor
sudo systemctl restart lavap
sudo systemctl status cosmovisor

```
## Thank to support VNBnode.
### Visit us at:

<img src="https://user-images.githubusercontent.com/50621007/183283867-56b4d69f-bc6e-4939-b00a-72aa019d1aea.png" width="30"/> <a href="https://t.me/VNBnodegroup" target="_blank">VNBnodegroup</a>

<img src="https://user-images.githubusercontent.com/50621007/183283867-56b4d69f-bc6e-4939-b00a-72aa019d1aea.png" width="30"/> <a href="https://t.me/Vnbnode" target="_blank">VNBnode News</a>

<img src="https://github.com/vnbnode/binaries/blob/main/Logo/VNBnode.jpg" width="30"/> <a href="https://VNBnode.com" target="_blank">VNBnode.com</a>
