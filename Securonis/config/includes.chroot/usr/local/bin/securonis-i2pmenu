#!/bin/bash

# Renk kodları
green="\e[32m"
red="\e[31m"
reset="\e[0m"

# ASCII Art Logo
echo -e "${green}"
cat << "EOF"

%%%%   @%%%%%%%%@   %%%%%%%%%@      ++++++    -----    ++++++    ++++++ 
%%%%  %%%%%%%%%%%%  %%%%%%%%%%%%  +++++++++ --------- ++++++++++++++++++
%%%%          %%%%  %%%%    %%%%  +++++++++ :-------- ++++++++++++++++++
%%%%         %%%%%  %%%%    %%%%   +++++++   -------   +++++++  ++++++++
%%%%        %%%%@   %%%%%%%%%%%%                                        
%%%%     %%%%%%     %%%%%%%%%%      ------   +++++++   -------  +++++++ 
%%%%    %%%%%       %%%%          :-------- +++++++++ ---------+++++++++
%%%%  %%%%%%%%%%%%% %%%%          --------: +++++++++ ---------+++++++++
%%%%  %%%%%%%%%%%%% %%%%            ------   +++++++   ------    ++++++ 

EOF
echo -e "${reset}"

while true; do
    echo "==========================="
    echo "      I2P ROUTER MENU     "
    echo "==========================="
    echo "1) Start I2P Router"
    echo "2) Stop I2P Router"
    echo "3) Graceful Stop"
    echo "4) Restart I2P Router"
    echo "5) Check I2P Status"
    echo "6) Install I2P (Auto-Start)"
    echo "7) Remove I2P (Disable Auto-Start)"
    echo "8) View Thread Dump"
    echo "9) About"
    echo "0) Exit"
    echo "==========================="
    read -p "Choose an option: " choice

    case $choice in
        1) i2prouter start ;;
        2) i2prouter stop ;;
        3) i2prouter graceful ;;
        4) i2prouter restart ;;
        5) i2prouter status ;;
        6) i2prouter install ;;
        7) i2prouter remove ;;
        8) i2prouter dump ;;
        9)
            echo "I2P - Invisible Internet Project"
            echo "- Start: Launches I2P Router as a daemon."
            echo "- Stop: Terminates I2P process."
            echo "- Graceful Stop: Stops I2P, may take up to 11 minutes."
            echo "- Restart: Stops and starts I2P again."
            echo "- Status: Shows whether I2P is running."
            echo "- Install: Enables I2P auto-start on boot."
            echo "- Remove: Disables I2P auto-start."
            echo "- Dump: Shows Java thread dump."
            ;;
        0) echo "Exiting..."; exit 0 ;;
        *) echo -e "${red}Invalid option!${reset}" ;;
    esac

done
