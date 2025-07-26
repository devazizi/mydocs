## To recovery root password( change it )

### I will show you a simple good trick you should take you mv to rescue mode 
### For example in **VCenter**  
- [x] Launch console
- [x] Press Ctrl+Alt+Delete after reboot your vm
- [x] When GRUB menu appeared quickly press e button on your keyboard
- [x] Modify kernel : look for a something like that linux /boot/vmlinuz-... ro .. and change it to linux /boot/vmlinuz-... rw init=/bin/bash
- [x] Press Ctrl+X or F10 to boot with the modified settings.
- [x] Run command: ```passwd```
- [x] Remount Filesystem : ```mount -o remount,rw /```
- [x] Reboot your vm: exec /sbin/init

### your root password successfully changed