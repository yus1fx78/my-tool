#!/bin/bash

# Rənglər
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
CYAN='\033[0;36m'
NC='\033[0m'

# Logo (Xəta verməməsi üçün tək dırnaqlı 'EOF' istifadə edilir)
clear
printf "${RED}"
cat << 'EOF'
 __   __ _   _  ____  ___  _____ 
 \ \ / /| | | |/ ___||_ _||  ___|
  \ V / | | | |\___ \ | | | |_   
   | |  | |_| | ___) || | |  _|  
   |_|   \___/ |____/|___||_|    
                                 
      NMAP HELPER TOOL BY YUSIF
====================================================================
EOF
printf "${NC}\n"

echo -e "${YELLOW}Zəhmət olmasa bir Nmap əməliyyatı seçin:${NC}\n"

echo -e "${GREEN}1)${NC} Tez Skan (Quick Scan)            ${CYAN}-> Ən çox istifadə olunan 100 portu sürətlə yoxlayır (-F)${NC}"
echo -e "${GREEN}2)${NC} Aqressiv Skan (Aggressive Scan)  ${CYAN}-> ƏS, versiya, skriptlər və traceroute yoxlanılır (-A)${NC}"
echo -e "${GREEN}3)${NC} Bütün Portların Skanı (All Ports)${CYAN}-> 1-dən 65535-ə qədər olan bütün portları yoxlayır (-p-)${NC}"
echo -e "${GREEN}4)${NC} SYN Gizli Skan (Stealth Scan)    ${CYAN}-> TCP bağlantısını tamamlamadan daha gizli skan edir (-sS)${NC}"
echo -e "${GREEN}5)${NC} UDP Port Skanı (UDP Scan)        ${CYAN}-> Sistemdəki açıq UDP portlarını (məs. DNS) tapır (-sU)${NC}"
echo -e "${GREEN}6)${NC} Şəbəkə Kəşfi (Ping Sweep)        ${CYAN}-> Şəbəkədəki aktiv cihazları port yoxlamadan təyin edir (-sn)${NC}"
echo -e "${GREEN}7)${NC} Xidmət Versiyası (Version Detect)${CYAN}-> Açıq portlarda işləyən servislərin dəqiq versiyasını tapır (-sV)${NC}"
echo -e "${GREEN}8)${NC} Zəiflik Skanı (Vulnerability)    ${CYAN}-> Nmap-in NSE skriptləri ilə məlum zəiflikləri axtarır (--script vuln)${NC}"
echo -e "${GREEN}9)${NC} Ping Tələb Etməyən (No Ping)     ${CYAN}-> Hədəf ping-ə cavab vermədikdə (Firewall) skana məcbur edir (-Pn)${NC}"
echo -e "${GREEN}10)${NC} Firewall Keçmə (Fragment Packets)${CYAN}-> Paketləri bölərək bəzi Firewall/IDS sistemlərini aldadır (-f)${NC}"
echo -e "${RED}0)${NC} Çıxış (Exit)\n"

read -p "Seçiminizi daxil edin (0-10): " secim

if [[ "$secim" == "0" ]]; then
    echo -e "${RED}Proqramdan çıxılır...${NC}"
    exit 0
fi

if [[ "$secim" =~ ^([1-9]|10)$ ]]; then
    read -p "Hədəf IP və ya Domain ünvanını daxil edin: " target
    echo -e "\n${YELLOW}[*] Hədəf: $target üçün skan başladılır...${NC}\n"

    case $secim in
        1) sudo nmap -F "$target" ;;
        2) sudo nmap -A "$target" ;;
        3) sudo nmap -p- "$target" ;;
        4) sudo nmap -sS "$target" ;;
        5) sudo nmap -sU "$target" ;;
        6) sudo nmap -sn "$target" ;;
        7) sudo nmap -sV "$target" ;;
        8) sudo nmap --script vuln "$target" ;;
        9) sudo nmap -Pn "$target" ;;
        10) sudo nmap -f "$target" ;;
    esac
    
    echo -e "\n${GREEN}[+] Əməliyyat tamamlandı!${NC}"
else
    echo -e "${RED}Yanlış seçim etdiniz. Zəhmət olmasa yenidən cəhd edin.${NC}"
fi
