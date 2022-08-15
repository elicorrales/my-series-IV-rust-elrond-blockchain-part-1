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
```
wget -O erdpy-up.py \
    https://raw.githubusercontent.com/ElrondNetwork/elrond-sdk-erdpy/master/erdpy-up.py;
