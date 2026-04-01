# LXC Networking

I wanted to try running docker in an lxc. First issue with lxc was networking

```bash
root@pxmx:~# cat  /etc/pve/lxc/107.conf
lxc.apparmor.profile: unconfined
lxc.cgroup2.devices.allow: a
lxc.cap.drop:
```


**Custom install.sh for LXC instead of Kali**

```bash
root@web01:~/websploit# cat install.sh
#!/usr/bin/env bash
# WebSploit installation script
# Author: Omar Ωr Santos
# Web: https://websploit.org
# X: @santosomar LinkedIn: https://linkedin.com/in/santosomar
# Version: 5.0


clear

print_banner() {
    # Define Colors and Styles using tput
    local bold reset blue green yellow cyan
    bold=$(tput bold)
    reset=$(tput sgr0) # Resets all attributes

    # Check if terminal supports colors (at least 8)
    if [[ $(tput colors) -ge 8 ]]; then
        blue=$(tput setaf 4)
        # green=$(tput setaf 2) # Uncomment if you want to use green later
        yellow=$(tput setaf 3) # Good for prompts
        cyan=$(tput setaf 6)   # Nice for titles or key info
    else
        # Set to empty if no color support; bold might still work
        blue=""
        green=""
        yellow=""
        cyan=""
        reset=""
    fi

    clear
    # Use cyan for the top/bottom separators and title
    echo "${bold}${blue}======================================================================${reset}"
    echo "${bold}${cyan}                WebSploit Labs Installer (v5.0)${reset}"
    echo "${bold}${blue}======================================================================${reset}"
    echo # Blank line for spacing

    # Use bold for labels, standard text for values, blue for URL
    echo " ${bold}Author:${reset}  Omar Ωr Santos"
    echo " ${bold}Web:${reset}     ${blue}https://websploit.org${reset}" # Make URL stand out
    echo " ${bold}Twitter:${reset} @santosomar"
    echo # Blank line

    # Description in standard text
    echo " This script will install the tools, configurations, and Docker"
    echo " containers required for the WebSploit Labs learning environment."
    echo # Blank line
    echo "----------------------------------------------------------------------"
    echo # Blank line

    # Use yellow for the prompt to draw attention
    read -n 1 -s -r -p "${yellow}Press any key to continue the setup...${reset}"
    echo # Add a newline after the keypress for cleaner subsequent output
}

print_banner

set -e  # Exit immediately if a command exits with a non-zero status
set -u  # Treat unset variables as an error

#--------------------------------------------------
# 1) Check if running as root
#--------------------------------------------------
if [[ $EUID -ne 0 ]]; then
  echo "You need admin privileges in order to install these packages. Please run this script as root (e.g. sudo ./install.sh). Additional details at https://websploit.org"
  exit 1
fi

#--------------------------------------------------
# 2) Basic apt update & install
#--------------------------------------------------
echo "[+] Updating apt and installing base packages..."
apt update -y
apt install -y wget curl git ack-grep gdb vim exuberant-ctags python3-pip python3-venv  hostapd ffuf tor jupyter-notebook 

#--------------------------------------------------
# 3) Install Python-based tools via apt (where possible)
#    This avoids PEP 668 issues for commonly packaged tools
#--------------------------------------------------
echo "[+] Installing Python-based tools via apt where available..."
apt install -y python3-flake8 python3-flask

#--------------------------------------------------
# 4) For Python packages not in apt, use a virtual environment
#    This prevents "externally-managed-environment" errors.
#--------------------------------------------------
echo "[+] Creating a virtual environment for pip-only packages..."
VENV_DIR="/opt/python-tools"
mkdir -p "$VENV_DIR"
python3 -m venv "$VENV_DIR"
# Activate venv
source "$VENV_DIR/bin/activate"

echo "[+] Installing PIP-only packages in the virtual environment..."
# Example packages not in apt:
pip install --upgrade pip
pip install certspy

# Deactivate the venv for now
deactivate

apt update -y

#--------------------------------------------------
# 5) Installing docker and pulling the docker-compose.yml
#--------------------------------------------------

#--------------------------------------------------
# 6) Clone various GitHub repos including mine, SecLists, GitTools, and PayloadsAllTheThings
#--------------------------------------------------
echo "[+] Cloning GitHub repos..."
cd /root

# Helper function to clone or update a repo
clone_or_update() {
    local repo_url="$1"
    local dir_name=$(basename "$repo_url" .git)
    
    if [ -d "$dir_name" ]; then
        echo "[*] Directory '$dir_name' already exists. Pulling latest changes..."
        cd "$dir_name" && git pull && cd ..
    else
        echo "[+] Cloning $repo_url..."
        git clone "$repo_url"
    fi
}

clone_or_update https://github.com/The-Art-of-Hacking/h4cker.git
clone_or_update https://github.com/danielmiessler/SecLists.git
clone_or_update https://github.com/internetwache/GitTools.git
clone_or_update https://github.com/swisskyrepo/PayloadsAllTheThings.git
clone_or_update https://github.com/The-Art-of-Hacking/websploit.git

# IoT firmware exercises for reverse engineering courses
mkdir -p /root/iot_exercises
cd /root/iot_exercises
wget https://github.com/OWASP/IoTGoat/releases/download/v1.0/IoTGoat-raspberry-pi2.img -O firmware1.img
wget https://github.com/santosomar/DVRF/releases/download/v3/DVRF_v03.bin -O firmware2.bin


#--------------------------------------------------
# 7) Fetching docker-compose.yml from WebSploit.org
#--------------------------------------------------
echo "[+] Using docker-compose.yml from websploit.git"

echo "[+] Starting containers..."
cd /root/websploit
docker-compose -f docker-compose.yml up -d


#--------------------------------------------------
# 8) Install radamsa from source
#--------------------------------------------------
echo "[+] Installing radamsa..."
cd /root
if [ -d "radamsa" ]; then
    echo "[*] Directory 'radamsa' already exists. Pulling latest changes..."
    cd radamsa && git pull
else
    echo "[+] Cloning radamsa..."
    git clone https://gitlab.com/akihe/radamsa.git
    cd radamsa
fi
make && make install

#--------------------------------------------------
# 9) Install Sublist3r (example of pip in venv or system-wide)
#     We'll do it in the same venv to avoid PEP 668 issues
#--------------------------------------------------
echo "[+] Installing Sublist3r in the same Python venv..."
cd /root
if [ -d "Sublist3r" ]; then
    echo "[*] Directory 'Sublist3r' already exists. Pulling latest changes..."
    cd Sublist3r && git pull
else
    echo "[+] Cloning Sublist3r..."
    git clone https://github.com/aboul3la/Sublist3r.git
    cd Sublist3r
fi

# Reactivate the venv
source "$VENV_DIR/bin/activate"
pip install -r requirements.txt
deactivate

#--------------------------------------------------
# 10) Install enum4linux-ng
#--------------------------------------------------
cd /root
clone_or_update https://github.com/cddmp/enum4linux-ng.git
# You can also pip-install it in the venv if you want

#--------------------------------------------------
# 11) If using Parrot, install searchsploit
#--------------------------------------------------
distribution=$(lsb_release -i | awk '{print $NF}')
if [[ "$distribution" == "Parrot" ]]; then
  echo "[+] Installing searchsploit for Parrot..."
  if [ -d "/opt/exploitdb" ]; then
    echo "[*] Directory '/opt/exploitdb' already exists. Pulling latest changes..."
    cd /opt/exploitdb && git pull
  else
    echo "[+] Cloning exploitdb..."
    git clone https://github.com/offensive-security/exploitdb.git /opt/exploitdb
  fi
  ln -sf /opt/exploitdb/searchsploit /usr/local/bin/searchsploit
fi



#--------------------------------------------------
# 12) Container info script
#--------------------------------------------------
echo "[+] Installing 'containers' script..."
curl -sSL https://raw.githubusercontent.com/The-Art-of-Hacking/websploit/refs/heads/master/containers.sh > /root/containers.sh
chmod +x /root/containers.sh
mv /root/containers.sh /usr/local/bin/containers

#--------------------------------------------------
# Done
#--------------------------------------------------
echo "
[✔] All done! All tools, apps, and containers have been installed and setup.
Have fun hacking! - Omar (Ωr) Santos
"
```

