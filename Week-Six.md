# Week Six: Gaming with Box64

**Migrating from x86 to Riscv (Part2)**  

3. **Testing Supertuxkart (sudo apt-get install supertuxkart)**   

Change mode
```
sudo chroot ~/debian-chroot
```

Tarring file from x86   
```
sudo tar czvf debian-chroot.tar.gz debian-chroot/
```

Transfer file over to Riscv enviornment   
```
scp debian-chroot.tar.gz debian@10.0.0.184:/media/debian/b987d4cc-fc01-4a0a-a656-6c9c781adf34
```

Unzip on Riscv enviornment   
```
sudo tar xzvf debian-chroot.tar.gz -C /media/debian/b987d4cc-fc01-4a0a-a656-6c9c781adf34
```

Running game with   
```
export LD_LIBRARY_PATH=/media/debian/b987d4cc-fc01-4a0a-a656-6c9c781adf34/debian-chroot/usr/lib/x86_64-linux-gnu/
box64 /media/debian/b987d4cc-fc01-4a0a-a656-6c9c781adf34/debian-chroot/usr/games/supertuxkart
```

<img width="1565" alt="image" src="https://github.com/JamesYen220/RiscV-SummerTraining/assets/100248639/e43b3a13-c81b-49cc-8ba7-f15f5de10b70">  

Owner responded with   
https://github.com/ptitSeb/box64/issues/904  
```
sudo apt install libopenal1
```

However, more errors occurred  
<img width="1396" alt="image" src="https://github.com/JamesYen220/RiscV-SummerTraining/assets/100248639/dce53421-bf22-4b43-b7f4-9255edcd61f2">  

Clean up command   
```
apt-get remove --purge supertuxkart
apt-get autoclean
apt-get autoremove
```

4. **Testing Openttd (sudo apt-get install openttd)**

Running game with   
```
box64 /media/debian/b987d4cc-fc01-4a0a-a656-6c9c781adf34/debian-chroot/usr/games/openttd
```
<img width="1505" alt="image" src="https://github.com/JamesYen220/RiscV-SummerTraining/assets/100248639/9ed2f515-2ef3-4043-8fb0-10b141b29360">   

Clean up command   
```
apt-get remove --purge openttd
apt-get autoclean
apt-get autoremove
```

5. **Testing 0 A.D (sudo apt-get install 0ad)**

Running game with   
```
box64 /media/debian/b987d4cc-fc01-4a0a-a656-6c9c781adf34/debian-chroot/usr/games/pyrogenesis
```

<img width="1115" alt="image" src="https://github.com/JamesYen220/RiscV-SummerTraining/assets/100248639/fb09f9e1-40d8-4bb4-b419-38e377fa193f">  

Run the command   
```
export BOX64_LD_LIBRARY_PATH=$BOX64_LD_LIBRARY_PATH:/media/debian/b987d4cc-fc01-4a0a-a656-6c9c781adf34/debian-chroot/usr/lib/games/0ad/
```
<img width="1620" alt="image" src="https://github.com/JamesYen220/RiscV-SummerTraining/assets/100248639/b85807e0-1854-41a8-a0ca-a08b3ed2b8e6">  
<img width="1556" alt="image" src="https://github.com/JamesYen220/RiscV-SummerTraining/assets/100248639/859737ed-feff-4f1a-96f4-5cd2e5208dc2">  

Clean up command   
```
apt-get remove --purge 0ad
apt-get autoclean
apt-get autoremove
```

**Installing Gl4es**  
Reference: https://github.com/ptitSeb/gl4es  

Configure `locales`
```
sudo apt install locales
sudo dpkg-reconfigure locales
```
Set to `en-US-UTF8`  
<img width="1217" alt="image" src="https://github.com/JamesYen220/RiscV-SummerTraining/assets/100248639/8f5a1a0b-6627-4b35-a3e9-a7c948064ce1">  

Reboot the system with  
```
sudo reboot
```

Install and compile Gl4es  
```
git clone git@github.com:ptitSeb/gl4es.git
cd gl4es
mkdir build; cd build; cmake .. -DCMAKE_BUILD_TYPE=RelWithDebInfo;
make
```
<img width="650" alt="image" src="https://github.com/JamesYen220/RiscV-SummerTraining/assets/100248639/fab93928-f8e7-4e57-b2f7-203bf5ad6fdf">  
<img width="535" alt="image" src="https://github.com/JamesYen220/RiscV-SummerTraining/assets/100248639/1a7a6526-8420-4882-8fd7-7c692860d13e">  


**Testing Openttd**  

