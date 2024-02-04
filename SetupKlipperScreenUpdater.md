**Adding KlipperScreen to Moonraker's Update Manager**
Moonraker has an update manager which makes keeping your system up to date really easy. To add KlipperScreen to this list, add the following lines to the end of `moonraker.conf`. You should then be able to update KlipperScreen via the web interface.

```
[update_manager KlipperScreen]
type: git_repo
path: ~/KlipperScreen
origin: https://github.com/jordanruthe/KlipperScreen.git
env: ~/.KlipperScreen-env/bin/python
requirements: scripts/KlipperScreen-requirements.txt
install_script: scripts/KlipperScreen-install.sh
managed_services: KlipperScreen
```
