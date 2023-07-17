# Week Four: Installing environment


Nice videos to watch: https://www.youtube.com/watch?v=WViYgkqtDAA&ab_channel=RISC-VInternational  

**Working on Risc-V board using Box64**  
Configuration  
<img width="711" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/9032cd92-b041-4abd-bcbb-09b13ac9c626">  

Compiling/Installing Box64  
```bash
git clone https://github.com/ptitSeb/box64
cd box64
mkdir build; cd build; cmake .. -D RV64=1 -D CMAKE_BUILD_TYPE=RelWithDebInfo
make -j4  
sudo make install
```
```
sudo systemctl restart systemd-binfmt
```
<img width="1060" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/421cb9ea-521f-4648-8a0c-984e9ee966b9">  


Seems that `binfmt_misc` isn't built into our kernel, which is causing the `modprobe: FATAL: Module binfmt_misc not found` error. `# CONFIG_BINFMT_MISC is not set`, means `binfmt_misc` is not enabled in our kernel. 

It seems that the kernel has to be rebuilt with `binfmt_misc` support enabled. Contacted official support, rebuilt kernel.  
References: https://wiki.sipeed.com/hardware/en/lichee/th1520/lpi4a/4_burn_image.html  
https://pan.baidu.com/e/1xH56ZlewB6UOMlke5BrKWQ  

Flashing or "burning" a new operating system (OS) image onto a piece of hardware refers to the process of removing the existing OS and replacing it with a different one. Here's a more detailed breakdown:

**Operating System (OS) Image**: This is a file that contains the entire operating system and all necessary files to start up and run the OS on a piece of hardware. It could include the kernel (the core part of the OS), device drivers (software that lets the OS use hardware like a mouse or keyboard), and other software. This image is often compressed into a single file that can be easily downloaded and transferred.

**Hardware**: In this context, the hardware could be a variety of things, such as a computer, a smartphone, a tablet, a smart home device, or a microcontroller like a Raspberry Pi. All these devices have some form of OS to function properly, even if it's a very minimal one.

**Flashing**: This is the process of writing the OS image onto the device's storage. Flashing is often used when you want to change the OS on your device, either to update it to a newer version, switch to a different OS, or to restore the device to its factory settings. 

This process involves erasing the current OS and data from the device's storage and then writing the new OS image to the storage. The name "flashing" comes from the fact that this process often involves writing to flash memory, a type of memory that can be electrically erased and reprogrammed.

It's worth noting that flashing an OS can be risky - if something goes wrong during the process, the device could become unresponsive or "bricked". That's why it's important to follow flashing instructions carefully and to ensure your device has a reliable power source during the process.

Afterwards, we need to reinstall everything  
First, set up the ssh  
```
sudo apt-get install openssh-server
ssh debian@10.0.0.184
```
<img width="1017" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/a56b2ab6-867b-4106-9b95-b40106876185">  

The error is related to a security measure implemented in SSH to protect against malicious activities such as man-in-the-middle attacks. When you connect to a remote server for the first time via SSH, the server's public key is stored in a file on your local machine (in this case, the file is `~/.ssh/known_hosts`)   

When you try to connect to the server again, SSH checks the server's public key against the one stored in the `known_hosts` file. If they don't match, SSH stops the connection attempt and throws the error you're seeing. This mismatch could be due to legitimate reasons (like reinstallation of the server's operating system or changes in its network settings) or because of a security threat    

To solve this issue, you need to remove the existing key for the remote host from the `known_hosts` file. The error message indicates that the offending key is on line 10 of the `known_hosts` file  
```
ssh-keygen -R 10.0.0.184
ssh debian@10.0.0.184
```
Next, update your system and install necessary packages   
```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install build-essential
sudo apt-get install git
sudo apt install cmake
```
Now, rerun the `box64` configuration/installation steps  
Installed Successfully  
<img width="1026" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/0684b69a-c969-4951-80a7-904ecf6992b3">  

**Installing Wine**  
References: https://www.youtube.com/watch?v=7lNnXcNgFFw&t=30s&ab_channel=TheByteman  
Follow installation steps from the video  
After you have installed and unzipped the file, move the file to your home directory    
```
./wine64 winecfg
```

**Installing Clash VPN**  

Download risc-v version   
<img width="903" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/0d401992-d923-4a51-952a-80ddf3b71fab">  

Move file from host machine to 终端  
```
scp ./clash-linux-riscv64-v1.17.0.gz debian@10.0.0.184:/home/debian
```
<img width="1066" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/5611247e-cd64-403f-9834-ef6363ed5ba3">  

for the wget -O /opt/clash/config.yaml "" change the content of the "" based on your Clash订阅链接  
```
sudo su
mkdir /opt/clash && cd /opt/clash
sudo mv clash-linux-riscv64-v1.17.0.gz /opt/clash/
wget -O /opt/clash/config.yaml "https://new.ssrss.de/sub?target=clash&new_name=true&url=https%3A%2F%2Fssrss.de%2Fs%2Fr5Jfu&udp=true&tfo=true&config=https%3A//ssrss.de/ssrss.ini" 
wget https://dl3.ssrss.club/Country.mmdb
gunzip -c *.gz > clash && chmod +x clash
```
```
cat > /usr/lib/systemd/system/clash.service <<'EOF'
[Unit]
Description=clash
[Service]
TimeoutStartSec=0
ExecStart=/opt/clash/clash -d /opt/clash
[Install]
WantedBy=multi-user.target
EOF
```
If nothing goes wrong, you should be able to use clash now   
```
systemctl start clash  
```
You can also auto start clash with   
```
systemctl enable clash
```  
Check the status with   
```
systemctl status clash
```
<img width="956" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/d92fd6ae-6371-466f-93d0-c2647c47343e">  

Next, go to `Settings` -> `Network` and then enable and set the proxy ports according to your configuration file  
<img width="1027" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/df326293-e1e6-42ab-909b-b5b4348d7351">  
  

You can also set your proxy settings in the website http://clash.razord.top/#/proxies, note the ports should match the ones in your configuration file  
<img width="418" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/3d0c6d00-4471-427f-94b1-ff70d47d042d">  
<img width="1043" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/a513c343-c451-4844-af4e-863da6680801">  


**Requesting实验室服务器**  
<img width="559" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/6ec14f31-ec87-4e88-825b-0b1453fe5d40">  
```
ssh james@10.0.16.16
```
<img width="1055" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/28d9027b-0f4c-4c12-8f74-6b3c3a025aff">  

