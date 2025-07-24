# Cheatsheet: Wireshark/tshark auf der Konsole (mit PCAP-Speicherung) – Für fortgeschrittene Einsteiger

Dieses Cheatsheet hilft dir, Netzwerkpakete auf der Konsole effizient mit **tshark** (dem CLI-Tool von Wireshark) aufzuzeichnen, zu filtern, zu speichern und zu analysieren. Alles mit ausführlichen Erklärungen, besonders für Linux-/Shell-geübte Nutzer.

---

## 🛠 Vorbereitung und Berechtigungen

**Wireshark/tshark sollten möglichst _nicht als root_ laufen.**
Füge deinen User der Gruppe „wireshark“ hinzu:

```
sudo dpkg-reconfigure wireshark-common
sudo adduser $USER wireshark
# Danach ab- und wieder anmelden!
```
Damit darfst du Pakete ohne Root-Rechte mitschneiden.

---

## 📡 Netzwerkschnittstellen finden und Live-Mitschnitt starten

- **Verfügbare Interfaces auflisten:**
  ```
  tshark -D
  ```
  Listet alle Netzwerkgeräte auf. Wähle das gewünschte Interface für den Mitschnitt.

- **Live-Mitschnitt auf z. B. eth0:**
  ```
  tshark -i eth0
  ```
  Startet das Mitschneiden von Paketen. Mit `[Strg]+[C]` beenden.

- **Sofortige Anzeige (optional -k):**
  ```
  tshark -i eth0 -k
  ```

---

## 💾 PCAP-Mitschnitte speichern

### Capture in PCAP/PCAPNG-Datei sichern

- **Basis-Mitschnitt als Datei:**
  ```
  tshark -i eth0 -w capture.pcap
  ```
  Sichert alle Pakete auf der angegebenen Schnittstelle in der Datei _capture.pcap_ (Standard: modernes pcapng-Format).

- **Klassisches pcap-Format (kompatibler):**
  ```
  tshark -i eth0 -w capture.pcap -F pcap
  ```

- **Anzahl Pakete begrenzen:**
  ```
  tshark -i eth0 -c 500 -w capture.pcap
  ```
  Stoppt nach 500 Paketen.

- **Dauer/Dateigröße begrenzen:**
  ```
  tshark -i eth0 -a duration:60 -w capture.pcap         # Nur 1 Minute
  tshark -i eth0 -a filesize:10000 -w capture.pcap      # Nur 10 MB
  ```

- **Ringpuffer für lange Mitschnitte (z. B. 5× 100 MB):**
  ```
  tshark -i eth0 -b filesize:100000 -b files:5 -w ringbuffer.pcap
  ```
  Automatisches Rotieren der Mitschnittdateien für Dauerbeobachtung.

---

## 🔄 Mitschnitte auslesen und analysieren

- **Aufgezeichneten Mitschnitt im Terminal anzeigen:**
  ```
  tshark -r capture.pcap
  ```

- **Nur bestimmte Pakete zeigen (Display-Filter):**
  ```
  tshark -r capture.pcap -Y "http"
  tshark -r capture.pcap -Y "ip.src==192.168.1.1"
  tshark -r capture.pcap -Y "tcp.port==443"
  ```

- **Nur bestimmte Felder extrahieren (für Scripting):**
  ```
  tshark -r capture.pcap -Y "http" -T fields -e frame.time -e ip.src -e http.host
  ```

---

## 📊 Live-Analyse und Filter

- **Nur HTTP-Requests anzeigen:**
  ```
  tshark -Y "http.request"
  ```

- **Nur DNS-Pakete live anzeigen:**
  ```
  tshark -i eth0 -Y "dns"
  ```

- **Nur Pakete zum Zielport 80 (HTTP):**
  ```
  tshark -Y "tcp.dstport==80"
  ```

---

## 🧰 Nützliche Spezialoptionen

- **Anzahl Pakete limitieren:**
  ```
  tshark -c 100 -i eth0
  ```

- **Statistiken für Mitschnitt (10-Sekunden-Raster):**
  ```
  tshark -z io,stat,10
  ```

- **Namensauflösung für Adressen aktivieren:**
  ```
  tshark -i eth0 -N mn
  # m: MAC-Adressen, n: Netzwerk-Hosts
  ```

- **Verfügbare Felder/protokolle anzeigen:**
  ```
  tshark -G fields
  ```

---

## 🗃️ Praxistipps zur Speicherung/Analyse

- **Mitschnitt im Hintergrund starten (z. B. für Forensik):**
  ```
  nohup tshark -i eth0 -w /pfad/capture.pcap &
  ```

- **Analyse großer Dateien filtern und ausgeben:**
  ```
  tshark -r capture.pcap -Y "http" | grep "Host:"
  ```

- **Mit klassischer Wireshark-GUI öffnen:**  
  Übertrage die `.pcap` oder `.pcapng` auf einen Rechner mit Oberfläche und öffne sie dort komfortabel mit `wireshark`.

---

## 🆘 Hilfe & Dokumentation

- **Übersicht aller Optionen:**
  ```
  tshark -h
  ```

- **Online-Referenz für alle Display-Filter:**
  https://www.wireshark.org/docs/dfref/

- **Manpage:**
  ```
  man tshark
  ```

---

## ⚡ Zusammenfassung Workflow (Schnellreferenz)

```
# 1. Interface finden
tshark -D

# 2. Mitschnitt live in Datei sichern (z. B. für 1 Minute)
tshark -i eth0 -a duration:60 -w test.pcap

# 3. Mitschnitt auswerten:
tshark -r test.pcap -Y "http" -T fields -e frame.time -e ip.src -e http.host

# 4. Ausschnitt für GUI exportieren
tshark -r test.pcap -Y "smb" -w smb_traffic.pcap
```

---

> **Tipp:** Kombiniere die Output-Power von tshark (z. B. Display-Filter, Feldauswahl) mit klassischen Shell-Tools (`grep`, `awk`, `less`) für deine eigene Analyse-Pipeline.

Entwickle eigene Scripte/Workflows – so bekommst du schnell und präzise genau die Netzwerkdaten, die du für HomeLab, Troubleshooting oder (Legal!) Pentesting brauchst.
```

