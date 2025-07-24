
```markdown
# Praxis-Aufgabenpaket: Nmap für HomeLab und Pentest-Grundlagen

Mit diesen Aufgaben lernst und übst du die wichtigsten Einsatzmöglichkeiten von **nmap** in der Shell. Die Übungen vermitteln alle zentralen Scan-Arten und liefern sinnvolle Praxisbezüge zu deinem HomeLab. Am Ende schreibst du selbst ein erstes Skript für automatisierte Netzanalysen.

---

## 1. Host Discovery im Heimnetz

- **Ziel:** Finde heraus, welche Geräte und Container in deinem lokalen Netz aktiv sind.
- **Aufgabe:**  
  Führe auf deinem Server aus:
  ```
  nmap -sn 192.168.1.0/24
  ```
  - Notiere mindestens drei gefundene Hosts.
  - Prüfe, ob alle erwarteten Docker-Container auftauchen.

---

## 2. Basis-Portscan auf einem Einzelhost

- **Ziel:** Prüfe, welche Standard-Dienste auf einem Zielserver laufen (z. B. dein Ubuntu-Server, Raspberry Pi, oder NAS).
- **Aufgabe:**  
  ```
  nmap 192.168.1.X
  ```
  - Notiere alle gefundenen offenen Ports und ihre Dienste.
  - Welche Dienste sollten offen sein, welche nicht?

---

## 3. Vollständiger TCP-Portscan durchführen

- **Ziel:** Finde auch nicht-standardmäßige Ports oder Dienste (z. B. versteckte Admin-Interfaces).
- **Aufgabe:**  
  ```
  nmap -p- 192.168.1.X
  ```
  - Gibt es ungewöhnliche Ports? Recherchiere, welcher Dienst dort läuft.

---

## 4. Stealth-Scan (SYN/halboffen) testen

- **Ziel:** Simuliere einen unauffälligen Pentest auf deinem Homelab.
- **Aufgabe:**  
  ```
  sudo nmap -sS 192.168.1.X
  ```
  - Vergleiche das Ergebnis mit einem normalen Scan.  
  - Was ist gleich, was ist anders?

---

## 5. UDP-Scan und alternative Protokolle

- **Ziel:** Prüfe, ob und welche UDP-Dienste laufen (z. B. DNS, SNMP).
- **Aufgabe:**  
  ```
  sudo nmap -sU 192.168.1.X
  ```
  - Finde einen aktiven UDP-Port. Was steckt dahinter?

---

## 6. Service- und Versionsscan

- **Ziel:** Erkenne nicht nur offene Ports, sondern auch, welche Software dort läuft.
- **Aufgabe:**  
  ```
  nmap -sV 192.168.1.X
  ```
  - Welche Versionen laufen? Gibt es veraltete Dienste?

---

## 7. Betriebssystem-Erkennung

- **Ziel:** Schätze ein, welches OS auf einem Ziel läuft (z. B. zur Inventarisierung im HomeLab).
- **Aufgabe:**  
  ```
  sudo nmap -O 192.168.1.X
  ```
  - Wird das Betriebssystem richtig erkannt?

---

## 8. Netzwerk-Scan auf ganzes Subnetz

- **Ziel:** Automatisiertes Mapping des Heimnetzes.
- **Aufgabe:**  
  ```
  nmap 192.168.1.0/24
  ```
  - Liste die gemeldeten Geräte samt gefundenen SSH/HTTP-Ports.

---

## 9. Schwachstellenscan mit Nmap-Skripten

- **Ziel:** Suche nach bekannten Schwachstellen (nur im eigenen Netz!).
- **Aufgabe:**  
  ```
  nmap --script vuln 192.168.1.X
  ```
  - Wurden mögliche Schwachstellen gefunden?

---

## 10. Scan-Ergebnisse speichern und dokumentieren

- **Ziel:** Sichere und vergleiche Ergebnisse bei späteren Netzwerkänderungen.
- **Aufgabe:**  
  ```
  nmap -oN mein_scan.txt 192.168.1.X
  ```
  - Öffne die Datei, überprüfe die Ausgabe.

---

## 11. Bonus: Dein eigenes Automatisierungs-Skript

- **Ziel:** Erstelle ein Bash- oder zsh-Skript, das mehrere Hosts scannt und die wichtigsten Daten extrahiert.
- **Aufgabe:**  
  - Lege eine Textdatei `targets.txt` mit 2–5 IPs oder Hostnamen an.
  - Schreibe das Skript `autoscan.sh`:
    1. Für jeden Host in `targets.txt`:  
       - Führe `nmap -sV` aus.
       - Extrahiere offene Ports und Haupt-Services (z. B. mit `grep` und `awk`).
       - Speicher das Ergebnis mit Zeitstempel in einer Logdatei.
    2. Füge eine Übersicht hinzu: Welche Dienste laufen am häufigsten?

  **Tipp für den Start**  
  ```
  #!/bin/zsh
  for host in $(cat targets.txt); do
      echo "Scan von $host am $(date)" >> scanlog.txt
      nmap -sV $host | grep open >> scanlog.txt
      echo "" >> scanlog.txt
  done
  ```

---

> **Hinweis:** Passe die Netzwerkbereiche (192.168.1.X) auf deine HomeLab-Umgebung an!
> Durch diese Aufgaben trainierst du die wichtigsten Scans und entwickelst Routine im Umgang mit nmap – und legst den Grundstein für Automatisierung eigener Analyse-Workflows!
```

