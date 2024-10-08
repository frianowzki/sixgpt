# sixgpt Miner

### We're gonna run sixgpt Miner by Vana via VPS
#
#
#
##
##
#### The minimum specs requirements to run is:
- #### 4 vCPU
- #### 8GB of RAM
- #### 100GB Disk Space
#

- #### Request faucet first from here: https://faucet.vana.org/satori

- #### Update and install Docker 

````
bash <(curl -s https://raw.githubusercontent.com/frianowzki/installer/main/docker.sh)
````
#
- #### Create sixgpt folder

````
mkdir sixgpt
cd sixgpt
````
#
- #### Export your private keys & network

```
bash <(curl -s https://raw.githubusercontent.com/frianowzki/sixgpt/main/import.sh)
```
#
*Change $VANA_PRIVATE_KEY with your private keys*

*Change $VANA_NETWORK with satori*
#
- #### Now add this 
```
cat <<EOF > docker-compose.yml
version: '3.8'

services:
  ollama:
    image: ollama/ollama:0.3.12
    ports:
      - "11435:11434"
    volumes:
      - ollama:/root/.ollama
    restart: unless-stopped
 
  sixgpt3:
    image: sixgpt/miner:latest
    ports:
      - "3015:3000"
    depends_on:
      - ollama
    environment:
      - VANA_PRIVATE_KEY=\${VANA_PRIVATE_KEY}
      - VANA_NETWORK=\${VANA_NETWORK}
    restart: always

volumes:
  ollama:
EOF
```
#
- #### And now lets run this!

```
docker compose up -d
```
#
- #### You can check the logs with 

```
docker compose logs -fn 100
```
#
- #### If you want to stop and delete sixgpt, run this
```
docker stop <container-id> && docker rm <container-id>
```
```
rm -rf sixgpt
```



