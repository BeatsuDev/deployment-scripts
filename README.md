# Deployment scripts for zero downtime website updates
Deployment step by step:
 - Generate static website in GitHub actions
 - Compress and send tarball to server at `/var/www/` via SSH (host IP and key are added to GitHub secrets)
 - Run `./update.sh <tarball>` on server at `/var/www/` working directory
 - Voila!

Update strategy:
 - `cd /var/www/`
 - Extract tarball to folder named: `"%D.%b.%Y[-<n>]"` (e.g. 29.nov.2024, or 29.nov.2024-1 if one already exists for today)
 - Symlink html to new, dated folder
 - `/var/www/html` symlink is served by HTTP web server
