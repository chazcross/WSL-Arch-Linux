# Install
1. Install LxRunOffline
	```
	https://github.com/DDoSolitary/LxRunOffline
	```

2. Download distro - You should grab the most up to date version
	```
	https://github.com/DDoSolitary/LxRunOffline/wiki
	```

3. Install distro (update paths and distro version)
	```
	LxRunOffline.exe i -n ArchLinux -d **Install Location**\ArchLinux -f '**DOWNLOADLOCATION**\archlinux-bootstrap-2020.08.01-x86_64.tar.gz' -s  -r root.x86_64`
	```

4. Verify the wsl version for your distro
	```
	wsl --list --verbose
	```
	-Set the wsl version if it is incorrect
	```
	wsl --set-version ArchLinux 2
	```

5. Boot into ArchLinux
	- Restart windows terminal and it will be an Option

6. Set Password
	```
	passwd root
	```

7. Set up keys - If it fails run it again
	```
	pacman-key --init
	pacman-key --populate
	```

8. Close and Reopen Arch

9. Set up an initial mirror, pacman and initial tools
	```
	echo 'Server = http://mirrors.advancedhosters.com/archlinux/$repo/os/$arch' >> /etc/pacman.d/mirrorlist
	pacman -Sy
	pacman -S reflector
	reflector --country 'United States' --age 24 --sort rate --save /etc/pacman.d/mirrorlist
	pacman -S base-devel
	pacman -S nano
	```

10. Set locals
	```
    nano /etc/locale.gen
	```
    - uncomment the local you want such as en_US.UTF-8 UTF-8
	- `ctrl-x` to exit and save
    ```
	locale-gen
	```

11. Update the system
	```
	pacman -Syyu
	```

12. Setup Fakeroot if using WSl 1 - Update version is newer ones exist
	```
	cd ~
	pacman -S wget
	mkdir sources
	cd sources
	wget http://ftp.debian.org/debian/pool/main/f/fakeroot/fakeroot_1.24.orig.tar.gz
	tar xvf fakeroot_1.24.orig.tar.gz
	cd fakeroot-1.24/

	./bootstrap

  	./configure --prefix=/usr \
    	--libdir=/usr/lib/libfakeroot \
    	--disable-static \
    	--with-ipc=tcp

	make
	make install
	echo '/usr/lib/libfakeroot' > /etc/ld.so.conf.d/fakeroot.conf
	install -Dm644 README /usr/share/doc/fakeroot/README
	```

13. Set up user
	```
	useradd -m **username**
    passwd **username**
	nano /etc/sudoers
	```
	- Add your user in this section.
		```
		## User privilege specification
		root ALL=(ALL) ALL
		**username** ALL=(ALL) ALL
		```

14. Exit Arch

15. Change linux user in powershell
	```
	LxRunOffline.exe su -n ArchLinux -v 10001
	```

16. Start arch and you have a workign system

## Fixes
1. If you have issues with windows file permissions remount the drive
	```
	cd ~
    sudo umount /mnt/c
    sudo mount -t drvfs C: /mnt/c -o metadata
	```

## Optional
1. Set up ZSH and spaceship theme
	```
	sudo pacman -S git
	sudo pacman -S zsh
    zsh
	sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
	git clone https://github.com/denysdovhan/spaceship-prompt.git "$ZSH_CUSTOM/themes/spaceship-prompt"
    ln -s "$ZSH_CUSTOM/themes/spaceship-prompt/spaceship.zsh-theme" "$ZSH_CUSTOM/themes/spaceship.zsh-theme"
	```
    - Set ZSH_THEME="spaceship" in your .zshrc.
	```
	nano .zshrc
	```
	- Set Options to disable docker check on zsh if using wsl 1  https://github.com/denysdovhan/spaceship-prompt/blob/master/docs/Options.md

2. config nano colors
	```
    mkdir ~/.config
    mkdir ~/.config/nano
    cp /etc/nanorc ~/.config/nano/nanorc
    nano ~/.config/nano/nanorc
    ```
	- uncomment `include "/usr/share/nano/*.nanorc`

# Uninstall
```
LxRunOffline.exe ui -n ArchLinux
```