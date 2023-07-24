# Week Five: Gaming with Box64


Nice videos to watch: https://www.youtube.com/watch?v=WViYgkqtDAA&ab_channel=RISC-VInternational  

**Working on Risc-V board using Box64**  
Trying to set up cross-compilation environment  
```bash
sudo dpkg --add-architecture amd64
sudo apt-get update
sudo apt-get install gcc-multilib
sudo apt-get install g++-multilib
```
Add corresponding debian repositories  
```
sudo nano /etc/apt/sources.list.d/amd64-architecture.list
```
Paste the following content  
```
deb http://deb.debian.org/debian bookworm contrib main non-free-firmware
deb http://deb.debian.org/debian bookworm-updates contrib main non-free-firmware
deb http://deb.debian.org/debian bookworm-backports contrib main non-free-firmware
deb http://deb.debian.org/debian-security bookworm-security contrib main non-free-firmware
```

<img width="619" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/1a3344a5-8553-4443-a994-7d91912d6854">  


Tried to resolve the errors above with  
<img width="620" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/9c8a618f-03c0-4743-8054-5d62ffaab33a">  
Replaced `systemd-sysv` with `libpam-systemd:amd64` and `libnss-systemd:amd64`  
Didn't realize so I proceeded... realized that this wasn't the right way so I tried to revert the changes  
```
sudo dpkg --remove-architecture amd64
```
<img width="811" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/38427459-60eb-4c17-9dd5-31812e7a98a8">  

At this point I should have stopped and looked but I thought that it was fine removing `amd64`  
<img width="470" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/3254b044-efe2-4860-9e3d-f06be187bbf6">  
<img width="833" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/c96d081b-3a1c-4a8f-a6a0-874e9aac184b">  

System broke :(  
<img width="831" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/c74a5455-d08b-4023-9102-2d0966bc564c">  
<img width="832" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/9673b408-cf22-486e-b664-5f9a9e03a375">  

Fixed by installing back `gnome` (system didn't break, just some dependencies got disrupted)  
Back and running (make sure you can connect to internet first, I deleted internet related files too)  
```
sudo apt install gnome
```

Somehow the SD card with additional storage is also reognized now (might have to change permission to get working though)  
<img width="707" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/4b93863f-1cd4-4941-9074-88a0ee1201d5">  


**Creating x86 Workspace**  

Cloud enviornment is based on `Ubunutu` so we going to have to use `debootstrap` to emulate debian 12 (bookworm) enviornment  
<img width="682" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/68c18a46-5a08-4417-9063-fb16b948cf8d">  

`Debootstrap` is a tool which can be used to install a Debian base system into a subdirectory of another, already installed system. It doesn't require an installation CD, just network access to a Debian repository.

1. **Install debootstrap**
   On your Ubuntu system, you can install `debootstrap` with `apt-get`:
   ```
   sudo apt-get update
   sudo apt-get install debootstrap
   ```
2. **Create a Directory for the New System**
   Let's create a directory where we'll install the new system. Let's call it `~/debian-chroot`:
   ```
   mkdir ~/debian-chroot
   ```
3. **Run debootstrap**
   Now you can run `debootstrap` to install Debian into `~/debian-chroot`. For Debian 12 "Bookworm", you can use:
   ```
   sudo debootstrap --arch amd64 bookworm ~/debian-chroot http://deb.debian.org/debian/
   ```
   Replace `bookworm` with the codename of the Debian release you want to install, and replace `amd64` with the architecture of the system you're installing.
   The URL at the end of the command is the location of a Debian mirror. You can replace this with the URL of any valid Debian mirror.
   This command will take a while to run as it needs to download and install all the base packages.
4. **Chroot into the New System**
   Once `debootstrap` has finished, you can use `chroot` to start a shell in the new system:
   ```
   sudo chroot ~/debian-chroot
   ```
   Now, you'll have a shell prompt inside the new system, from which you can run commands as if you were actually running that system.

5. **Setting up apt sources**

   We'll need to set up the `/etc/apt/sources.list` file in the chroot so that `apt` knows where to download packages from. Here's an example:
  ```
  #deb cdrom:[Debian GNU/Linux 12.0.0 _Bookworm_ - Official amd64 NETINST with firmware 20230610-10:21]/ bookworm main non-free-firmware
  
  deb http://deb.debian.org/debian/ bookworm main non-free-firmware
  deb-src http://deb.debian.org/debian/ bookworm main non-free-firmware
  
  deb http://security.debian.org/debian-security bookworm-security main non-free-firmware
  deb-src http://security.debian.org/debian-security bookworm-security main non-free-firmware
  
  # bookworm-updates, to get updates before a point release is made;
  # see https://www.debian.org/doc/manuals/debian-reference/ch02.en.html#_updates_and_backports
  deb http://deb.debian.org/debian/ bookworm-updates main non-free-firmware
  deb-src http://deb.debian.org/debian/ bookworm-updates main non-free-firmware
  
  # This system was installed using small removable media
  # (e.g. netinst, live or single CD). The matching "deb cdrom"
  # entries were disabled at the end of the installation process.
  # For information about how to configure apt package sources,
  # see the sources.list(5) manual.
  ```
   Afterwards, run:

   ```
   apt-get update
   ```
   This will update the package list.

<img width="847" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/f009ab02-1781-4d7b-b237-b283e7907497">  


Seems like the cross architecture completely messed up the system   
<img width="839" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/53653e3d-a837-467e-a688-30375b80de98">  
<img width="832" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/d3683f7a-7d25-49c6-8877-eb3006fdbb32">  

Have to reformat system again   
Seems like it's not such a good idea to add cross architecutre (lesson learned)  

Back and running  
<img width="842" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/ceb93bfd-0416-4497-a427-745071b4a7fb">  

**Migrating from x86 to Riscv**  

1. **Testing Freeciv**   

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
box64 /media/debian/b987d4cc-fc01-4a0a-a656-6c9c781adf34/debian-chroot/usr/games/freeciv-gtk3.22 
```

Seems like box64 doesn't support this game yet... Onto the next   
<img width="819" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/3c807422-be8f-4274-ada5-289d2c2a767c">  
<img width="842" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/7a58936f-0671-403d-ba42-adb693f8f40d">  
<img width="858" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/13ac6da5-1895-423b-a251-27589e95aed7">  

Confirmed with owner of box64   
https://github.com/ptitSeb/box64/issues/897   

Clean up command   
```
apt-get remove --purge freeciv
apt-get autoclean
apt-get autoremove
```

2. **Testing Wesnoth**   

Running game with  
```
box64 /media/debian/b987d4cc-fc01-4a0a-a656-6c9c781adf34/debian-chroot/usr/games/wesnoth-1.16
```

<img width="672" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/f459258e-20a4-40b5-9923-fe2a4e1ad77b">  
<img width="878" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/e9ee2ab5-3d66-404a-b780-c9707159e7d2">  

Files exist, must register to path, resolved with   
```
export LD_LIBRARY_PATH=/media/debian/b987d4cc-fc01-4a0a-a656-6c9c781adf34/debian-chroot/usr/lib/x86_64-linux-gnu/
```

It seems this game doesn't work also :/    

<img width="852" alt="image" src="https://github.com/JamesYen220/test/assets/100248639/eb106753-47c2-4b76-9af3-80d0b7665e66">  
![Uploading image.png…]()  

Clean up command   
```
apt-get remove --purge wesnoth
apt-get autoclean
apt-get autoremove
```

3. **Testing Supertuxkart**   
