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
    echo "1) Start DNSCrypt"
    echo "---------------------"
    echo "2) Set DNSCrypt Service"
    echo "3) Restart DNSCrypt Service"
    echo "4) Remove DNSCrypt Service"
    echo "---------------------"
    echo "5) Check DNSCrypt"
    echo "6) Check DNSCrypt Service Status"
    echo "--------------------"
    echo "7) Exit"
}


enable_dnscrypt() {
    echo "[+] Checking DNSCrypt configuration..."
    cd /etc/dnscrypt-proxy
    sudo dnscrypt-proxy -check

    echo "[+] Starting DNSCrypt proxy manually..."
    sudo dnscrypt-proxy 

    echo "[+] Checking if DNSCrypt proxy is running..."
    sudo dnscrypt-proxy -resolve example.com
}

# Enable DNSCrypt Service
set_systemd_service() {
    echo "[+] Installing DNSCrypt service..."
    cd /etc/dnscrypt-proxy
    sudo dnscrypt-proxy -service install

    echo "[+] Starting DNSCrypt service..."
    sudo dnscrypt-proxy -service start 
}

# Restart DNSCrypt Service
restart_systemd_service() {
    echo "[+] Restarting DNSCrypt service..."
    cd /etc/dnscrypt-proxy
    sudo dnscrypt-proxy -service restart
}

# Remove DNSCrypt Service
remove_systemd_service() {
    echo "[+] Stopping DNSCrypt service..."
    cd /etc/dnscrypt-proxy
    sudo dnscrypt-proxy -service stop

    echo "[+] Uninstalling DNSCrypt service..."
    sudo dnscrypt-proxy -service uninstall
}

# Check DNSCrypt Status
check_dnscrypt_status() {
    echo "[+] DNSCrypt Service Status:"
    sudo systemctl status dnscrypt-proxy 
}

# Check DNSCrypt
check_dnscrypt() {
    echo "[+] Checking DNSCrypt configuration..."
    cd /etc/dnscrypt-proxy
    sudo dnscrypt-proxy -check

    echo "[+] Listing available DNS servers..."
    sudo dnscrypt-proxy -list

    echo "[+] Resolving example.com using DNSCrypt..."
    sudo dnscrypt-proxy -resolve example.com
}

# Main Menu Loop
while true; do
    menu
    read -p "Enter your choice: " choice

    case $choice in
        1) enable_dnscrypt ;;
        2) set_systemd_service ;;
        3) restart_systemd_service ;;
        4) remove_systemd_service ;;
        5) check_dnscrypt ;;
        6) check_dnscrypt_status ;;
        7) echo "Exiting..."; exit 0 ;;
        *) echo "Invalid choice! Please select a valid option." ;;
    esac

    echo -e "\nPress any key to continue..."
    read -n 1 -s -r
done
