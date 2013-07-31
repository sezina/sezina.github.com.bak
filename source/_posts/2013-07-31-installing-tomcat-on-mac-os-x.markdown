---
layout: post
title: "Installing Tomcat on Mac(OS X)"
date: 2013-07-31 15:13
comments: true
categories: [Tomcat, OS X]
---

Step 1. Download Tomcat7 Binary Distributions from Tomcat homepage.
Step 2. Move the downloaded directory to `/usr/local`:
```bash
sudo mv ~/Downloads/Apache-tomcat-7.0.24 /usr/local
```

Step 3. Create a symbolic link in `/Library`:
```bash
sudo ln -s /usr/local/Apache-tomcat-7.0.24 /Library/Tomcat
```

Step 4. Change ownership of `/Library/Tomcat`:
```bash
sudo chown -R <user_name> /Library/Tomcat
```

Step 5. Make all scripts executable:
```bash
sudo chmod +x /Library/Tomcat/bin/*.sh
```

Step 6. Use command `/Library/Tomcat/bin/startup.sh` to start tomcat, then brower `http://localhost:8080`.