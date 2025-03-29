#!/bin/bash

# Tor durumunu kontrol etme (checktorproject kullanarak)
check_tor_status() {
    tor_check=$(curl -s https://check.torproject.org/api/ip)
    if echo "$tor_check" | grep -q '"IsTor":true'; then
        echo "Tor is running."
    else
        echo "Tor is not running."
    fi
}

# DNS sızıntı testi
dns_leak_test() {
    echo "Checking DNS Leak..."
    dig +short @resolver1.opendns.com myip.opendns.com
}

# IP sızıntı testi
ip_leak_test() {
    echo "Checking IP Leak..."
    curl -s https://ipleak.net/json/
}

# VPN durumunu kontrol etme
check_vpn_status() {
    if ip a | grep -q "tun0"; then
        echo "VPN is running."
    else
        echo "VPN is not running."
    fi
}

# DNSCrypt kontrolü
check_dnscrypt() {
    if pgrep dnscrypt >/dev/null 2>&1; then
        echo "DNSCrypt is running."
        return 10
    else
        echo "DNSCrypt is not running."
        return 0
    fi
}


check_privacy_score() {
    score=0

    # Live Mode
    if [ "$1" == "live" ]; then
        score=$((score + 20))
    fi

    # VPN
    check_vpn_status
    score=$((score + $?))

    # Tor
    check_tor_status
    score=$((score + $?))

    # DNSCrypt
    check_dnscrypt
    score=$((score + $?))

    echo "Your privacy score is: $score"
}

# Sistem durumu bilgilerini gösterme
system_status() {
    echo "Uptime: $(uptime -p)"
    echo "Load Average: $(cat /proc/loadavg)"
    echo "RAM Usage: $(free -h | grep Mem | awk '{print $3 "/" $2}')"
    echo "CPU Usage: $(top -bn1 | grep 'Cpu(s)' | sed 's/.*, *\([0-9.]*\)%* id.*/\1/' | awk '{print 100 - $1}')%"
    echo "-------------------------------------"
    echo "Disk Usage:"
    df -h
}

# Sistem bilgilerini gösterme
system_info() {
    echo "OS: $(lsb_release -ds)"
    echo "Kernel: $(uname -r)"
    echo "Architecture: $(uname -m)"
    echo "CPU Info: $(lscpu | grep 'Model name')"
    echo "Memory Info: $(free -h)"
    echo "Disk Info: $(lsblk -o NAME,FSTYPE,SIZE,MOUNTPOINT,UUID)"
}

# Ağ durumunu gösterme
network_status() {
    ip a | grep inet
}

# IP adresini kontrol etme
check_ip() {
    curl ifconfig.me
}

# DNS sunucusunu değiştirme
change_dns() {
    echo "Changing DNS to Cloudflare (1.1.1.1)..."
    echo "nameserver 1.1.1.1" | sudo tee /etc/resolv.conf
}

# Ağ izlerini silme (BleachBit kullanarak)
delete_network_traces() {
    echo "Deleting network traces with BleachBit..."
    sudo apt-get install -y bleachbit
    bleachbit -c system.cache system.recent_documents system.tmp system.trash
    echo "Network traces deleted."
}

# Gizlilik durumunu kontrol etme
check_privacy_status() {
    echo "Checking privacy settings..."
    ps aux | grep tor
    sudo ufw status
    sudo apparmor_status
}

# Güvenlik taraması yapma
security_scan() {
    echo "Running detailed security scan..."
    sudo apt install -y clamav
    sudo freshclam
    sudo clamscan -r --bell -i --exclude-dir="^/sys" --exclude-dir="^/proc" --exclude-dir="^/dev" /
}

# İzleri temizleme
cleaning_traces() {
    echo "Cleaning traces..."
    history -c && history -w
    rm -rf ~/.cache ~/.bash_history ~/.zsh_history
    sudo rm -rf /tmp/* /var/tmp/* /var/log/*
    echo "Traces cleaned."
}

# RAM'i silme
wipe_ram() {
    echo "Wiping RAM..."
    sync; echo 3 > /proc/sys/vm/drop_caches
    sudo sh -c "echo 1 > /proc/sys/vm/drop_caches"
}

# Diski güvenli bir şekilde silme
wipe_disk() {
    read -p "Enter the disk to wipe (e.g., /dev/sdX): " disk
    sudo shred -v -n 5 -z "$disk"
}

# Hostname'i değiştirme
change_hostname() {
    read -p "Enter new hostname: " new_host
    sudo hostnamectl set-hostname "$new_host"
}

# Fiziksel güvenliği etkinleştirme
physical_security() {
    echo "Activating physical security measures..."

    # USB portlarını engelle
    echo "Disabling USB ports..."
    sudo sh -c "echo 'blacklist usb-storage' > /etc/modprobe.d/blacklist-usb.conf"

    # Tüm aktif oturumları kilitle
    echo "Locking all active sessions..."
    sudo pkill -KILL -u $(whoami)

    # Ekran ve klavye girişlerini engelle
    echo "Disabling screen/keyboard input..."
    sudo setterm -blank force

    # Eğer TPM varsa, TPM güvenliği etkinleştir
    echo "Enabling TPM security (if available)..."
    # TPM ile ilgili güvenlik komutları burada eklenebilir

    # Kernel güvenlik önlemleri
    echo "Setting kernel security parameters..."
    sudo sysctl -w kernel.randomize_va_space=2
    sudo sysctl -w kernel.kptr_restrict=2
    sudo sysctl -w kernel.dmesg_restrict=1
    sudo sysctl -w fs.protected_hardlinks=1
    sudo sysctl -w fs.protected_symlinks=1

    # Donanım güvenliği
    echo "Setting hardware security parameters..."
    sudo modprobe -r usb_storage
    echo "blacklist usb_storage" | sudo tee /etc/modprobe.d/blacklist-usb-storage.conf

    # TMP güvenliği
    echo "Setting tmp security parameters..."
    sudo mount -o remount,nodev,noexec,nosuid /tmp

    echo "Physical security measures are now in place."
}

# Fiziksel güvenliği devre dışı bırakma
disable_physical_security() {
    echo "Disabling physical security measures..."

    # USB portlarını etkinleştir
    echo "Enabling USB ports..."
    sudo rm /etc/modprobe.d/blacklist-usb.conf
    sudo modprobe usb_storage

    # Kernel güvenlik önlemlerini eski haline getirme
    echo "Reverting kernel security parameters..."
    sudo sysctl -w kernel.randomize_va_space=1
    sudo sysctl -w kernel.kptr_restrict=0
    sudo sysctl -w kernel.dmesg_restrict=0
    sudo sysctl -w fs.protected_hardlinks=0
    sudo sysctl -w fs.protected_symlinks=0

    # Donanım güvenliğini eski haline getirme
    echo "Reverting hardware security parameters..."
    sudo rm /etc/modprobe.d/blacklist-usb-storage.conf

    # TMP güvenliğini eski haline getirme
    echo "Reverting tmp security parameters..."
    sudo mount -o remount /tmp

    echo "Physical security measures have been disabled."
}

# Çekirdek güvenlik durumunu kontrol etme
check_kernel_security_status() {
    echo "Checking kernel security status..."
    
#!/bin/bash

# Function to check if Paranoia Mode is enabled
is_paranoia_mode_enabled() {
    if [ -f /etc/sysctl.d/99-paranoia-mode.conf ] && iptables -L INPUT | grep -q "DROP"; then
        return 0
    else
        return 1
    fi
}

# Check if /etc/sysctl.d/99-securonis-hardening.conf exists
if [ -f /etc/sysctl.d/99-securonis-hardening.conf ]; then
    echo "[+]Kernel Settings are done. Your system is very secure against Network and System attacks."
else
    echo "[!]Kernel settings are not applied. Your system may be vulnerable to Network and System attacks."
    echo "To make your system and network more secure, select System Hardening Settings from the desktop or run the following command in the terminal:"
    echo "systemhardening"
fi

# Check if Paranoia Mode is enabled
if is_paranoia_mode_enabled; then
    echo "You are in Paranoia Mode. Your system is at the highest security level."
fi
}

# Paranoia modunu etkinleştirme
activate_paranoia_mode() {
    echo "Activating paranoia mode..."
    sudo /usr/local/bin/paranoia
}

# Paranoia modunu devre dışı bırakma
deactivate_paranoia_mode() {
    echo "Deactivating paranoia mode..."
    sudo /usr/local/bin/paranoiadeactivate
}

# Sistemi yok etme
nuke_the_system() {
    echo "Nuking the system..."
    sudo /usr/local/bin/nuke2system
}

# Disk şifrelemesi kontrolü
check_disk_encryption() {
    echo "Checking disk encryption..."
    sudo lsblk -o NAME,FSTYPE,MOUNTPOINT,UUID
}

# Güvenlik güncellemelerini kontrol etme
check_security_updates() {
    echo "Checking for security updates..."
    sudo apt update && sudo apt list --upgradable | grep -i security
}

# Tüm IP bilgilerini kontrol etme
check_all_ip_info() {
    echo "Checking all IP information..."
    curl ifconfig.me/all
}

# MAC adresini spoof etme
spoof_mac() {
    read -p "Enter network interface (e.g., eth0, wlan0): " iface
    sudo ifconfig "$iface" down
    sudo macchanger -r "$iface"
    sudo ifconfig "$iface" up
    echo "MAC address spoofed."
}

# Boş alanı silme
wipe_free_space() {
    echo "Wiping free space..."
    sudo dd if=/dev/zero of=/zerofillfile bs=1M
    sudo rm -f /zerofillfile
    echo "Free space wiped."
}

# SSL/TLS fingerprint doğrulama
ssl_tls_fingerprint_verification() {
    read -p "Enter domain to verify: " domain
    echo | openssl s_client -connect "$domain":443 2>/dev/null | openssl x509 -fingerprint -noout
}

# Firewall kurallarını sıkılaştırma
harden_firewall() {
    echo "Hardening firewall rules..."
    sudo ufw default deny incoming
    sudo ufw default deny outgoing
    sudo ufw enable
    echo "Firewall hardened."
}

# Ağ trafiğini izleme
monitor_network_traffic() {
    echo "Monitoring network traffic..."
    sudo iftop -i $(ip route | grep '^default' | awk '{print $5}')
}

# Güvenli dosya silme
secure_delete() {
    read -p "Enter file or directory to securely delete: " target
    sudo srm -rv "$target"
}

# Tarayıcı izlerini silme
delete_browser_traces() {
    echo "Deleting browser traces..."
    rm -rf ~/.mozilla/firefox/*.default-release/places.sqlite ~/.config/google-chrome/Default/History
    echo "Browser traces deleted."
}

# IP konumu kontrol etme
check_ip_location() {
    echo "Checking IP location..."
    curl -s ipinfo.io
}

# MITM saldırı kontrolü ve savunma
mitm_attack_check_defense() {
    echo "Running MITM attack check and defense..."
    sudo /usr/local/bin/antimitm
}

# ASCII giriş menüsü
while true; do
    clear
    echo "====================================="
    echo "         System and Network          "
    echo "====================================="
    echo "Hostname: $(hostname)"
    echo "IP Address: $(hostname -I | awk '{print $1}')"
    echo "====================================="
    echo "           System Status             "
    echo "====================================="
    system_status
    echo "====================================="
    echo "           System Info               "
    echo "====================================="
    system_info
    echo "====================================="
    echo "           Network Info              "
    echo "====================================="
    network_status
    echo "====================================="
    echo "              Menu                   "
    echo "====================================="
    echo "1) Check IP"
    echo "2) Check IP Location"
    echo "3) Check Privacy Score"
    echo "4) Check Tor Status"
    echo "5) Check VPN Status"
    echo "6) Check All IP Information"
    echo "7) DNS Leak Test"
    echo "8) IP Leak Test"
    echo "-------------------"
    echo "9) MITM Attack Check & Defense"
    echo "10) Change DNS"
    echo "11) Spoof MAC Address"
    echo "12) Delete Network Traces"
    echo "13) Monitor Network Traffic"
    echo "-------------------"
    echo "14) Security Scan (ClamAV)"
    echo "15) Cleaning System Traces"
    echo "16) Wipe RAM"
    echo "17) Wipe Disk"
    echo "18) Wipe Free Space"
    echo "19) Change Hostname"
    echo "20) Check Kernel Security Status"
    echo "21) Check Disk Encryption"
    echo "22) Check Security Updates"
    echo "23) Set Hardened Firewall Rules"
    echo "24) Secure File Delete"
    echo "-------------------"
    echo "25) SSL/TLS Fingerprint Verification"
    echo "26) Clean Browser Traces"
    echo "-------------------"
    echo "27) Activate Paranoia Mode"
    echo "28) Deactivate Paranoia Mode"
    echo "--------------------"
    echo "29) Activate Physical Security"
    echo "30) Disable Physical Security"
    echo "--------------------"
    echo "31) Nuke The System"
    echo "--------------------"
    echo "0) Exit"
    echo "====================================="
    read -p "Choose an option: " choice
    
    case $choice in
        1) check_ip ;;
        2) check_ip_location ;;
        3) check_privacy_score ;;
        4) check_tor_status ;;
        5) check_vpn_status ;;
        6) check_all_ip_info ;;
        7) dns_leak_test ;;
        8) ip_leak_test ;;
        9) mitm_attack_check_defense ;;
        10) change_dns ;;
        11) spoof_mac ;;
        12) delete_network_traces ;;
        13) monitor_network_traffic ;;
        14) security_scan ;;
        15) cleaning_traces ;;
        16) wipe_ram ;;
        17) wipe_disk ;;
        18) wipe_free_space ;;
        19) change_hostname ;;
        20) check_kernel_security_status ;;
        21) check_disk_encryption ;;
        22) check_security_updates ;;
        23) harden_firewall ;;
        24) secure_delete ;;
        25) ssl_tls_fingerprint_verification ;;
        26) delete_browser_traces ;;
        27) activate_paranoia_mode ;;
        28) deactivate_paranoia_mode ;;
        29) physical_security ;;
        30) disable_physical_security ;;
        31) nuke_the_system ;;
        0) exit 0 ;;
        *) echo "Invalid option. Try again." ;;
    esac
    read -p "Press Enter to continue..." dummy
done 
