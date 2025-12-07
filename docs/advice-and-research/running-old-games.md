# Running Old Games

I own a bunch of old games from the 1990s and 2000s. Sometimes, I need to get them running, and it's a right faff, so
here's some things that I've documented to try to get things working.

## Running old Windows games

- Could try playing around with DosBox, PCem, or 86Box, but I find them quite fiddly and complicated, so I tend to use
  Oracle VM VirtualBox and create Virtual Machines (VMs).
- Sometimes the game will actually work on modern Windows, if you just enable run it in compatibility mode (Right-click
  the EXE > Properties > Compatibility > Run in compatibility mode for: ...).
- Use [WinWorld](https://winworldpc.com/home) to get old Windows installation discs and product key codes for installing
  Windows 95, 98 or 2000 on one of these VMs.
	- Remember that Windows 95 and 98 (and maybe 2000?) needs a boot disk that you mount as a floppy on the Virtual
	  Machine - you can get these from WinWorld too.
	- Yes, you're actually installing Windows on a Virtual machine. No, it doesn't take that long on modern hardware and
	  the process is relatively straight-forward, but sometimes has some quite technical bits.
	- [This video](https://www.youtube.com/watch?v=FoJESSDvEEU) was quite helpful in installing Windows 95, but who
	  knows if it will get taken down.
	- For Windows 95, mount the boot disk and the installation media, and do the following:
		- Load NEC IDE CDROM driver.
		- `fdisk` to create a partition (fixed disk drive) on the Virtual Machine's hard-drive for using.
		- Enable large disk support.
		- Create a DOS partition or Logical DOS drive
		- Create a Primary DOS partition.
		- Confirm it and make active.
		- Restart
		- Load NEC IDE CDROM driver.
		- `format C:` to format a C drive for usage.
		- Proceed with format
		- Hit Enter for no name.
		- `C:` to change to the C drive.
		- `mkdir WIN95` to create a folder/directory called `WIN95`
		- `cd WIN95` to change into that directory.
		- `copy D:\WIN95` to copy files from the CD into the current directory (assuming your CD is the D drive).
		- (It will copy the files).
		- Eject the CD.
		- `SETUP` to run the Windows setup.
		- Hit Enter.
		- Continue with the Windows 95 setup.
- Whenever you need to move files into the Virtual Machine from your host machine, either try to mount a folder as an
  extra drive (can be fiddly) or just zip/wrap the files up into an ISO file and then mount the ISO file as if it was a
  CD into the Virtual Machine (simpler but more cumbersome if you forget files or need to add extra files).

If you install Windows 95 (or 98) and you get a `Windows protection error`, then go get
the [FIX95CPU ISO from Archive.org](https://archive.org/details/fix-95-cpu-v3-final) and insert it and follow the
prompts to fix your Virtual Machine to not get the protection error. It looks like red text and scary, but it's fine.
Then continue with whatever you were doing.

If you need 256 colors for a game, then go get
the [256 color driver for Windows 95/98/ME from Archive.org](https://archive.org/details/256color_win9x) and install it.
You might get some fatal errors after installing it, but I found rebooting the machine led me back to Windows and
everything seemed to work fine and 256 colors was available as an option in Display settings.

If you boot a game and the sound is stuttering (and the visuals might stutter a little as well along with the sound)
then try switching sound driver. By default, it'll be using a Sound Blaster 16 - which works for Windows audio but
sometimes sucks for games. Instead, you should change the sound card & driver to be AC97 by doing:

- Get
  the [RealTek AC97 Audio Codec Driver for Windows 95 from userdrivers.com](https://www.userdrivers.com/Sound-Card/Realtek-AC97-Audio-Codec-Driver-4-06-for-Windows-95/download/)
  as an EXE.
- Don't run the EXE, but instead extract it using [7zip](https://7-zip.org/download.html).
- This will give you bunch more EXEs, some text files and CAB files and a WIN95 directory.
- Put all these extracted files into an ISO file (there's probably a program you can find to do this, but on a Linux
  host I used the `genisoimage` CLI tool).
- Shutdown your Virtual Machine (if it's not already stopped).
- Change the Sound Card to AC97.
- Mount the new AC97 ISO you made as a CD.
- Windows will detect the new sound card and prompt you to install a driver.
- Navigate into the CD and select the WIN95 directory where the driver is found.
- Install the driver.
- Reboot.
- The game should now run smoother and the audio shouldn't stutter.

## Abandonware games

[OldGamesDownload](https://oldgamesdownload.com) hosts ZIPs of old games that you can try running.
