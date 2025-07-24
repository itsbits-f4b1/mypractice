# Praxis-Aufgabenpaket: Wireshark/tshark in der Konsole anwenden und automatisieren

Mit diesen Aufgaben kannst du dein Wissen und Handling von tshark (Wireshark CLI) praxisnah vertiefen. Die Aufgaben bauen logisch aufeinander auf – von Basics, über Filter/Analyse bis hin zum eigenen kleinen Script.

---

## 1. Interfaces finden und ersten Mitschnitt machen

- **Ziel:** Lerne, welche Netzwerkschnittstellen auf deinem System verfügbar sind und starte einen ersten, kurzen Mitschnitt.
  - Führe `tshark -D` aus und notiere mindestens zwei Interface-Namen.
  - Starte einen 10-Sekunden-Mitschnitt auf deinem Hauptinterface und speichere ihn als test1.pcap.

---

## 2. Kontrolliere und analysiere gespeicherte Captures

- **Ziel:** Prüfe, dass dein Mitschnitt erfolgreich war und sieh dir die ersten Pakete im Terminal an.
  - Lies test1.pcap mit `tshark -r test1.pcap`.
  - Identifiziere mindestens ein verwendetes Protokoll (z. B. ARP, DNS, TCP).

---

## 3. Mitschnitt gezielt auf Protokolle filtern

- **Ziel:** Übe Filter während der Aufnahme und Auswertung.
  - Starte einen Mitschnitt, der ausschließlich DNS-Pakete enthält (`-Y "dns"`) und stoppe nach 50 Paketen.
  - Speichere den Mitschnitt als dns_only.pcap.

---

## 4. Arbeiten mit Feldextraktion

- **Ziel:** Extrahiere gezielt Informationen für Deine Analyse.
  - Extrahiere nur Quell- und Ziel-IP aus test1.pcap:  
    `tshark -r test1.pcap -T fields -e ip.src -e ip.dst`
  - Ergänze: Lass dir diese Ausgabe nachträglich in eine Textdatei schreiben.

---

## 5. Live-Mitschnitt mit Namenauflösung

- **Ziel:** Verständliche Anzeige durch Hostnamen.
  - Starte einen neuen Mitschnitt und zeige direkt lesbare Hostnamen und Ports:  
    `tshark -i <interface> -N mn`

---

## 6. Automatische Rotierung (Ringpuffer)

- **Ziel:** Lerne längere Mitschnitte bequem und platzsparend zu speichern.
  - Führe einen Mitschnitt aus, der einen Ringpuffer nutzt (`-b`) – z. B. 3 Dateien à 5MB.

---

## 7. Statistikfunktionen erkunden

- **Ziel:** Finde statistische Auffälligkeiten im Traffic.
  - Nimm mit tshark 120 Sekunden Netzwerkverkehr auf und wertete am Ende aus:  
    `tshark -r <deine_datei> -z io,stat,30`  
    (z. B. 30s-Intervalle)

---

## 8. Export für die Wireshark-GUI

- **Ziel:** Bereite einen gefilterten Auszug für die grafische Wireshark-Auswertung vor.
  - Erstelle aus deinem Hauptmitschnitt eine kleine pcap, die nur HTTP-Pakete enthält.
    - Tipp: `tshark -r test1.pcap -Y "http" -w http_traffic.pcap`
  - Öffne dies (wenn möglich) auf einem anderen Rechner mit Wireshark.

---

## 9. Automatisierte Analyse mit Pipe und grep

- **Ziel:** Finde auffällige Traffic-Muster schnell in der Shell.
  - Suche aus test1.pcap alle Zeilen, in denen die lokale IP-Adresse vorkommt, und zähle sie:  
    `tshark -r test1.pcap -T fields -e ip.src -e ip.dst | grep "192.168..." | wc -l`

---

## 10. Dein erstes einfaches Analysescript

- **Ziel:** Automatisiere wiederkehrende Analysen.
  - Schreibe ein kleines Bash-Skript (z. B. analyse.sh), das:
    - Als Parameter einen .pcap-Dateinamen erwartet.
    - Alle HTTP-Requests in dieser Datei aufsummiert.
    - Die häufigsten Ziel-Ports (Top 3) ausgibt.
  - Nutze für die Umsetzung: tshark, grep, sort, uniq, head.

  **Tipp für den Start:**
  ```
  #!/bin/zsh
  pcapfile=$1
  echo "HTTP-Requests:"
  tshark -r $pcapfile -Y "http.request" | wc -l
  echo "Top 3 Ziel-Ports:"
  tshark -r $pcapfile -T fields -e tcp.dstport | sort | uniq -c | sort -nr | head -3
  ```
```

