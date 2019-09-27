# Minimal XMage Server based on Alpine

## Usage 1
```bash
docker run --rm -it \
	-p 17171:17171 \
	-p 17179:17179 \
	--add-host example.com:0.0.0.0 \
	-e "serverAddress=example.com" \
	moques/docker-xmage-alpine:1.4.39V0
```

XMage needs to know the domain name the server is running on. The `--add-host` option adds an entry to the containers `/etc/hosts` file for this domain. 
Using environment variables you can modify the `config.xml` file.
You should always set `serverAddress` to the same value as `--add-host`.
If you dont have a Domain you can use a service like http://nip.io.

If you like to preserve the database during updates and restarts you can mount a volume at `/xmage/mage-server/db`

## Usage 2 "full and easy with standard config and domain on http://nip.io"
```bash
git clone https://github.com/MoQuEs/docker-xmage-alpine.git
sudo bash ./docker-xmage-alpine/dockerStartDocker.sh
```

### Update to Usage 2 (auto restart/update)
```bash
git clone https://github.com/MoQuEs/docker-xmage-alpine.git
sudo bash ./docker-xmage-alpine/dockerFullUpdate.sh
```

### Install requirements:
```bash
apt-get update
apt-get install git docker.io docker-compose
```

## Full example Docker Compose file
```yaml
version: '2'
services:
    mage:
        image: moques/docker-xmage-alpine:1.4.39V0
        container_name: moques_docker-xmage-alpine
        ports:
            - "17171:17171"
            - "17179:17179"
        extra_hosts:
            - "example.com:0.0.0.0"
        environment:
            - Xms=256M
            - Xmx=512M
            - MaxPermSize=256M
            - adminPassword=admin
            - serverAddress=example.com
            - serverName=mage-server
            - port=17171
            - secondaryBindPort=17179
            - backlogSize=800
            - numAcceptThreads=6
            - maxPoolSize=1000
            - leasePeriod=12000
            - socketWriteTimeout=10000
            - maxGameThreads=40
            - maxSecondsIdle=600
            - minUserNameLength=1
            - maxUserNameLength=32
            - userNamePattern=[^a-z0-9]
            - invalidUserNamePattern=[^a-z0-9]
            - minPasswordLength=0
            - maxPasswordLength=100
            - maxAiOpponents=50
            - saveGameActivated=false
            - authenticationActivated=false
            - googleAccount=
            - mailgunApiKey=
            - mailgunDomain=
            - mailSmtpHost=
            - mailSmtpPort=
            - mailUser=
            - mailPassword=
            - mailFromAddress=
        volumes:
            - xmage-db:/xmage/mage-server/db
            - xmage-saved:/xmage/mage-server/saved
volumes:
    xmage-db:
        driver: local
    xmage-saved:
        driver: local
```


## Links
[GitHub - Main](https://github.com/MoQuEs/docker-xmage-alpine/) \
[Docker - Main](https://hub.docker.com/r/moques/docker-xmage-alpine/) \
[GitHub - Based On](https://github.com/goesta/docker-xmage-alpine/) \
[Docker - Based On](https://hub.docker.com/r/goesta/xmage-alpine/)
