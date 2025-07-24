
```markdown
# Nmap-Cheatsheet mit Praxisbeispielen für Geübte Anfänger

Nmap ist das Standard-Tool für Netzwerkerkundung und Pentesting. Die folgenden Scans sind wichtig für Analyse und Sicherheit. Jeder Scan ist mit einem typischen Praxis-Einsatz versehen.

---

## 1. Host Discovery (Ping-Scan)

**Befehl:**  
```
nmap -sn 192.168.1.0/24
```
**Nutzen:**  
Du findest heraus, welche Geräte in deinem Heimnetz überhaupt erreichbar/aktiv sind – ideal für Bestandsaufnahme und unentdeckte Fremdgeräte aufspüren[1][5][20].

---

## 2. Basis-Portscan (alle „wichtigen“ Ports)

**Befehl:**  
```
nmap 192.168.1.10
```
**Nutzen:**  
Du prüfst, welche Dienste auf einem bestimmten Server laufen (z. B. hast du einen neuen Docker-Container deployed und willst wissen, ob seine Ports offen sind)[5].

---

## 3. Vollständiger TCP-Portscan

**Befehl:**  
```
nmap -p- 192.168.1.10
```
**Nutzen:**  
Du siehst **alle** offenen Ports (1–65535), nicht nur die Standardports – oft hilfreich, um ungewöhnliche Dienste zu entdecken (z. B. Webserver auf Port 8081)[1].

---

## 4. Stealth-Scan (TCP SYN) – „unerkannt abtasten“

**Befehl:**  
```
sudo nmap -sS 192.168.1.10
```
**Nutzen:**  
Du prüfst Ports halb-offen, damit weniger im Ziel-Server-Log auffällt – eignet sich für Security-Überprüfungen oder Penetrationstest im eigenen Netz[1][18].

---

## 5. UDP-Scan

**Befehl:**  
```
sudo nmap -sU 192.168.1.10
```
**Nutzen:**  
UDP-Dienste (z. B. DNS, SNMP) sind „leise“ und werden oft vergessen – dieser Scan zeigt, ob sie offen sind. Gerade für IoT-/HomeLab-Geräte extrem wichtig[3][18].

---

## 6. Version-Detection (Dienste identifizieren)

**Befehl:**  
```
nmap -sV 192.168.1.10
```
**Nutzen:**  
Du erfährst, welche Software und Version auf offenen Ports läuft. Praktisch, um z. B. veraltete Webserver zu erkennen – oder nach Updates gezielt zu suchen[5][6].

---

## 7. Betriebssystem-Erkennung

**Befehl:**  
```
sudo nmap -O 192.168.1.10
```
**Nutzen:**  
Ermittelt automatisch das Betriebssystem eines Netzgeräts. Nützlich, wenn du dokumentieren willst, welche Geräte in deinem HomeLab mit welchem OS laufen (Linux/Windows etc.)[6].

---

## 8. Subnetz-Scan (ganze Netzwerksegmente prüfen)

**Befehl:**  
```
nmap 192.168.1.0/24
```
**Nutzen:**  
Scanne ein ganzes LAN (z. B. nach neuen IoT-Geräten suchen oder vor Gästen das eigene Netzwerk checken)[5][1].

---

## 9. Mit Skripten nach Schwachstellen suchen

**Befehl:**  
```
nmap --script vuln 192.168.1.10
```
**Nutzen:**  
Automatisierte Prüfung auf bekannte Schwachstellen (CVEs) – wichtig vor Netzfreigaben oder für Angriffsflächenanalyse im Pentesting-Lab[6][7].

---

## 10. Ergebnis speichern und später auswerten

**Befehl:**  
```
nmap -oN scan.txt 192.168.1.10
```
**Nutzen:**  
Speichert den vollständigen Scan als Textdatei, z. B. für spätere Vergleichsscans nach Änderungen oder als Doku für HomeLab-Audits[1].

---

## 11. Schneller Service-/Portscan mit Hostnamen

**Befehl:**  
```
nmap -F myserver.local
```
**Nutzen:**  
Schneller Überblick über offene Dienste nach Neuinstallation oder wichtigen Updates via Hostname (z. B. ob SSH und HTTP wieder erreichbar sind)[5][1].

---

## 12. Netzwerk mit ausgeschalteter Ping-Prüfung scannen

**Befehl:**  
```
nmap -Pn 192.168.1.10
```
**Nutzen:**  
Hilfreich, wenn deine Ziele auf Pings (ICMP) nicht reagieren (Firewall): Der Scan läuft trotzdem und prüft alle Ports[1][5].

---

## 13. Scans auf mehrere Hosts aus einer Datei

**Befehl:**  
```
nmap -iL targets.txt
```
**Nutzen:**  
Ideal für größere Netzwerke oder wiederholte Audits: Füge alle Hosts/IPs in eine Datei ein, der Scan prüft automatisiert alle Ziele ab[1].

---

## Übersichtstabelle (Befehl, Zweck, Praxisbeispiel)

| Befehl                           | Zweck                              | Praxisbeispiel                                      |
| --------------------------------- | ---------------------------------- | --------------------------------------------------- |
| nmap -sn 192.168.1.0/24          | Host Discovery                     | Alle Geräte im LAN finden                           |
| nmap 192.168.1.10                | Basis-Portscan                     | Dienste auf Docker-Host prüfen                      |
| nmap -p- 192.168.1.10            | Alle Ports scannen                 | Ungewöhnliche Services aufspüren                    |
| sudo nmap -sS 192.168.1.10       | Stealth-Portscan (halb-offen)      | Pentesting ohne Spuren im Log zu hinterlassen       |
| sudo nmap -sU 192.168.1.10       | UDP-Ports scannen                  | DNS-/SNMP-Dienste von NAS prüfen                    |
| nmap -sV 192.168.1.10            | Software & Versionen anzeigen      | Alte Web- oder SSH-Server erkennen                  |
| sudo nmap -O 192.168.1.10        | Betriebssystem erkennen            | Linux oder Windows identifizieren                   |
| nmap 192.168.1.0/24              | Subnetz scannen                    | IoT-Geräte finden, Netzüberblick gewinnen           |
| nmap --script vuln 192.168.1.10  | Schwachstellen prüfen              | Security-Check vor Freigabe eines Servers           |
| nmap -oN scan.txt 192.168.1.10   | Ergebnisse speichern               | Doku und Vergleich nach Umbauten                    |
| nmap -F myserver.local           | Schnellscan gängiger Ports         | Nach Updates rasch aufs Wesentliche prüfen          |
| nmap -Pn 192.168.1.10            | Scan ohne ICMP-/Ping-Test          | Firewalled Geräte, die auf Ping „unsichtbar“ sind   |
| nmap -iL targets.txt             | Scan über Datei                    | Wiederholte Scans in größerem Heimnetz              |

---

**Tipp**: Nmap ist mächtig – kombiniere erste Experimente mit einzelnen IPs/Containern im Homelab, bevor du größere Netzbereiche scannst. Nutze stets das eigene Netz oder Testumgebungen!

> Wer sein HomeLab und Netzwerk versteht, erhöht nicht nur die Sicherheit, sondern wird auch erfolgreicher im Pentesting und der Systemadministration.
```

