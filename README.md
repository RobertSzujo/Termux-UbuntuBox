# Termux Prefix File with preinstalled Ubuntu 22.04, Wine and VirGL GPU acceleration

## Please make sure to read the whole readme file before installing this file!

# What does it contain?
- Base Termux files
- Wayland and Termux-X11 preinstalled for "native" display output
- Preinstalled Mesa with Zink support + VirGL rendering server
- Preinstalled PulseAudio server for audio output
- Proot-distro preinstalled with pre-configured Ubuntu 22.04 LTS (Jammy Jellyfish) RootFS (see text below for applied modifications)

# Ubuntu 22.04 RootFS contents:
- Simple XFCE4 desktop, which can be used using VNC or Termux-X11 (see instructions below)
- Audio should work out-of-the-box (it works independently from the display output, so you will hear audio as long as you leave the proot-distro opened).
- Contains up-to-date packages as 2023-01-15 (you can easily update using apt update/apt upgrade)
- Firefox preinstalled & configured to use non-snap version for updating (compatible with proot)
- Visual Studio Code pre-installed (with extension support)
- Box86 and Box64 preinstalled and pre-configured (with latest version as of 2023-01-15, you can also find auto updater scripts in tools folder)
- Bash support for Box86 and Box64 is also included, so you can use shell scripts, see more info at https://box86.org/2022/09/running-bash-with-box86-box64/
- Enabled multiarch environment (armhf, arm64, i386, amd64), you can use "apt install packagename:architecture) to install non arm64 packages (i386 packages can be used with box86, while amd64 can be used with box64).
- Wine 7.0.1 (for 32bit) and Wine 6.0.1 (for 64bit) preinstalled, both has Mono, Gecko, and Visual C++ runtimes preinstalled. You can start each version using shortcuts on desktop (with or without GPU acceleration). The desktop shortcuts start 7-Zip File Manager by default.

## System requirements:
- Android device with ARM64 processor and 64-bit Android OS (32-bit ARM or x86/x64 CPUs, and 32-bit operating systems are not supported)
- About ~10GB of free space (however, at least 16GB of free storage is recommended)
- 2GB of RAM (4GB is recommended, or even more if you want to play games)
- GPU with Vulkan support for VirGL graphics acceleration (Mali, Adreno, RDNA2 should be fine)
- Root is not required!
- For Android 12 or higher, you also need to modify your phone settings to prevent Android from killing proot-distro automatically: https://ivonblog.com/en-us/posts/fix-termux-signal9-error/
## Download:
Updated as of 2023-01-21:
https://drive.google.com/file/d/1nu2GIXjM9Qa8bRh6ZHbY8aGmllVsjqWz/view?usp=share_link

## How to install?
> **WARNING!** If you have an existing Termux installation, installing this prefix file will WIPE your existing Termux prefix folder, including all of your installed and configured packages. The home directory (which is the default directory that you see on startup) should not be affected, but I still recommend making a backup of any important files there.

- Download prefix file (see link in Downloads section, available in .7z format)
- Unpack the 7-Zip file and place the .tar file on your phone storage
- Install Termux from F-Droid (please do not use Google Play Store version)
- In Termux, use "termux-setup-storage" command and allow application to reach shared storage
- Use  "termux-restore ./storage/shared/<path to .tar file>" command to install the prefix. If done, restart Termux.

> Example: If you placed the .tar file on the root folder of your shared storage, the command would be "termux-restore ./storage/shared/termux-ubuntubox-v2.tar"

- You are ready to go! (see section below if you also want to use Termux-X11)
## How to setup Termux-X11 as a display option?
Termux-X11 has better performance compared to VNC, but it is a bit more difficult to set up, and also more difficult to control (as unlike many VNC viewers, Termux-X11 does not offer pan&zoom feature for display, and it does not really include touchscreen-friendly control options).

Termux-X11 is a separate Android app, so you also have to install that.

 - Use the official GitHub Actions page, and download the latest version there (download link will only work if you are logged in to GitHub): https://github.com/termux/termux-x11/actions/workflows/debug_build.yml
 - Unpack the .apk file from the archive, and install it on your Android device.
	
