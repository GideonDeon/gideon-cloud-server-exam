NAME: Gideon Oluwanifemi Gideon  
STUDENT ID: ALT/SOE/024/1862

Overview  
This project demonstrates deploying a modern cloud-based landing page using an Ubuntu server, Nginx web server, and a Node.js reverse proxy. It includes server provisioning, firewall configuration, and dynamic web content.

---

Steps Taken

1. Server Provisioning
- Launched an Ubuntu instance on **AWS EC2**
- Updated system packages and installed necessary software (Node.js, Nginx, Git)
- Set up basic security (SSH access, optional UFW)

2. Web Server Setup
- Installed **Nginx** as the main web server
- Configured **Nginx as a reverse proxy** to forward traffic to a Node.js app running on `localhost:3000`
  ```
  server {
    listen 80;
    server_name _;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
  }
  ```
3. Node.js App Setup
- Created a basic `app.js` using Node.js and installed Express to serve static HTML content
- App serves landing page files (HTML, CSS, assets) from `/var/www/landingpage/a.iHealth`
- App listens on port `3000`, behind Nginx
- Installed and managed with PM2 for background process handling

```javascript
const express = require('express');
const path = require('path');
const app = express();

// Serve static files from the cloned directory
app.use(express.static(path.join(__dirname, '../landingpage/a.iHealth')));

// Default route
app.get('/', (req, res) => {
  res.sendFile(path.join(__dirname, '../landingpage/a.iHealth/index.html'));
});

const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Server running at http://localhost:${PORT}`);
});
```
4. Landing Page  
    Custom landing page includes:
      - Full name and role (e.g. Lead Cloud Engineer)
      - Project title: The Future of AI-Powered Health
      - startup pitch
      - Bio with skills, experience, and education    
      - Page styled using Tailwind CSS  
      - Minor animation applied to the page header for interactivity
   
6. Networking & Security
   - Allowed HTTP (port 80) and HTTPS (port 443) via AWS security group
   - UFW firewall disabled for this setup
   - SSL not implemented, as this deployment is IP-based without a domain (NOTE: I understand the use of Let's Encrypt (certbot) but it requires a domain, which i could not purchase for certain reasons)

7.  ACCESS
    - Public IP: http://13.42.37.199/

![Screen_shot_1](https://github.com/user-attachments/assets/b1a4ea54-37a7-4c05-93f3-69314fcb2002)  

![Screen_shot_2](https://github.com/user-attachments/assets/331af5d3-0430-4381-a082-cd5605454fc9)


Tools & Technologies
- Ubuntu 22.04 (EC2)
- Node.js + Express
- Nginx
- PM2 (process manager)
- Git
- Tailwind CSS  

Git repo link to the landingpage:  
https://github.com/GideonDeon/a.iHealth
