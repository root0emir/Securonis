#!/bin/bash

# Securonis Linux - DNSCrypt Manager

set -e

# ASCII Art Function
ascii_art() {
    cat << "EOF"
 ________                                     _____           
 __  ___/______________  ________________________(_)_______   
 _____ \_  _ \  ___/  / / /_  ___/  __ \_  __ \_  /__  ___/   
 ____/ //  __/ /__ / /_/ /_  /   / /_/ /  / / /  / _(__  )    
 /____/ \___/\___/ \__,_/ /_/    \____//_/ /_//_/  /____/     
                                                              
 _____________   _________________                      _____ 
 ___  __ \__  | / /_  ___/_  ____/___________  ___________  /_
 __  / / /_   |/ /_____ \_  /    __  ___/_  / / /__  __ \  __/
 _  /_/ /_  /|  / ____/ // /___  _  /   _  /_/ /__  /_/ / /_  
 /_____/ /_/ |_/  /____/ \____/  /_/    _\__, / _  .___/\__/  
                                        /____/  /_/               
EOF
}

# Menu Function
menu() {
    ascii_art
    echo -e "\e[32m[Securonis Linux - DNSCrypt Manager]\e[0m"
    echo "1) Enable DNS Encryption"
    echo "2) Disable DNS Encryption"
    echo "3) Check DNS Status"
    echo "4) Exit"
}

# Enable DNSCrypt
enable_dnscrypt() {
    echo "[+] Installing DNSCrypt..."
    sudo apt update
    sudo apt install -y dnscrypt-proxy

    echo "[+] Configuring DNSCrypt service..."
    cd /etc/dnscrypt-proxy
    sudo dnscrypt-proxy -service install
    sudo dnscrypt-proxy -service start

    echo "[+] Updating system DNS settings..."
    sudo sed -i 's/#DNS=/DNS=127.0.0.1/' /etc/systemd/resolved.conf
    sudo systemctl restart systemd-resolved

    echo "[✔] DNSCrypt has been successfully enabled!"
}

# Disable DNSCrypt
disable_dnscrypt() {
    echo "[!] Disabling DNS Encryption..."
    sudo dnscrypt-proxy -service stop
    sudo dnscrypt-proxy -service uninstall

    echo "[+] Restoring default DNS settings..."
    sudo sed -i 's/DNS=127.0.0.1/#DNS=/' /etc/systemd/resolved.conf
    sudo systemctl restart systemd-resolved

    echo "[✔] DNS Encryption has been disabled!"
}

# Check DNS Status
check_status() {
    echo "[+] DNSCrypt Service Status:"
    sudo systemctl status dnscrypt-proxy | grep "Active:"

    echo "[+] DNS Resolution Test:"
    cd /etc/dnscrypt-proxy
    dnscrypt-proxy -resolve example.com
}

# Main Menu Loop
while true; do
    menu
    read -p "Enter your choice: " choice

    case $choice in
        1) enable_dnscrypt ;;
        2) disable_dnscrypt ;;
        3) check_status ;;
        4) echo "Exiting..."; exit 0 ;;
        *) echo "Invalid choice! Please select a valid option.";;
    esac

    echo -e "\nPress any key to continue..."
    read -n 1 -s -r
done