```
chmod +x media/debian/b987d4cc-fc01-4a0a-a656-6c9c781adf34/openttd_13_3_65275.sh
cd media/debian/b987d4cc-fc01-4a0a-a656-6c9c781adf34/
cd Downloads
BOX64_BASH=~/box64/tests/bash box64 ./openttd_13_3_65275.sh
```
![image](https://github.com/JamesYen220/RiscV-SummerTraining/assets/100248639/df642c5c-e7f0-446d-a3ec-d4fd3fabd087)  

Start game with  
```
cd GOG Games
LD_LIBRARY_PATH=~/gl4es/lib box64 ./start.sh
```
![image](https://github.com/JamesYen220/RiscV-SummerTraining/assets/100248639/2fc99c61-5e2f-4a3c-a4b6-d37c2a4bea66)  
Seems like too many missing symbols causing the game to not start properly     


**Testing Starvalley**  
References: https://forum.rvspace.org/t/run-stardew-valley-on-visionfive-2/2642  
https://github.com/ptitSeb/gl4es/blob/master/COMPILE.md  
https://github.com/ptitSeb/box64/issues/635  
https://github.com/ptitSeb/box86-compatibility-list/issues/220  
https://github.com/ptitSeb/box64/issues/531  

Buy Stardew Valley on gog.com
```
chmod +x Downloads/stardew_valley_1_5_6_1988831614_53040.sh
BOX64_BASH=/usr/local/bin/box64/tests/bash box64 Downloads/stardew_valley_1_5_6_1988831614_53040.sh 
```
<img width="884" alt="image" src="https://github.com/JamesYen220/RiscV-SummerTraining/assets/100248639/c029ec0c-0df7-4b5a-8783-cdc04b9cb97c">  

After you run `BOX64_BASH=~/box64/tests/bash box64 ./stardew_valley_1_5_6_1988831614_53040.sh` press `next` on everything    
```
cd Downloads
BOX64_BASH=~/box64/tests/bash box64 ./stardew_valley_1_5_6_1988831614_53040.sh
```
![image](https://github.com/JamesYen220/RiscV-SummerTraining/assets/100248639/950a24e0-6f2d-4c97-a492-e877052d1845) 

Start game with  
```
cd GOG Games
LD_LIBRARY_PATH=~/gl4es/lib box64 ./start.sh
```

Error starting game   
![image](https://github.com/JamesYen220/RiscV-SummerTraining/assets/100248639/6175b3fd-6a32-4a6b-97f6-4f0bf80ae4e9)  

Like owner suggested need a few libraries  
https://github.com/ptitSeb/box64/issues/907  

We need to create another debootstrap  
```
sudo mkdir ~/test
sudo debootstrap buster ~/test http://deb.debian.org/debian/
sudo chroot test/
apt-get update
apt-get install openssl
apt-get install libssl1.1
```

We can confirm that we have the libraries with   
```
find / -name "libssl.so.1"
find / -name "libssl*"
find / -name "libcrypto*"
```

After confirming, we move the files from x86 machine back to RISC-V machine
```
scp /home/james/test/usr/lib/x86_64-linux-gnu/libssl.so.1.1 debian@10.0.0.184:~
scp /home/james/test/usr/lib/x86_64-linux-gnu/libcrypto.so.1.1 debian@10.0.0.184:~
```

From the RISC-V machine
```
sudo mv ~/libssl.so.1.1 /usr/lib/x86_64-linux-gnu/
sudo mv ~/libcrypto.so.1.1 /usr/lib/x86_64-linux-gnu/
```

Now, rerun the `LD_LIBRARY_PATH=~/gl4es/lib box64 ./start.sh` command  
![image](https://github.com/JamesYen220/RiscV-SummerTraining/assets/100248639/01aaec30-d31b-48ae-bbae-534978d754c8)  
![image](https://github.com/JamesYen220/RiscV-SummerTraining/assets/100248639/e2635d9b-98d6-4ba2-9436-a51a4c61c45c)  
![image](https://github.com/JamesYen220/RiscV-SummerTraining/assets/100248639/3714ae5e-b28e-46d7-9ea7-920f6134e6ba)  
![image](https://github.com/JamesYen220/RiscV-SummerTraining/assets/100248639/9c256916-b9ec-421c-b663-fa133ac88e25)  
![image](https://github.com/JamesYen220/RiscV-SummerTraining/assets/100248639/2865a137-3c4a-4e1b-b38a-6cb43c5c08d7)  
![image](https://github.com/JamesYen220/RiscV-SummerTraining/assets/100248639/c36d7555-f349-4fab-a26e-48aad5571f43)  
![image](https://github.com/JamesYen220/RiscV-SummerTraining/assets/100248639/1d72b484-f4f2-4c6e-bb6e-bed0fe405f9e)  

Finally working although it's a bit laggy :D   

**Cleaning Up Storage**  

```
sudo apt update
sudo apt autoremove
sudo apt clean
sudo apt-get autoclean
```
