# Xfce-arch-rice
My xfce rice configs 
#!/bin/bash

=== System Update and Essential Packages ===

echo "[+] Updating system and installing XFCE + essentials..." sudo pacman -Syu --noconfirm sudo pacman -S --noconfirm xfce4 xfce4-goodies 
picom plank neofetch htop btop 
papirus-icon-theme lxappearance 
network-manager-applet 
pavucontrol xfce4-pulseaudio-plugin 
xfce4-whiskermenu-plugin 
archlinux-wallpaper 
gvfs gvfs-mtp thunar-archive-plugin file-roller 
pulseaudio pulseaudio-alsa 
noto-fonts ttf-jetbrains-mono ttf-font-awesome 
git wget curl unzip

=== Enable essential services ===

echo "[+] Enabling NetworkManager..." sudo systemctl enable NetworkManager sudo systemctl start NetworkManager

=== Set up themes and icons ===

echo "[+] Setting dark themes and icons..." git clone https://github.com/dracula/gtk.git ~/Dracula-GTK mkdir -p ~/.themes && cp -r ~/Dracula-GTK ~/.themes/Dracula xfconf-query -c xsettings -p /Net/ThemeName -s "Dracula" xfconf-query -c xsettings -p /Net/IconThemeName -s "Papirus-Dark" xfconf-query -c xsettings -p /Gtk/FontName -s "JetBrains Mono 10"

=== Panel Configuration ===

echo "[+] Setting top panel with system info, clock, and launcher..." xfconf-query -c xfce4-panel -p /panels -n -t int -s 1 xfconf-query -c xfce4-panel -p /panels/panel-1/position -s "p=top" xfconf-query -c xfce4-panel -p /panels/panel-1/size -s 30 xfconf-query -c xfce4-panel -p /panels/panel-1/plugins -t string -s whiskermenu -t string -s separator -t string -s clock -t string -s separator -t string -s pulseaudio -t string -s network-monitor -t string -s systray

=== Set Arch logo for Whisker Menu ===

echo "[+] Adding Arch logo to Whisker menu..." mkdir -p ~/.icons && cp /usr/share/pixmaps/archlinux-logo.png /.icons/arch.png'"

=== Bottom Dock ===

echo "[+] Setting up Plank dock at bottom..." mkdir -p ~/.config/plank/dock1/ cat <<EOF > ~/.config/plank/dock1/settings [PlankDockPreferences] DockItems=firefox.dockitem; Alignment=center AutoHide=true DockPosition=bottom HideDelay=200 HideEffect=zoom IconSize=32 ItemsAlignment=center LockItems=false ZoomEnabled=true ZoomPercent=120 EOF

mkdir -p ~/.config/autostart cat <<EOF > ~/.config/autostart/plank.desktop [Desktop Entry] Type=Application Exec=plank Hidden=false NoDisplay=false X-GNOME-Autostart-enabled=true Name=Plank EOF

=== Picom (Compositor) for transparency and animations ===

echo "[+] Configuring Picom for effects..." mkdir -p ~/.config/picom cat <<EOF > ~/.config/picom/picom.conf backend = "glx"; vsync = true; fading = true; fade-in-step = 0.03; fade-out-step = 0.03; shadow = true; shadow-radius = 12; shadow-offset-x = -15; shadow-offset-y = -15; blur-method = "dual_kawase"; blur-strength = 7; corner-radius = 12; EOF

echo "picom --config ~/.config/picom/picom.conf &" >> ~/.xprofile

=== Final Touch ===

echo "[+] Setting wallpaper and cleaning up..." wget -O ~/Pictures/wallpaper.jpg https://wallpapercave.com/wp/wp11285954.jpg dbus-launch xfconf-query -c xfce4-desktop -p /backdrop/screen0/monitor0/image-path -s ~/Pictures/wallpaper.jpg

=== Done ===

echo "[+] XFCE Cyberpunk Rice complete! Reboot and enjoy." echo "[+] If dock or panel missing, run: xfce4-panel & plank &"
