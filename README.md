## Requirements
1. Purchase a Domain Name (Namecheap, etc.)
2. Have an Ubuntu 22.04 LTS with a Public IP address (Linode, etc.)
3. Ensure your purchased domain resolves to your Ubuntu's public IP address. (e.g., `ping ctf-example.teameffort.work` should show your IP address and be reachable)

## Install

1. Complete the [CTFd Docker Install](https://docs.ctfd.io/docs/deployment/installation#docker).
   1. `apt update`
   2. `apt install docker.io docker-compose`
   3. `git clone https://github.com/CTFd/CTFd.git`
   4. `cd CTFd`
   5. `head -c 64 /dev/urandom > .ctfd_secret_key`
 
2. Get an HTTPS certificate from Let's Encrypt
   1. `snap install core; sudo snap refresh core`
   2. `snap install --classic certbot`
   3. `sudo ln -s /snap/bin/certbot /usr/bin/certbot`
   4. `sudo certbot certonly`
   5. Select Option 1 (spin up a temporary webserver)
   6. Enter your email address
   7. Agree to the terms
   8. Allow (or not) to share your info
   9. Enter domain name (e.g., ctf-example.teameffort.work)
   10. Take note of where the certificate and key are saved to (e.g., /etc/letsencrypt/live/ctf-example.teameffort.work/<fullchain.pem><privkey.pem>)

3. Configure nginx for HTTPS
   1. `cd conf/nginx`
   2. `mv http.conf https.conf`
   3. Replace all contents of https.conf with the settings in this [example](https://github.com/au-bgrech/CTFd-HTTPS-Install-Steps/blob/main/https.conf%20(example)). Ensure to replace the ctf.teameffort.work with your own domain!
   4. `cd ../..`
   5. The docker-compose.yml contains an nginx section. Ensure it matches this [example](https://github.com/au-bgrech/CTFd-HTTPS-Install-Steps/blob/main/docker-compose.yml%20(nginx%20section)). Ensure to replace the ctf.teameffort.work with your own domain!

4. Start the CTFd instance!
   1. `docker-compose --restart=always up -d`
