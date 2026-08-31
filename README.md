#!/bin/bash

# Rənglər
GREEN='\033[0;32m'
CYAN='\033[0;36m'
NC='\033[0m'

show_menu() {
    clear
    cat << "EOF"
 __     __  _    _    _____   _____   ______ 
 \ \   / / | |  | |  / ____| |_   _| |  ____|
  \ \_/ /  | |  | | | (___     | |   | |__   
   \   /   | |  | |  \___ \    | |   |  __|  
    | |    | |__| |  ____) |  _| |_  | |     
    |_|     \____/  |_____/  |_____| |_|     
                                             
EOF
    echo -e "${CYAN}==============================================================================================${NC}"
    echo -e "${GREEN} 1. Tez Skan (Quick Scan)${NC}           -> Ən çox istifadə olunan 100 portu sürətlə yoxlayır (-F)"
    echo -e "${GREEN} 2. Aqressiv Skan (Aggressive Scan)${NC} -> ƏS, versiya, skriptlər və traceroute yoxlanılır (-A)"
    echo -e "${GREEN} 3. Bütün Portların Skanı${NC}           -> 1-dən 65535-ə qədər olan bütün portları yoxlayır (-p-)"
    echo -e "${GREEN} 4. SYN Gizli Skan (Stealth Scan)${NC}   -> TCP bağlantısını tamamlamadan daha gizli skan edir (-sS)"
    echo -e "${GREEN} 5. UDP Port Skanı (UDP Scan)${NC}       -> Sistemdəki açıq UDP portlarını (məs. DNS) tapır (-sU)"
    echo -e "${GREEN} 6. Şəbəkə Kəşfi (Ping Sweep)${NC}       -> Şəbəkədəki aktiv cihazları port yoxlamadan təyin edir (-sn)"
    echo -e "${GREEN} 7. Xidmət Versiyası (Version)${NC}      -> Açıq portlarda işləyən servislərin dəqiq versiyasını tapır (-sV)"
    echo -e "${GREEN} 8. Zəiflik Skanı (Vulnerability)${NC}   -> Nmap-in NSE skriptləri ilə məlum zəiflikləri axtarır"
    echo -e "${GREEN} 9. Ping Tələb Etməyən (No Ping)${NC}    -> Hədəf ping-ə cavab vermədikdə (Firewall) skana məcbur edir (-Pn)"
    echo -e "${GREEN}10. Firewall Keçmə${NC}                  -> Paketləri bölərək bəzi Firewall/IDS sistemlərini aldadır (-f)"
    echo -e "${GREEN}11. Əməliyyat Sistemi (OS Detect)${NC}   -> Hədəf cihazın hansı əməliyyat sistemini (OS) işlətdiyini tapır (-O)"
    echo -e "${GREEN}12. TCP Connect Skan (TCP Connect)${NC}  -> SYN skanı alınmadıqda tam TCP bağlantısı qurur (-sT)"
    echo -e "${GREEN}13. Standart Skriptlər (Default)${NC}    -> Nmap-in ilkin (default) təhlükəsizlik skriptlərini icra edir (-sC)"
    echo -e "${GREEN}14. Saxta IP Skanı (Decoy Scan)${NC}     -> Öz IP-nizi gizlətmək üçün saxta IP-lərlə birgə skan edir (-D RND:10)"
    echo -e "${GREEN}15. Çox Yavaş Skan (Paranoid/T0)${NC}    -> İDS/Firewall-a düşməmək üçün paketləri çox yavaş göndərir (-T0)"
    echo -e "${CYAN}==============================================================================================${NC}"
    echo -e " ${GREEN}0. Çıxış${NC}"
    echo -e "${CYAN}==============================================================================================${NC}"
}

while true; do
    show_menu
    read -p "Seçiminizi edin (0-15): " secim

    if [[ "$secim" == "0" ]]; then
        echo "Proqramdan çıxılır..."
        exit 0
    fi

    # Seçimin 1 ilə 15 arasında olmasını yoxlayır
    if [[ "$secim" =~ ^([1-9]|1[0-5])$ ]]; then
        read -p "Hədəf IP ünvanını daxil edin: " target_ip
        echo -e "\nSkan Başlayır: $target_ip ...\n"
        
        case $secim in
            1) nmap -F "$target_ip" ;;
            2) sudo nmap -A "$target_ip" ;;
            3) nmap -p- "$target_ip" ;;
            4) sudo nmap -sS "$target_ip" ;;
            5) sudo nmap -sU "$target_ip" ;;
            6) nmap -sn "$target_ip" ;;
            7) nmap -sV "$target_ip" ;;
            8) nmap --script vuln "$target_ip" ;;
            9) nmap -Pn "$target_ip" ;;
            10) sudo nmap -f "$target_ip" ;;
            11) sudo nmap -O "$target_ip" ;;
            12) nmap -sT "$target_ip" ;;
            13) nmap -sC "$target_ip" ;;
            14) nmap -D RND:10 "$target_ip" ;;
            15) nmap -T0 "$target_ip" ;;
        esac
        
        echo -e "\nSkan Bitdi!"
        read -p "Davam etmək üçün Enter düyməsinə basın..."
    else
        echo "Yanlış seçim! Zəhmət olmasa 0-15 arası bir rəqəm seçin."
        sleep 2
    fi
done