**Custom docker-compose for websploit**

```yaml
services:

# OWASP WebGoat Lab
  webgoat:
    container_name: webgoat
    image: santosomar/webgoat
    platform: linux/amd64
    restart: unless-stopped
    ports:
      - "8200:8080"
    networks:
      - websploit

# OWASP Juice Shop Lab
  juice-shop:
    container_name: juice-shop
    image: bkimminich/juice-shop
    restart: unless-stopped
    ports:
      - "8201:3000"
    networks:
      - websploit

# Damn Vulnerable Web Application Lab
  dvwa:
    container_name: dvwa
    image: santosomar/dvwa
    platform: linux/amd64
    restart: unless-stopped
    ports:
      - "8202:80"
    networks:
      - websploit

# Sci-Fi themed CTF challenge
  galactic-archives:
    container_name: galactic-archives
    image: santosomar/galactic-archives
    platform: linux/amd64
    restart: unless-stopped
    ports:
      - "8203:80"
    networks:
      - websploit

# Halo-themed CTF challenge
  gravemind:
    container_name: gravemind
    image: santosomar/gravemind
    platform: linux/amd64
    restart: unless-stopped
    ports:
      - "8204:80"
    networks:
      - websploit

# Star Wars themed CTF challenge
  y-wing:
    container_name: y-wing
    image: santosomar/ywing:latest
    platform: linux/amd64
    restart: unless-stopped
    ports:
      - "8205:80"
    networks:
      - websploit

# Multi-Vulnerability Gauntlet Lab
  hydra-nexus:
    container_name: hydra-nexus
    build: ./additional-labs/Multi-Vulnerability-Gauntlet
    restart: unless-stopped
    ports:
      - "8206:80"
    networks:
      - websploit

# XSS Lab
  phantom-script:
    container_name: phantom-script
    build: ./additional-labs/XSS
    restart: unless-stopped
    ports:
      - "8207:80"
    networks:
      - websploit

# SSRF Lab
  trojan-relay:
    container_name: trojan-relay
    build: ./additional-labs/SSRF
    restart: unless-stopped
    ports:
      - "8208:80"
    networks:
      - websploit

# SQL Injection Lab
  sqli-breach:
    container_name: sqli-breach
    build: ./additional-labs/SQLi
    restart: unless-stopped
    ports:
      - "8209:80"
    networks:
      - websploit

# Command Injection Lab
  shell-inject:
    container_name: shell-inject
    build: ./additional-labs/Command_Injection
    restart: unless-stopped
    ports:
      - "8210:80"
    networks:
      - websploit

# Path Traversal Lab
  maze-walker:
    container_name: maze-walker
    build: ./additional-labs/Path_Traversal
    restart: unless-stopped
    ports:
      - "8211:80"
    networks:
      - websploit

# XXE Lab
  entity-smuggler:
    container_name: entity-smuggler
    build: ./additional-labs/XXE
    restart: unless-stopped
    ports:
      - "8212:5014"
    networks:
      - websploit

# JWT Vulnerability Lab
  token-tower:
    container_name: token-tower
    build: ./additional-labs/token-tower
    restart: unless-stopped
    ports:
      - "8213:80"
    networks:
      - websploit

# Server-Side Template Injection Lab
  render-reign:
    container_name: render-reign
    build: ./additional-labs/render-reign
    restart: unless-stopped
    ports:
      - "8214:80"
    networks:
      - websploit

# Insecure Deserialization Lab
  deserial-gate:
    container_name: deserial-gate
    build: ./additional-labs/deserial-gate
    restart: unless-stopped
    ports:
      - "8215:80"
    networks:
      - websploit

# DEF CON 31 Challenge
  redis-rogue:
    container_name: redis-rogue
    image: santosomar/dc31_01:latest
    platform: linux/amd64
    restart: unless-stopped
    ports:
      - "8216:80"
    networks:
      - websploit

# GraphQL Lab
  graphql-galaxy:
    container_name: graphql-galaxy
    build: ./additional-labs/GraphQL
    restart: unless-stopped
    ports:
      - "8217:80"
    networks:
      - websploit

# Prototype Pollution Lab
  proto-pollute:
    container_name: proto-pollute
    build: ./additional-labs/Prototype_Pollution
    restart: unless-stopped
    ports:
      - "8218:80"
    networks:
      - websploit

networks:
  websploit:
    driver: bridge
```

See zeek-suricata-snort 