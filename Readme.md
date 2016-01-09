# decrypt-initcpio

This is a hook for mkinitcpio allowing to decrypt devices with keyfiles. It was built for myself and I am not sure if this is useful for anyone else on earth. **It was built for mkinitcpio on arch linux and is not tested anywhere else.**

Decrypting with keyfiles is something you can already do with `/etc/mtab`. But you still have to unlock each device on it's own. This hooks does something different: You can have all keyfiles stored in an encrypted partition. You must enter the password of this encrpytion and then the hook decrypts all other partitions.

## Installation

0. Install the package by moving the `hook` file into you hooks directory and the `install` file into you install directory. Rename both as `decrypt`. Now move the configuration to `/etc/initcpio` and call it decryot as well. Or if you are lazy, check out my [PKGBUILDs](https://github.com/dasJ/pkgbuilds).
1. Create a partition (unencrypted) on your MBR/GPT with a common filesystem (I use ext4, but FAT should also be okay). Now note the UUID of this filesystem, as you will need it later. From now on, this will be called the keystore.
2. You need to create a keyfile for each partition/disk/... that should be unlocked and add it to this LUKS container. Put the keyfile into the keystore with **the same name it will use later when you have it decrypted**.
3. Modify the config at `/etc/initcpio/decrypt`. First, add the keystore UUID into the variable and then fill the `volumes`-variable. It is a colon-separated list with the volumes to be unlocked. Each volume consists of the UUID and the name it will have after it's unlocked (which is the same as the keyfile, remember?). Those two values are pipe-separated.
4. Add the hook to your mkinitcpio hooks at the right location (maybe between block and filesystems). Use the keyboard hook before this hook to have the correct keyboard layout if yours is not the american.
5. Rebuild your initial ramdisk.
6. Reboot and pray.

## Disclaimer

I wrote this during my free time. You can feel free to use this in a production environment, if you know what you are doing. I do not take any responsibility if something goes bad.

decrypt-initcpio may be broken, have security issues, or not work at all. Feel free to open github issues/prs without the guaratee of a quick answer :)

