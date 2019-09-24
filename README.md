# Multi Machine Fabric Setup

Ubuntu
To run Hyperledger Composer and Hyperledger Fabric, we recommend you have at least 4Gb of memory.

The following are prerequisites for installing the required development tools:
```
Operating Systems: Ubuntu Linux 14.04 / 16.04 LTS (both 64-bit), or Mac OS 10.12
Docker Engine: Version 17.03 or higher
Docker-Compose: Version 1.8 or higher
Node: 8.9 or higher (note version 9 is not supported)
npm: v5.x
git: 2.9.x or higher
Python: 2.7.x
```

A code editor of your choice, we recommend VSCode.
**If installing Hyperledger Composer using Linux, be aware of the following advice:

Login as a normal user, rather than root.
Do not su to root.
When installing prerequisites, use curl, then unzip using sudo.
Run prereqs-ubuntu.sh as a normal user. It may prompt for root password as some of it's actions are required to be run as root.
Do not use npm with sudo or su to root to use it.
Avoid installing node globally as root.**

If you're running on Ubuntu, you can download the prerequisites using the following commands:
```
curl -O https://hyperledger.github.io/composer/v0.19/prereqs-ubuntu.sh

chmod u+x prereqs-ubuntu.sh

```
### Install nodejs / Composer
```
npm install -g composer-cli@0.16
npm install -g composer-rest-server@0.16
npm install -g generator-hyperledger-composer@0.16
npm install -g yo
npm install -g composer-playground@0.16

```
### Download the repo for Multi Org

cd ~
curl -sSL https://goo.gl/byy2Qj | bash -s 1.0.4
mkdir fabric-binaries
mv bin fabric-binaries/bin
echo 'export PATH=~/fabric-binaries/bin:$PATH' >> ~/.profile
git clone https://github.com/Hemadri513/Multi-Machine-Fabric-Setup.git
cd Multi-Machine-Fabric-Setup
cd composer
nano howtobuild.sh //Replace the IP addresses in HOST1 and HOST2 with your own IPs 
./howtobuild.sh
cd ../..
```

At this point, if you have done these instructions for one machine, either duplicate your VM at this time or prepare another environment with the same steps as described so far until you get to git cloning this repo. Instead of cloning the repo, scp -r the fabric-dev-servers-multipeer folder from the first machine to the second.

On the first machine run
```
./startFabric.sh
```
On the second machine run
```
./startFabric-Peer2.sh
```

After this continue on the first machine.

### Create the Composer profile on the First Machine and start Composer Playground and Blockchain Explorer
```
./createPeerAdminCard.sh
nohup composer-playground -p 8181 &
cd ..
git clone https://github.com/InflatibleYoshi/blockchain-explorer
sudo apt install postgresql postgresql-contrib
cd blockchain-explorer
git checkout release-3
```
Edit config.json with the correct tlscerts path. You do not need them functionally but they are there because there have been reported issues when not including the tls certs.
```
sudo -u postgres psql
\i app/db/explorerpg.sql
\q
npm install
cd app/test
npm install
npm run test
cd ../../client
npm install
npm test -- -u –coverage
npm run build
cd ..
./start.sh
```

At this point you should be able to navigate a browser to http:/{HOST1-DOMAIN/IP}:8080 and connect to either alice or bob's trade network instances.
