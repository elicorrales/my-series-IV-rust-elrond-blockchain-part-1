# my-series-IV-rust-elrond-blockchain-part-1

### This project is part of a series and includes a video.

See [Here](https://github.com/elicorrales/blockchain-tutorials/blob/main/README.md) for the overall document that
refers to all the series.  
  
# (Optional) Set Up WSL Ubuntu  
  We are going to try our luck using a fresh install of Ubuntu 22.04.  
  For more details, please review:  
  - Video: [Blockchain Programmer - Part 3: How to Set Up Multiple WSL Ubuntu Environments](https://www.youtube.com/watch?v=9hssNB5LiVE&list=PLNKa8O7lX-w5nEQjNbFRQV7e3Sd4qLi44&index=3)  
  -   Doc: [How-To-Setup-Multiple-WSL-Ubuntu](https://github.com/elicorrales/blockchain-tutorials/blob/main/How-To-Setup-Multiple-WSL-Ubuntu.md)  

  (I tried to include everything in the GitHub README but the video is live so it's the actual work of creating the environment, that the doc might have missed a step or two.  

```
sudo apt update && sudo apt -y upgrade
```


# Set Up Local Node (LOCALNET)  
(Reference: https://docs.elrond.com/developers/setup-local-testnet/)  
(NOTE: I found some issues with the doc)  
  
#### What is ```erdpy```?  
> It consists of Command Line Tools and Python SDK 
> for interacting with the Blockchain (in general) 
> and with Smart Contracts (in particular).
  
It can be installed with this script:  
First we download the script.  
```
wget -O erdpy-up.py \
    https://raw.githubusercontent.com/ElrondNetwork/elrond-sdk-erdpy/master/erdpy-up.py;
```
  
Then we run the script.  
```
python3.10 erdpy-up.py;
```
  
You might encounter an issue, and see the following suggestion:  
```
sudo apt install python3.10-venv
```
  
Do that, and re-run the ```python3.10 erdpy-up.py```.  
  
You may have to adjust your ```$PATH```.  
Make sure you're still in ```${HOME}```, and do
```
find . -name "*erdpy*"
```
  
Now edit your ```.bashrc``` and at the bottom, add:  
```
export PATH=${HOME}/elrondsdk:$PATH;
```
  
Test it by doing ```erdpy --version```.  
  
You're going to need some prerequisites for your localnet.  
```
erdpy testnet prerequisites
```
  
Set up some global config parameters.  
```
erdpy config set chainID local-testnet
```
```
erdpy config set proxy http://localhost:7950
```
  
Now we create some directories:  
```
mkdir elrond-local-testnet;
mkdir -p my-first-elrond-project/my-client;
mkdir -p my-first-elrond-project/my-smart-contracts;
```
  
```
tree
.
├── elrond-local-testnet      #<--- this holds localnet stuff
└── my-first-elrond-project   #<---our main project dir
    ├── my-client
    └── my-smart-contracts
```
  
Go into the directory...and create a file...
```
cd elrond-local-testnet
```
edit (vim or nano) ```testnet.toml```:  
```
[networking]
port_proxy = 7950
```
  
While still in same directory, run:  
```
erdpy testnet config
```
  
(this one takes a while)
Upon running this command, a new folder called testnet will be added in the current directory. This folder contains the Node & Proxy binaries, their configurations, plus the development wallets.  
  
Did it fail?  You might be missing ```gcc``` compiler.  
```
sudo apt install -y build-essential
```
  
Your best course now is to ```rm -rf testnet``` (the auto-created sub-dir) and start over.  
Re-run the ```erdpy testnet config```.  
  
Finally, we are ready to start up the localnet.  
  
## Starting The Local Testnet  
```
erdpy testnet start
```
  
## Re-Starting The Local Testnet  
 
If you're only doing local development and you don't care about retaining data/state between runs of the localnet, this (below) may help.  If you **DO** care, this (below) will **NOT** help.  
  
Trying to start the localnet on a subsequent run may show a cycling of this output (below).  If so, you can just ```rm -rf testnet``` (the auto-generated directory).  
```
[PID=80] ERROR[2022-08-15 17:34:47.534]   economics data request                   observer = http://localhost:10101 error = Get "http://localhost:10101/network/economics": dial tcp 127.0.0.1:10101: connect: connection refused
[PID=80] WARN [2022-08-15 17:34:47.534]   cannot get node status. will mark as inactive address = http://localhost:10100 error = Get "http://localhost:10100/node/status": dial tcp 127.0.0.1:10100: connect: connection refused
[PID=80] WARN [2022-08-15 17:34:47.534]   economic metrics: get from API           error = sending request error
[PID=80] ERROR[2022-08-15 17:34:47.534]   validator statistics                     observer = http://localhost:10101 error = no response
[PID=80] WARN [2022-08-15 17:34:47.534]   validator statistics: get from API       error = validator statistics data not found at any observer
[PID=80] WARN [2022-08-15 17:34:47.534]   cannot get node status. will mark as inactive address = http://localhost:10101 error = Get "http://localhost:10101/node/status": dial tcp 127.0.0.1:10101: connect: connection refused
[PID=80] WARN [2022-08-15 17:34:47.535]   cannot get node status. will mark as inactive address = http://localhost:10100 error = Get "http://localhost:10100/node/status": dial tcp 127.0.0.1:10100: connect: connection refused
[PID=80] WARN [2022-08-15 17:34:47.535]   cannot get node status. will mark as inactive address = http://localhost:10101 error = Get "http://localhost:10101/node/status": dial tcp 127.0.0.1:10101: connect: connection refused


[PID=80] ERROR[2022-08-15 17:35:12.554]   heartbeat                                observer = http://localhost:10100 shard = 0 error = {"data":null,"error":"node is starting","code":"internal_issue"}
[PID=80] ERROR[2022-08-15 17:35:12.554]   heartbeat                                error = heartbeat status not found at any observer shard = 0
[PID=80] WARN [2022-08-15 17:35:12.554]   heartbeat: get from API                  error = heartbeat status not found at any observer


[PID=80] ERROR[2022-08-15 17:35:37.575]   heartbeat                                observer = http://localhost:10100 shard = 0 error = {"data":null,"error":"node is starting","code":"internal_issue"}
[PID=80] ERROR[2022-08-15 17:35:37.575]   heartbeat                                error = heartbeat status not found at any observer shard = 0
[PID=80] WARN [2022-08-15 17:35:37.575]   heartbeat: get from API                  error = heartbeat status not found at any observer
[PID=80] WARN [2022-08-15 17:35:47.537]   cannot get node status. will mark as inactive address = http://localhost:10100 error = observer http://localhost:10100 responded with code 500
[PID=80] WARN [2022-08-15 17:35:47.539]   cannot get node status. will mark as inactive address = http://localhost:10101 error = observer http://localhost:10101 responded with code 500
[PID=80] ERROR[2022-08-15 17:35:47.539]   validator statistics                     observer = http://localhost:10101 error = no response
[PID=80] WARN [2022-08-15 17:35:47.539]   validator statistics: get from API       error = validator statistics data not found at any observer
[PID=80] WARN [2022-08-15 17:35:47.540]   cannot get node status. will mark as inactive address = http://localhost:10100 error = observer http://localhost:10100 responded with code 500
[PID=80] WARN [2022-08-15 17:35:47.540]   cannot get node status. will mark as inactive address = http://localhost:10101 error = observer http://localhost:10101 responded with code 500
```
  
Once you have removed the ```testnet``` directory, then do
```
erdpy testnet config
```
  
And then,
```
erdpy testnet start
```
