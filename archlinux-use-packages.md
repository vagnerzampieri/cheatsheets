# manage wifi
sudo ip link set wlp1s0 down
sudo wifi-menu

# mixer sounds https://www.youtube.com/watch?v=3cuuLQ19WTA
sudo pacman -S alsa-utils
alsamixer

# mount disks and pen drives https://github.com/coldfix/udiskie/wiki/Usage
udiskie &

Another references:
https://i3wm.org/docs/userguide.html i3 user guide
https://github.com/jelly/Dotfiles/blob/master/.config/i3/config i3 config
https://gist.github.com/miguelmota/e51b9a3c3a42202f058215ebb1d0ca72 i3 Window Manager Cheat Sheet
https://vim.rtorr.com/ VIM Cheat Sheet
https://fontawesome.com/cheatsheet Font Awesome Cheat Sheet

# purge
sudo pacman -Rns package

# monitors
arandr