In Termux, open ~/.termux/termux.properties file for edit (for example, you can use "nano ~/.termux/termux.properties"), and set the allow-external-apps property to true (also uncomment the line if needed), then restart Termux.

After installing app, I recommend you to start at least for a first time (without any pre-configuration). You should see nothing interesting, as currently we do not have a server outputting a dispaly, so you can close the app through the notification panel.

## How to use?

 - Open Termux.
 - You have two options:
	 - **Option A (CLI/VNC):** If you want to use Ubuntu using VNC server (or CLI only), you should use the command "startubuntubox".  This will start VirGL server for graphics acceleration, start PulseAudio for audio output, and, of course, start the Ubuntu proot-distro. You should also see a short greeting message in the notification explaining the most important commands. You can start the built in XFCE4 desktop VNC server using "vncserver :1" command. The server will be available on "localhost:5901", the login password is "ubuntu".
	 - **Option B (Termux-X11):** If you want to use Ubuntu using Termux-X11, you should use the command "startubuntubox-termux-x11". This will also enable VirGL and PulseAudio, and will also start Termux-X11 application automatically. However, display output is not functional at this point, so switch back to Termux, and use the command "~/start-x11-server.sh" to start XFCE desktop. After that, switch back to Termux-X11 app, and you should see the desktop there.

To stop the XFCE4 desktop, use the "Exit VNC" button on the desktop if you use VNC, or stop the Termux-X11 app using the notification panel if you use X11 connection. To exit from the Ubuntu proot-distro, use the "exit" command.

## Notes about GPU acceleration (VirGL)
	
> So far, only Samsung XClipse 920 GPU (Exynos 2200) is currently confirmed to work, while I experienced immediate crash on Adreno 650 (Snapdragon 865). Other GPUs may or may not work, feel free to try it and feedback on the Reddit thread (see on the bottom of page).
	
GPU acceleration is not enabled by default, as it is not very stable at the moment. You can use the 'source ~/enable-gpu-acc.sh' command to enable GPU acceleration for the current terminal session.
For Wine, you can see separate shortcuts on the desktop for launching in GPU accelerated mode.

Currently, GPU acceleration is avaiable for OpenGL 4.0 or below. However, graphical problems, or app crashes are quite common, so if you are experiencing problems, you can always stick with software rendering (llvmpipe).

GPU acceleration should work with any Android device as long as it has Vulkan support (so both Mali, Adreno and RDNA2 GPUs should work). However, I only tested it on one device so far: Samsung Galaxy S22 (Exynos 2200, RDNA2-based GPU).

For the best performance, using Termux-X11 is heavily recommended when using GPU acceleration (using VNC takes a massive hit on performance).

## Notes about Wine

Both 32-bit and 64-bit versions of Wine are included. Please note, each version has its own file system, so apps installed on one version can't be accessed from the other. If possible, always use the 32 bit version of an app and 32 bit version of Wine, as box86 is much more stable, and more compatible at the moment.
For some reason, drives can not be seen in 64 bit Wine file manager, but you can reach the C:\ drive and your phone's sdcard folder using the "Favourites" tab in 7-Zip. This problem does not affect the 32 bit version.

Please note, the shortcuts that Windows installers will place on your desktop can't be directly used to start the apps. This is because the lack of binfmt support, i386 and amd64 applications can't be detected automatically. So, you should use the file manager inside of Wine to launch Windows applications.

## Compatibility (compared to ExaGear)
As the current state, box86 and box64 is still in  heavy development, and proot-distro also has some drawbacks, both of which makes app and game compatibility a "hit-or-miss" option. So if all you want is to play and enjoy Windows games, I think it's better to use a modified version of Exagear for the best compatibility. However, if you like tinkering, or also want to use native Linux apps, this proot-distro may suit your needs.

## Credits
I would like to thank everyone for their development, support and solutions, including:
 - Termux developers
 - Termux-X11 developers
 - ptitSeb (and contributors) for developing box86 and box64
 - The whole r/termux community for their tips and guides
 - Wine developers
 - Ivon's blog for creating easy guides to install VirGL server and Termux-X11 (both of which are pre-configured in this project)

## Support, discussion
See https://www.reddit.com/r/termux/comments/10hu1x7/termux_prefix_with_preinstalled_ubuntu_2204_proot/ for the latest feedbacks.
