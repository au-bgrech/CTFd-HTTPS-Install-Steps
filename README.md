## Requirements
1. Purchase a Domain Name (Namecheap, etc.)
2. Have an Ubuntu 24.04 LTS with a Public IP address (Linode, etc.)
3. Ensure your purchased domain resolves to your Ubuntu's public IP address. (e.g., `ping ctf-example.teameffort.work` should show your IP address and be reachable)

## Install

1. Complete the [CTFd Docker Install](https://docs.ctfd.io/docs/deployment/installation#docker)
   1. `adduser ctf`
   2. `usermod -aG sudo ctf`
   3. `su ctf`
   4. `cd ~`
   5. `sudo apt update`
   6. `sudo apt upgrade`
   7. `sudo apt install docker.io docker-compose`
   8. `git clone https://github.com/CTFd/CTFd.git`
   9. `cd CTFd`
   10. `head -c 64 /dev/urandom > .ctfd_secret_key`

2. Get an HTTPS certificate from Let's Encrypt
   1. `sudo snap install core`
   2. `sudo snap refresh core`
   3. `sudo snap install --classic certbot`
   4. `sudo ln -s /snap/bin/certbot /usr/bin/certbot`
   5. `sudo certbot certonly`
   6. Select Option 1 (spin up a temporary webserver)
   7. Enter your email address
   8. Agree to the terms
   9. Allow (or not) to share your info
   10. Enter domain name (e.g., ctf-example.teameffort.work)
   11. Take note of where the certificate and key are saved to (e.g., /etc/letsencrypt/live/ctf-example.teameffort.work/<fullchain.pem><privkey.pem>)

3. Configure nginx for HTTPS
   1. `cd conf/nginx`
   2. `mv http.conf https.conf`
   3. `vim https.conf`
   - Replace all contents of https.conf with the settings in this [example](https://github.com/au-bgrech/CTFd-HTTPS-Install-Steps/blob/main/https.conf%20(example)). Ensure to replace the ctf.teameffort.work with your own domain!
   4. `cd ../..`
   5. `vim docker-compose.yml`
   - The docker-compose.yml contains an nginx section. Ensure it matches this [example](https://github.com/au-bgrech/CTFd-HTTPS-Install-Steps/blob/main/docker-compose.yml%20(nginx%20section)). Ensure to replace the ctf.teameffort.work with your own domain!

4. Start the CTFd instance!
   1. `sudo docker-compose up -d`
   2. On a desktop machine, open a web browser and navigate to your domain via HTTPS (e.g., https://ctf-example.teameffort.work) and begin setting up the CTFd platform.
