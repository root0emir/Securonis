#!/bin/bash

cat << "EOF"

              ##    
           ##*#=#   
          *=+=*     
         +==+       
       @%*+#        
        +:+@        
       %=::+@       
    %%-..::++*@@    
  %*...-:::.*###%@  
@%...-...-.::####%@@
@:.+...-.:-.*@%%%%%@
@.:..+...-.-:@%%%%%@
@-..:...-::-=@%%%%%@
@@::.:..::-:*%@@@%@ 
  @%.-=.-:=%@@@@%%  
    @@@%%@@@@%%%    


EOF

echo "[Seconionis Menu]"
echo "1) Start Tor Traffic Routing"
echo "2) Stop Tor Traffic Routing"
echo "3) Tor Service Status"
echo "4) Restart Tor Traffic Routing"
echo "5) Enable Autowipe"
echo "6) Enable Autostart"
echo "7) Get Remote IP"
echo "8) Change Tor Identity"
echo "9) Change MAC Addresses"
echo "10) Revert MAC Addresses"
echo "11) Info"
echo "12) Exit"

# Get user choice
read -p "Please enter your choice: " choice

case $choice in
    1)
        /usr/bin/seconionis start
        ;;
    2)
        /usr/bin/seconionis stop
        ;;
    3)
        /usr/bin/seconionis status
        ;;
    4)
        /usr/bin/seconionis restart
        ;;
    5)
        /usr/bin/seconionis autowipe
        ;;
    6)
        /usr/bin/seconionis autostart
        ;;
    7)
        /usr/bin/seconionis ip
        ;;
    8)
        /usr/bin/seconionis changeid
        ;;
    9)
        /usr/bin/seconionis changemac
        ;;
    10)
        /usr/bin/seconionis revertmac
        ;;
    11)
        /usr/bin/seconionis version
        ;;
    12)
        echo "Exiting..."
        exit 0
        ;;
    *)
        echo "Invalid choice!"
        ;;
esac
