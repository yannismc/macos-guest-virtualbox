## Push-button installer of macOS on VirtualBox
This is a Bash script that creates a VirtualBox guest macOS virtual machine by downloading unmodified macOS installation files directly from Apple servers.

A default install only requires the user to sit patiently and, less than ten times, press enter when prompted by the script, without interacting with the virtual machine. The script doesn't install any closed-source additions or extra bootloaders. Tested on Cygwin. Works on macOS and WSL, should work on most Linux distros.

### macOS Catalina (10.15), Mojave (10.14), and High Sierra (10.13) currently supported
macOS Catalina version 10.15.2 and higher is only supported by VirtualBox version 6.1.4 and higher. A workaround for lower versions of VirtualBox which involves using earlier versions of `boot.efi` is [described in issue 134](https://github.com/myspaghetti/macos-guest-virtualbox/issues/134#issuecomment-583216307).

### Pre-installation notes
- Create machine named "macOS" at VirtualBox
- PATH should contain the path to VBoxManage.exe

## Documentation
Documentation can be viewed by executing the command `./macos-guest-virtualbox.sh documentation`

## iCloud and iMessage connectivity and NVRAM
iCloud, iMessage, and other connected Apple services require a valid device name and serial number, board ID and serial number, and other genuine (or genuine-like) Apple parameters. These can be set in NVRAM by editing the script. See `documentation` for further information.

## Storage size
The script by default assigns a target virtual disk storage size of 80GB, which is populated to about 20GB on the host on initial installation. After the installation is complete, the storage size may be increased. See `documentation` for further information.

## Unsupported features
Developing and maintaining VirtualBox or macOS features is beyond the scope of this script. Some features may behave unexpectedly, such as USB device support, audio support, FileVault boot password prompt support, and other features.

### Performance
After successfully creating a working macOS virtual machine, consider importing it into QEMU/KVM so it can run with hardware passthrough at near-native performance. QEMU/KVM requires additional configuration that is beyond the scope of  the script.

### Audio
macOS may not support any built-in VirtualBox audio controllers. The bootloader [OpenCore](https://github.com/acidanthera/OpenCorePkg/releases) may be able to load open-source audio drivers in VirtualBox, providing the configuration for STAC9221 (Intel HD Audio) or SigmaTel STAC9700,83,84 (ICH AC97) is available.

### FileVault
The VirtualBox EFI implementation does not properly load the FileVault full disk encryption password prompt upon boot. The bootloader [OpenCore](https://github.com/acidanthera/OpenCorePkg/releases/tag/0.5.7) is able to load the password prompt with the parameter `ProvideConsoleGop` set to `true`. See bare [config.plist](https://github.com/myspaghetti/macos-guest-virtualbox/files/4455100/config.plist.txt).

## Dependencies
All the dependencies should be available through a package manager:  
`bash` `coreutils` `gzip` `unzip` `wget` `xxd` `dmg2img`  `virtualbox`

* [VirtualBox](https://www.virtualbox.org/wiki/Downloads)≥6.1.6 with Extension Pack, though versions as low as 5.2 may work.
* GNU `Bash`≥4.3, on Windows run through [Cygwin](https://cygwin.com/install.html) or WSL.
* GNU `coreutils`≥8.22, GNU `gzip`≥1.5, Info-ZIP `unzip`≥v6.0, GNU `wget`≥1.14, `xxd`≥1.7
* `dmg2img`≥1.6.5, on Cygwin the package is not available through the package manager so the script downloads it automatically.
