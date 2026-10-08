import socket
import getpass
import re

def port_scanner():
    target = input("Enter IP/hostname: ")
    # Validate the IP address or hostname

    try:
        ip = socket.gethostbyname(target)
        print(f"\nScanning {ip}...\n")

        ports = [21,22, 23, 25, 53, 80, 110, 443, 3306, 8080]

        for port in ports:
            s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            s.settimeout(0.5)

            if s.connect_ex((ip, port)) == 0:
                print(f"[+] Port {port} open")

                s.close()

    except socket.gaierror:
        print("[-] Invalid hostname/IP")

def dns_lookup():
    target = input("Enter domain name: ")
    try:
        ip = socket.gethostbyname(target)
        print(f"[+] {target} -> {ip}")
    except socket.gaierror:
        print("[-] DNS lookup failed")

    
def banner_grabber():
    target = input("Enter IP/hostname: ")
    port = int(input("Enter port: "))

    try:
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.settimeout(1)
        s.connect((target, port))
        s.send(b"\r\n")
        banner = s.recv(1024).decode(errors="ignore")

        print("\n[+] Banner:")
        print(banner if banner else "No banner returned")

        s.close()

    except Exception as e:
        print(f"[-] Connection failed: {e}")

def password_checker():
    password = getpass.getpass("Enter password to check: ")
    # Simple password strength check
    
    score = 0

    if len(password) >= 8:
        score += 1
    if re.search(r"[a-z]", password):
        score += 1
    if re.search(r"[0-9]", password):
        score += 1
    if re.search(r"[!@#$%^&*()-+]", password):
        score += 1

    if score <= 2:
        print("[-] Weak password")
    elif score <= 4:
        print("[!] medium password")
    else:
        print("[+] Strong password")



def main():
    while True:
        print("""
            ========== cybersecurity toolkit ==========
        
        1. Port Scanner
        2. DNS Lookup
        3. Banner Grabber
        4. Password Checker
        5. Exit
        """)

        choice = input("Select option: ")

        if choice == "1":
            port_scanner()
        elif choice == "2":
            dns_lookup()
        elif choice == "3":
            banner_grabber()
        elif choice == "4":
            password_checker()
        elif choice == "5":
            print("Exiting...")
            break
        else:
            print("[-] Invalid option ")

if __name__ == "__main__":
    main()
