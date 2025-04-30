# Ubuntu Setup
This is a setup, sepcifically for Ubuntu 25.04, to get a home desktop setup how Karl preffers it, ready for gaming and other

## Initial
Update existing packages
```
sudo apt update
sudo apt upgrade
```
### flatpak
Install flatpak, and add flathub. This is how we will install most of the software
https://flathub.org/setup/Ubuntu
```
sudo apt install flatpak
sudo apt install gnome-software-plugin-flatpak
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

Update flatpak software with
```
flatpak update
```
### GPU drivers
If you are running an AMD gpu, then your drivers come pre-installed with Linux and you are a better person.

For Nvidia drivers, open the Additonal Drivers app and select the latest available version. There are often multi versions, usually stick with the Nvidia proprietary ones as they will be more up to date.

## Gaming

### Steam
Install Steam
```
flatpak install flathub com.valvesoftware.Steam
```
Launch and sign into Steam, then navigate to Steam > Settings > Compatibility > Enable Steam Play for all other titles

Steam games will install in your homedir under ~/.var/app/com.valvesoftware.Steam/.local/share/Steam/steamapps/common

Install steam-devices, which just automatically changes some file permissions to allow steam to use hardware devices to access controllers
```
sudo apt install steam-devices
```

### Proton ge
Proton is a Windows compatability layer, built on top of WINE. Steam already comes with proton, but we will be adding proton ge wich is a fork that includes extras such as extra video codecs for more compatability.

I specifically encountered errors in Blue Prince where the videos won't render (and are replaced with a TV test colour screen) without this.

We will be installing a GUI proton manager ProtonUp-Qt to do most of the work for us, install this with
```
flatpak install flathub net.davidotek.pupgui2
```
Launch ProtonUp-Qt, and you should see "Steam Flatpak" in the install for dropdown. Click Add version, GE-Proton with the latest version (should already be selected by default).

Restart Steam, now you can change individual games to run on proton ge (Right click Properties > Compatibility > TICK Force the user of a specific Steam Play > select GE-Proton from the list), or change the default from the dropdown under Steam > Settings > Compatibility

ProtonUp-QT can also be used to install other proton/wine options for Heroic Games and Lutris. Use this if you enconter any issues with a particular game.

### Epic games
Epic has no linux installer, but there are two alternative options. You could install the Epic app via Lutris using the same instructions as EA App, or you could use Heroic Games Launcher which I prefer.

Follow either the Heroic Games Launcher or Lutris guides.

### GOG
GOG is in the same situation as Epic games, and can be installed with Lutris, or the games can be run in Heroic Games Launcher without needing the GOG app.

Follow either the Heroic Games Launcher or Lutris guides.

### EA App
EA games cannot be played via the Heroic Games Launcher, so you will need to follow the Lutris guide for this.

### Heroic Games Launcher
Install Heroic Games Launcher with
```
flatpak install flathub com.heroicgameslauncher.hgl
```
Lauch the app, navigate to Log in > Then login to Epic Games and/or GOG

### Lutris
Install Lutris with
```
flatpak install flathub net.lutris.Lutris
```
Launch the app, and it should start downloading required extras. Click the User icon next to the source that you plan on using, and follow the on-screen prompts to download specifc extras to run it.

Note with EA App, after originally signing in I was presented with a mobile app page that crashed, since then I have a Play icon next to the source that launches the full app which works.

## Discord
Install discord with
```
flatpak install flathub com.discordapp.Discord
```

## Floorp
Fuck firefox. Use this non-user-tracking fork instead
```
flatpak install flathub one.ablaze.floorp
```
Useful plugins:
* https://addons.mozilla.org/en-US/firefox/addon/ublock-origin/
* https://addons.mozilla.org/en-US/firefox/addon/bitwarden-password-manager/
* https://addons.mozilla.org/en-US/firefox/addon/sponsorblock/

## Moving installs to another drive
Once you have a drive mounted (by adding it to /etc/fstab) you can simply copy contents that you want on it across, then setup a symlink, and the app can contiune to load files via the symlinked path as though the files are still in that location.

Note with these examples that slashes at the end matter, (mv myfolder /mnt/slowharddrive) will move myfolder along with it's contents, while (mv myfolder/ /mnt/slowharddrive) will leave the myfolder in place but will move all it's contents.

Quotes are optional, but are required if you want to add spaces to a path.

Example, moving all Steam games onto a drive I have mounted at /mnt/slowharddrive/
```
sudo mv ~/.var/app/com.valvesoftware.Steam/.local/share/Steam/steamapps /mnt/showhardrive
sudo ln /mnt/showhardrive/steamapps ~/.var/app/com.valvesoftware.Steam/.local/share/Steam/steamapps -s
```

Move specifically just Helldivers 2
```
sudo mv '~/.var/app/com.valvesoftware.Steam/.local/share/Steam/steamapps/common/Helldivers 2' /mnt/showhardrive
sudo ln /mnt/showhardrive/steamapps '~/.var/app/com.valvesoftware.Steam/.local/share/Steam/steamapps/common/Helldivers 2' -s
```

## Other userful linux cli apps to install

### ncdu
Show the sizes of all directories and explore them (like windirstat etc)
```
sudo apt install ncdu
```
Use -x to ignore other drives, and sudo to get system files. Expore all drive contents with
```
sudo ncdu -x /
```

### vscode
Coding / development tool
```
sudo snap install code --classic
```

### sl
draws a cool train when you mess up typing ls
```
sudo apt install sl
```

## Updating everything
sudo apt upgrade will install updates for software installed via apt, but not flatpaks or other hardware devices. Note: snaps will auto-update

I reccomend creating a script in your homedir call update-all, pasting the contents below, then running this to update everything all at once
```
nano ~/update-all
# Paste the contents
# Save and quit with: CTRL+X y enter
# Make it executable with
chmod +x ~/update-all 
```
File contents
```
sudo apt update && sudo apt upgrade # apt updates (most of the system)
echo '======= apt updates done ======='
sudo flatpak update # update flatpaks
echo '======= flatpak updates done ======='
sudo fwupdmgr get-updates # firmware updates
echo '======= firmware updates done ======='
echo '======= all updates done ======='
```
