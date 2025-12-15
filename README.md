# k2plus-full-improvements
Full k2 plus improvements learned so far


Prerequsite

1. Fresh Install 

#### Installing K2 camera mod (I like this better than the one on the k2-improvements)
* `python -c "from six.moves import urllib; urllib.request.urlretrieve('https://github.com/DnG-Crafts/K2-Camera/archive/refs/heads/main.zip', '/root/main.zip')"`
* `python -c "import shutil; shutil.unpack_archive('/root/main.zip', '/root/')"`
* `sh ~/K2-Camera-main/install.sh`

### part 1 - bootstrap:
* copy bootstrap folder to main config folder using Fluidd
* `sh /mnt/UDISK/printer_data/config/bootstrap/bootstrap.sh`

### Prepare SSH for git management
* `mkdir -p ~/.ssh`
* `chmod 700 ~/.ssh`
* copy files from D:\3dPrinters\K2Plus\Fresh Install to it
* `chmod 600 ~/.ssh/id_rsa`
* `chmod 644 ~/.ssh/id_rsa.pub`
* `opkg install openssh-client`

### Init Git
* `cd printer_data/config/`
* `git init`
* `git remote add origin git@github.com:USERNAME/k2plus.git`
* `git config --global user.name "YOUR_NAME"`
* `git config --global user.email "YOUR_EMAIL@gmail.com"`

### Start new Git branch / Coming from previous code
* `mkdir freshinstall`
* `mv bootstrap box.cfg factory_printer.cfg printer* gcode_macro.cfg sensorless.cfg freshinstall/`
* `git pull origin master`
* `cp -r freshinstall/* .`
* `rm -rf freshinstall printer-*`
* `git status`
* `git add .`
* `git commit -m "fresh install"`
* `git push --set-upstream origin master`
* `git checkout -b start_version_X`

## part 2 - after running improvements script:
* `cd ~`
* `vim k2-improvements/no-carto.sh` - put # on webcam, fluid and klipper
* `chmod +x k2-improvements/features/resonance-tester/install.sh`
* `sh /mnt/UDISK/root/k2-improvements/no-carto.sh`
* `cd printer_data/config/custom/`
* `cp /mnt/UDISK/root/k2-improvements/features/macros/bed_mesh/bed_mesh.cfg .`
* `cp /mnt/UDISK/root/k2-improvements/features/macros/m191/m191.cfg .`
* `cp /mnt/UDISK/root/k2-improvements/features/screws_tilt_adjust/screws_tilt_adjust.cfg .`
* `cp /mnt/UDISK/root/k2-improvements/features/macros/start_print/start_print.cfg .`
* `cp /mnt/UDISK/root/k2-improvements/features/resonance-tester/resonance-tester.cfg .`
* `cd ~`
* `vim moonraker/moonraker.conf` add to end:
```
[spoolman]
server: http://YOUR_SPOOLMAN_IP:7912
#   URL to the Spoolman instance. This parameter must be provided.
sync_rate: 5
#   The interval, in seconds, between sync requests with the
#   Spoolman server.  The default is 5.
```
* `cp moonraker/moonraker.conf printer_data/config/`
* `git add .`
* `git push --set-remote origin/start_version_X`