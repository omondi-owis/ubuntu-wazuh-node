**Ubuntu Wazuh Node Setup**

Documentation and notes for setting up an Ubuntu server lab node and connecting it to a Wazuh SIEM manager.
Overview

    OS: Ubuntu 24.04 LTS (Kernel 6.8.0-146-generic)

    SIEM: Wazuh Agent v4.14.7

    Access: SSH with Ed25519 keys

  What I Did
  
**1. Initial Setup & System Updates****

    Connected from my Kali machine via SSH using an SSH key.

    Ran into a timeout issue with the regional apt archive mirror (ke.archive.ubuntu.com), so I updated /etc/apt/sources.list.d/ubuntu.sources to point to the main global mirrors.

     <img width="951" height="612" alt="image" src="https://github.com/user-attachments/assets/d2238179-fcc5-4e56-983d-422db33dbf1a" />

    Did a full system upgrade and installed the latest kernel update (6.8.0-146), then rebooted the server.

**2. Installing the Wazuh Agent**

Added the Wazuh repository and installed the agent so the server reports back to the lab manager (10.145.2.147):
Bash

curl -s https://packages.wazuh.com/key/GPG-KEY-Wazuh | sudo gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import
sudo chmod 644 /usr/share/keyrings/wazuh.gpg
echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | sudo tee /etc/apt/sources.list.d/wazuh.list
sudo apt update
sudo WAZUH_MANAGER="10.145.2.147" apt-get install -y wazuh-agent

**3. Service Check**

Enabled and started the service to make sure it runs on boot:
Bash

sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent

To check if everything's running smoothly:
Bash

sudo systemctl status wazuh-agent
