# TechSympo 2025 – Static Site

This is a lightweight, responsive HTML + CSS website for a university's national-level technical symposium.

## Files
- `index.html` – single-page site with sections: About, Tracks, Schedule, Speakers, Venue, Sponsors, CFP, Registration, FAQ, Contact
- `styles.css` – modern, responsive styles (no JS)
- `assets/logo.svg`, `assets/wave.svg` – lightweight graphics

## How to deploy on an AWS EC2 (Amazon Linux 2) with Nginx
1. Launch EC2 with **Amazon Linux 2 AMI**, attach a **security group** allowing inbound TCP **80** (HTTP) and **22** (SSH); optionally **443** for TLS.
2. SSH into the instance:
   ```bash
   ssh ec2-user@<PUBLIC_IP>
   ```
3. Install & enable Nginx:
   ```bash
   sudo yum update -y
   sudo amazon-linux-extras enable nginx1
   sudo yum install -y nginx
   sudo systemctl enable nginx
   sudo systemctl start nginx
   ```
4. Copy the site files to the default web root:
   ```bash
   # From your local machine terminal
   scp -r symposium_site/* ec2-user@<PUBLIC_IP>:/home/ec2-user/
   # On the instance
   sudo rm -rf /usr/share/nginx/html/*
   sudo cp -r /home/ec2-user/* /usr/share/nginx/html/
   sudo systemctl restart nginx
   ```
5. (Optional) Set up a domain + HTTPS (Let’s Encrypt with Certbot):
   ```bash
   sudo amazon-linux-extras enable epel
   sudo yum install -y certbot python3-certbot-nginx
   sudo certbot --nginx -d yourdomain.example
   ```

## Notes
- Replace placeholder text (dates, venue, sponsors) in `index.html`.
- Replace the registration form with your Google Form or backend endpoint.
- To add more pages, create new `.html` files and link from the navbar.
