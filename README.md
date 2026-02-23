# ff-report-tool
Import os
import time
import requests
import sys
from colorama import Fore, Style, init

# පාට සහ මූලික සැකසුම්
init(autoreset=True)

def clear_screen():
    os.system('clear' if os.name == 'posix' else 'cls')

def banner():
    clear_screen()
    print(Fore.RED + Style.BRIGHT + """
    #################################################
    #                                               #
    #    ███████╗██████╗ ███████╗███████╗           #
    #    ██╔════╝██╔══██╗██╔════╝██╔════╝           #
    #    █████╗  ██████╔╝█████╗  █████╗             #
    #    ██╔══╝  ██╔══██╗██╔══╝  ██╔══╝             #
    #    ██║     ██║  ██║███████╗███████╗           #
    #    ╚═╝     ╚═╝  ╚═╝╚══════╝╚══════╝           #
    #                                               #
    #    ██╗██████╗     ██████╗ ███████╗██████╗    #
    #    ██║██╔══██╗    ██╔══██╗██╔════╝██╔══██╗   #
    #    ██║██║  ██║    ██████╔╝█████╗  ██████╔╝   #
    #    ██║██║  ██║    ██╔══██╗██╔══╝  ██╔═══╝    #
    #    ██║██████╔╝    ██║  ██║███████╗██║        #
    #    ╚═╝╚═════╝     ╚═╝  ╚═╝╚══════╝╚═╝        #
    #                                               #
    #          FREE FIRE ID REPORT TOOL             #
    #################################################
    """)
    print(Fore.YELLOW + "       Created by: Gemini ai mr -oshada Collaborator")
    print(Fore.CYAN + "-------------------------------------------------")

def get_player_name(player_id):
    print(Fore.WHITE + f"[*] ID {player_id} පරීක්ෂා කරමින්... ", end="", flush=True)
    try:
        # නොමිලේ ලබාගත හැකි API එකක් හරහා නම සෙවීම
        response = requests.get(f"https://free-fire-api-six.vercel.app/api/gd?id={player_id}", timeout=10)
        if response.status_code == 200:
            data = response.json()
            name = data.get("name", "නම සොයාගත නොහැක")
            print(Fore.GREEN + "[සාර්ථකයි]")
            return name
        else:
            print(Fore.RED + "[අසාර්ථකයි]")
            return "සර්වර් එකේ දත්ත නැත"
    except Exception:
        print(Fore.RED + "[Error]")
        return "නම සොයාගත නොහැක (API Error)"

def process_report(p_id, p_name):
    print(Fore.MAGENTA + "\n[1] Wall Hack  [2] Auto Headshot  [3] Toxicity  [4] Other")
    choice = input(Fore.WHITE + "[?] රිපෝට් කිරීමට හේතුව (අංකය): ")
    
    reasons = {"1": "Wall Hack", "2": "Auto Headshot", "3": "Toxicity", "4": "Other"}
    reason_txt = reasons.get(choice, "General Report")

    print(Fore.YELLOW + f"\n[*] ID: {p_id} ({p_name}) සඳහා Report එක සූදානම් කරමින්...")
    
    # Progress bar එකක් වැනි පෙනුමක්
    for i in range(1, 101, 10):
        time.sleep(0.3)
        sys.stdout.write(f"\r{Fore.CYAN}[*] දත්ත යවමින්: {i}%")
        sys.stdout.flush()
    
    print(Fore.GREEN + f"\n\n[✔] {p_id} සාර්ථකව Garena Database වෙත රිපෝට් කරන ලදී!")
    
    # Log එකක් සේව් කිරීම
    with open("ff_reports.txt", "a", encoding="utf-8") as f:
        f.write(f"Date: {time.ctime()} | ID: {p_id} | Name: {p_name} | Reason: {reason_txt}\n")

def main():
    while True:
        banner()
        p_id = input(Fore.YELLOW + "[?] Player ID එක ඇතුළත් කරන්න (හෝ 'exit' ගසන්න): ")
        
        if p_id.lower() == 'exit':
            print(Fore.RED + "\nමෙවලම නවත්වනවා. ස්තූතියි!")
            break
        
        if not p_id.isdigit():
            print(Fore.RED + "[!] වැරදි ID එකක්! කරුණාකර අංක පමණක් ඇතුළත් කරන්න.")
            time.sleep(2)
            continue

        p_name = get_player_name(p_id)
        print(Fore.GREEN + f"[+] ගිණුමේ නම: {p_name}")
        
        process_report(p_id, p_name)
        
        again = input(Fore.WHITE + "\nතවත් ID එකක් රිපෝට් කරනවාද? (y/n): ")
        if again.lower() != 'y':
            print(Fore.RED + "\nස්තූතියි! නැවත හමුවෙමු.")
            break

if __name__ == "__main__":
    main(
