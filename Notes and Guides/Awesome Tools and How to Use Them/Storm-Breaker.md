---
sticker: lucide//cherry
---

#### TO DO LIST
- [ ] Add images For first half of the process
- [ ] Add images of the exploit being used with proof of concept. 



# Storm-Breaker: Comprehensive Guide

This document provides a structured and comprehensive guide for setting up and utilizing Storm-Breaker. The Storm-Breaker tool, hosted on GitHub, is a sophisticated toolkit designed for ethical hacking purposes. Additionally, this guide covers the setup process for NGROK, a secure tunneling service, which complements the functionalities of Storm-Breaker.

## 🛠 Setting Up Storm-Breaker

Begin by downloading and configuring Storm-Breaker from the official GitHub repository:

🔗 [Storm-Breaker GitHub Repository](https://github.com/ultrasecurity/Storm-Breaker)

Execute the following commands in your terminal to clone and set up the repository:

```bash
git clone https://github.com/ultrasecurity/Storm-Breaker
cd Storm-Breaker
sudo bash install.sh
sudo python3 -m pip install -r requirements.txt
sudo python3 st.py
```

![GitHub commit activity](https://img.shields.io/github/commit-activity/m/ultrasecurity/Storm-Breaker)
![GitHub contributors](https://img.shields.io/github/contributors/ultrasecurity/Storm-Breaker)

## 🌐 NGROK Integration

### Account Creation and Setup

NGROK is a vital component that establishes secure tunnelling. Begin by creating an NGROK account. After account creation, navigate to the 'Getting Started > Setup & Installation' section on the NGROK dashboard.

### Installation

NGROK can be installed using the `curl` command. Execute the following in your terminal:

```bash
curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.asc | sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null && echo "deb https://ngrok-agent.s3.amazonaws.com buster main" | sudo tee /etc/apt/sources.list.d/ngrok.list && sudo apt update && sudo apt install ngrok
```

### Configuration

After installation, configure NGROK by setting up the authtoken:

```bash
ngrok config add-authtoken <TOKEN>
```

>`ris:ErrorWarning`  **Note:** Retrieve your unique TOKEN from the NGROK setup page.

### Terminal Management

Upon completing the setup, it's advisable to split your terminal window horizontally for better management. This can be done by right-clicking the terminal and selecting 'Split Terminal Horizontally' or by using the shortcut `Ctrl + Shift + D`.

## 🔄 Starting NGROK Tunnel

To initiate the NGROK tunnel, type the following command in the terminal:

```bash
ngrok http 2525
```
>`ris:ErrorWarning`  **Note:** Do not run as sudo

Upon execution, a URL similar to the following will be displayed:

```plaintext
https://a10b-31-205-127-158.ngrok-free.app
```

## 🌐 Accessing the Server

Navigate to the displayed URL to access the login page. Use the default credentials for access:

```plaintext
Username: admin
Password: admin
```

>  `ris:LightbulbFlash`**Important:** Ensure that the listener is active (indicated by the Red Box) before disseminating any links.

## 🖥️ Understanding the Interface

Upon successful login, four links will be displayed, each corresponding to different functionalities. For instance:

```plaintext
http://a10b-31-205-127-158.ngrok-free.app/templates/camera_temp/index.html
```

This link, labeled as 'camera temp', is designed to prompt the target for permissions to access the camera. If granted, it may record video, take photos, or retrieve the location, contingent on the specific link sent.

> `ris:Pushpin` **Ethical Consideration:** It is imperative to operate within legal boundaries and adhere to ethical guidelines when utilizing these tools. Their use should be confined to authorized testing or educational environments.

This guide aims to facilitate a structured and professional approach to using Storm-Breaker and NGROK for ethical hacking and security testing purposes. Ensure compliance with all applicable laws and ethical standards in your operations.