Quellen:
[1] Nmap Cheat Sheet 2025 - Codelivly https://codelivly.com/nmap-cheat-sheet/
[2] Examples | Nmap Network Scanning https://nmap.org/book/man-examples.html
[3] Switches and Scan Types in Nmap - centron GmbH https://www.centron.de/en/tutorial/switches-and-scan-types-in-nmap/
[4] Nmap Cheat Sheet 2025: All the Commands & Flags - StationX https://www.stationx.net/nmap-cheat-sheet/
[5] 24 Nmap Commands for Network Security and Scanning https://www.tecmint.com/nmap-command-examples/
[6] Types of Nmap scans and best practices - TechTarget https://www.techtarget.com/searchnetworking/tip/Types-of-Nmap-scans-and-best-practices
[7] Top 16 Nmap Commands: Nmap Port Scan Cheat Sheet https://www.recordedfuture.com/threat-intelligence-101/tools-and-techniques/nmap-commands
[8] Usage and Examples | Nmap Network Scanning https://nmap.org/book/nse-usage.html
[9] Port Scanning Techniques - Nmap https://nmap.org/book/man-port-scanning-techniques.html
[10] The Best Nmap Cheat Sheet | Zero To Mastery https://zerotomastery.io/cheatsheets/nmap-cheat-sheet/
[11] Six practical use cases for Nmap https://www.redhat.com/en/blog/use-cases-nmap
[12] Switches and Scan Types in Nmap - DigitalOcean https://www.digitalocean.com/community/tutorials/nmap-switches-scan-types
[13] nmap Cheat Sheet: Commands you should know - cyberphinix https://cyberphinix.de/blog/nmap-cheat-sheet/
[14] How to Use Nmap: Complete Guide with Examples https://www.ninjaone.com/blog/how-to-use-nmap-complete-guide-with-examples/
[15] Options Summary | Nmap Network Scanning https://nmap.org/book/man-briefoptions.html
[16] Nmap Cheat Sheet - GeeksforGeeks https://www.geeksforgeeks.org/ethical-hacking/nmap-cheat-sheet/
[17] Usage and Examples | Nmap Network Scanning https://nmap.org/book/vscan-examples.html
[18] Tools - Nmap - SecurityGuill https://securityguill.com/nmap.html
[19] Nmap cheatsheet for penetration testing - GitHub https://github.com/ekol-x9/nmap-cheatsheet
[20] How to Use Nmap: Commands and Tutorial Guide https://www.varonis.com/blog/nmap-commands
