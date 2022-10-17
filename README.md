# NBDE
How to configure Network Bound Disk Encryption (Clevis and Tang) to automatically unlock the boot drive at boot

Install Fedora and in the drive section of Anaconda, choose to encrypt the drive.

On another server, install tang:

```sudo dnf install tang -y```

Make sure port 80 is open in the firewall

```sudo firewall-cmd --list-all```

If you need to open port 80

```sudo firewall-cmd --add-port=80/tcp```

```sudo firewall-cmd --runtime-to-permanent```

Start the tang server

```sudo systemctl enable --now tangd.socket```

```sudo systemctl status tangd.socket```

On my system, installing tang generated a key and you can see this key by running

```sudo tang-show-keys```

Back on the encrypted system, install clevis-luks and clevis-dracut.

```sudo dnf install clevis-luks clevis-dracut```

And bind the encrypted partition to the tang server (change my.tang.com to the FQDN of your tang server)

```clevis luks bind -d /dev/sdb3 tang '{"url":"http://my.tang.com"}'```

Reboot and watch magic happen as your computer automatically unlocks.  

If you need help finding the encrypted drive, install and start cockpit, it can be really helpful.

```sudo dnf install -y cockpit cockpit-bridge cockpit-file-sharing cockpit-machines cockpit-navigator cockpit-networkmanager cockpit-ostree cockpit-packagekit cockpit-pcp cockpit-selinux cockpit-podman cockpit-storaged cockpit-system cockpit-ws```

```sudo firewall-cmd --add-service=cockpit```

```sudo systemctl enable --now cockpit.socket```
