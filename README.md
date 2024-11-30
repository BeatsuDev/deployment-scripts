# Deployment scripts for zero downtime website updates
Deployment step by step:
 - Generate static website in GitHub actions
 - Compress and send tarball to server at `/var/www/` via SSH (host IP and key are added to GitHub secrets)
 - Run `./update <tarball>` on the server with `/var/www/` as working directory 
 - Voila!

Update strategy (this is what the script does):
 - Extract tarball to folder named: `"%D.%b.%Y[-<n>]"` (e.g. 29.nov.2024, or 29.nov.2024-1 if one already exists for today)
 - Symlink `html` to new, dated folder

`/var/www/html` symlink is served by HTTP web server

# Getting the scripts on your server
Want these scripts on your server? Run these commands on the server (SSH into your server first `ssh user@<ip-address/hostname>`,
must be a linux machine and assumes that git is installed)
```bash
cd ~ 
git clone https://github.com/BeatsuDev/deployment-scripts
rm -fr deployment-scripts/.git README.md
sudo chmod +755 deploymenqt-scripts/*
sudo mv deployment-scripts/* /var/www/
rm -fr deployment-scripts
cd -
```

> WARNING: Double check for name collisions before using these scripts, or always prefix the commands with `./` (e.g. `./update <tarball>`, `./update <directory>`).

You should now be able to use the commands `update <tarball>` and `switch <directory>` _while inside `/var/www/`_.
