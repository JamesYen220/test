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

<img width="817" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/60766bce-f3af-46e0-a751-dd580d22ac0b">  

Owner responded with   
https://github.com/ptitSeb/box64/issues/904  
```
sudo apt install libopenal1
```

However, more errors occurred  
<img width="828" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/f3345791-b1f9-47b9-b8e6-a1ce672a1cc4">  

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
<img width="828" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/1cff2935-de4a-4443-872b-3f1d66c7b25f">   

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

<img width="823" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/30ea355c-0466-49ae-82bf-e7a31e6252f8">   

Run the command   
```
export BOX64_LD_LIBRARY_PATH=$BOX64_LD_LIBRARY_PATH:/media/debian/b987d4cc-fc01-4a0a-a656-6c9c781adf34/debian-chroot/usr/lib/games/0ad/
```
<img width="848" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/536687ab-9f22-418e-814f-076c7c2a211a">   
<img width="841" alt="ima. e" src="https://github.com/JamesYen220/test/assets/100248639/b293242b-ced3-40e1-9e72-af61a3e601ba">   

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
<img width="828" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/0e516abf-67a8-45e5-be3a-18076e463d73">  

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
<img width="813" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/4065364d-3238-473d-b5b9-2977f8b5f239">  
<img width="686" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/3903a14e-9bdd-4476-8a67-d3b3e777df73">  


**Testing Openttd**  

```
chmod +x media/debian/b987d4cc-fc01-4a0a-a656-6c9c781adf34/openttd_13_3_65275.sh
cd media/debian/b987d4cc-fc01-4a0a-a656-6c9c781adf34/
cd Downloads
BOX64_BASH=~/box64/tests/bash box64 ./openttd_13_3_65275.sh
```
<img width="844" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/55efb9de-a78b-4b6f-ba4a-e17211570e17">  

Start game with  
```
cd GOG Games
LD_LIBRARY_PATH=~/gl4es/lib box64 ./start.sh
```
<img width="853" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/89a1d583-c2bb-4714-9529-5faa81936cf0">  
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
<img width="831" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/2b5a1671-da29-4455-b2f5-33d6457c8fdf">  

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
<img width="826" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/7f092002-ad34-4e94-95ba-141885c06e15">  

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
<img width="821" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/2662294c-c2c8-43df-a378-566a7045b29c">  
<img width="820" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/4406af96-3428-48c2-bc02-ff63ff27fab9">  
<img width="817" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/a59831a4-38d9-4d7b-b1f7-793d6c9def9a">   
<img width="822" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/5a587a20-4670-498f-9e5c-8db46ad4889d">  
<img width="819" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/998a21d5-5df9-4fdb-b38f-0b239c838b4b">  
<img width="821" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/095bc07c-6734-4902-8307-a30169653243">  
<img width="818" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/f7ef70b3-8ab8-4c96-b393-4405647a190b">  

Finally working although it's a bit laggy :D   

**Cleaning Up Storage**  

```
sudo apt update
sudo apt autoremove
sudo apt clean
sudo apt-get autoclean
```
