# Setup Guide for MyApp

This guide explains how to deploy MyApp on an Ubuntu server with Apache Tomcat 10, including optional Jenkins integration for automated deployment.

## Prerequisites

- Ubuntu server or EC2 instance
- Apache Tomcat 10 installed
- Java 11 or later installed
- Optional: Jenkins server for CI/CD

---

## Step 1: Verify Tomcat Installation

Check if Tomcat is running:

```bash
curl -I http://localhost:8080
```

You should see `HTTP/1.1 200 OK`.

---

## Step 2: Configure Tomcat Manager User

Edit the Tomcat users file:

```bash
sudo nano /etc/tomcat10/tomcat-users.xml
```

Add the following:

```xml
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<user username="jenkins" password="Jenkins123!" roles="manager-gui,manager-script"/>
```

Save and exit.

---

## Step 3: Install Tomcat Manager Webapp (if not installed)

```bash
sudo apt update
sudo apt install tomcat10-admin
```

---

## Step 4: Restart Tomcat

```bash
sudo systemctl restart tomcat10
```

Or, if installed manually:

```bash
sudo /opt/tomcat10/bin/shutdown.sh
sudo /opt/tomcat10/bin/startup.sh
```

---

## Step 5: Verify Manager Access

Test the `/manager/text` endpoint:

```bash
curl -u jenkins:Jenkins123! http://localhost:8080/manager/text/list
```

Expected output:

```
OK - Listed applications for virtual host [localhost]
```

---

## Step 6: Deploy WAR File (Manual)

Copy your WAR file to Tomcat’s `webapps` folder:

```bash
sudo cp myapp.war /var/lib/tomcat10/webapps/
```

Tomcat will automatically deploy it. Access the app at:

```
http://localhost:8080/myapp/
```

---

## Step 7: Deploy WAR via Jenkins (Optional)

Jenkins can deploy WAR files remotely using the manager-text API:

```bash
curl -T myapp.war "http://jenkins:Jenkins123!@localhost:8080/manager/text/deploy?path=/myapp&update=true"
```

- `path=/myapp` → context path (URL)  
- `update=true` → redeploy if already exists

---

## Notes

- Ensure the Tomcat manager allows access from the Jenkins server IP.  
- For production, restrict access in `manager.xml` using a `RemoteAddrValve`.
