# Linux Essentials Cheatsheet – HomeLab & Pentesting Edition

## Navigation

```
ls -lath         # Dateien mit Rechten, Zeit & versteckten Dateien anzeigen
cd .. / cd -     # Eine Ebene zurück / zum letzten Verzeichnis
cd               # Zum Home-Verzeichnis
pwd              # Aktuellen Pfad anzeigen
clear            # Terminal leeren
```

---

## Dateien & Verzeichnisse

```
touch datei.txt                              # Neue/leere Datei erstellen
mkdir ordner                                 # Neues Verzeichnis erstellen
mkdir -p pfad/zum/ordner                     # Verschachtelte Ordner anlegen
cp quelle ziel                               # Datei kopieren
cp -r quelle ziel                            # Ordner inkl. Inhalt kopieren
mv quelle ziel                               # Datei verschieben oder umbenennen
rm datei                                     # Datei löschen
rm -rf ordner                                # Ordner rekursiv & forciert löschen
mkfs.ntfs                                    # Datenträgerformatieren
```
### Beispiele für die Anwendung Dateien und Verzeichnisse

usbstick defekt erkennen 
```
lsblk -p                                     # Laufwerke anzeigen, Flag -p zeigt agesamten pfad an /dev/...
fdisk -l                                     # listet alle partitionen auf
sudo mkfs.ntfs /dev/sdb1                     # Dateisystem auf Datenträger schreiben
---
wenn dies fehlschlägt
---
sudo fdisk /dev/sdb                         # eine neue partitionstabelle erstellen
---
wenn dies fehlschlägt kann ein hw deekt vorliegen
---
sudo dd if=/dev/urandom of=/dev/sdb bs=4M status=progress       #  partition mit zufallszahlen beschreiben
---
tritt hier gleich ein fehler auf kann der datenträger defekt sein
---
```
### Beschreibung der Zufallszahl

- /dev/urandom blockiert niemals: Auch wenn der Entropiepool vorübergehend leer ist, liefert es „Pseudozufallszahlen“ auf Basis kryptografisch sicherer Algorithmen weiter – für die allermeisten Einsatzzwecke wie auch das Füllen eines USB-Sticks mit Zufallsdaten ist die Qualität vollkommen ausreichend 

- /dev/random blockiert das Lesen, falls die Entropie im Pool erschöpft ist (z.B. bei starker Nutzung und wenig Zufallsquellen).  

- Beide beziehen ihre Zufallszahlen aus dem Kernel-Entropie-Pool, der durch schwer vorhersagbares Umgebungsrauschen (z.B. Tastaturanschläge, Festplattenzugriffe, Netzwerkverkehr) „gefüttert“ wird

Die Ausgabe von /dev/urandom ist für fast alle Zwecke, auch für kryptographische Anwendungen, als sicher anzusehen (Ausnahme: bestimmte sehr hohe Sicherheitsanforderungen, etwa einige Zufallsgenerierungs-Szenarien beim frühesten Systemstart)

---

## Datei-Inhalt anzeigen & bearbeiten

### Anzeige-Tools

```
cat datei                                    # Ganzen Inhalt anzeigen
cat -n                                       # Mit Zeilennummern
cat -T / -E                                  # Zeigt TABs / Zeilenenden

head -n 20 datei                             # Erste 20 Zeilen
tail -n 50 datei                             # Letzte 50 Zeilen
tail -f logdatei                             # Anhängen und „Live“ mitlesen
```

### Interaktive Anzeige

```
less datei                                   # Interaktiv durchblättern
  /suchwort                                  # Vorwärts suchen
  q                                          # Beenden

nano / vi datei                              # Datei bearbeiten (Editor)

watch -n 1 "ps aux | grep dd"                # mit watch kann ein befehl dauerhaft aktualisiert werden das Flag -n gibt die aktualisierungszeit an default 2s (ohne -n)
```

---

## Speicherplatz, Rechte & Besitzer

```
du -sh ordner/                               # Größe des Ordners
df -h                                        # Freier Speicher auf Laufwerken
chmod 755 datei                              # Rechte setzen (z. B. rwxr-xr-x)
adduser Username                             # einen neuen USer anlegen.  
passwd Username                              # Passwort anlegen/ändern eines Users  
groupadd gruppennname                        # eine Gruppe hinzufügen  
cat /etc/group                               # alle gruppen anzeigen  
usermod -aG gruppenname Username             # User einer Gruppe hinzufügen. Danach System neustarten.  
su Username                                  # Benutzer wechseln  
whoami                                       # Aktuellen Benuter anzeigen.
chown benutzer:gruppe datei                  # Eigentümer ändern
```

---

## Suchen & Filtern

```
grep 'muster' datei                          # Einfache Textsuche in Datei
grep -i -n -r 'fehler' /var/log              # Case-Insensitive, mit Zeilennr., rekursiv
find /pfad -name "*.log"                     # Dateien (nach Name) finden
```

### Kombiniert mit Pipes (z. B. bei Logs):

```
cat *.log | grep 'fail'                      # Alle Zeilen mit 'fail'
cat *.log | grep -i 'fail' | grep 'critical' # Beide Begriffe in Zeile
cat *.log | grep 'warn' | less               # Gut lesbar bei vielen Zeilen
```

---

## Netzwerk

```
ip a                                         # IP-Adressen & Interfaces anzeigen
ping 8.8.8.8                                 # Host erreichbar?
traceroute domain.de                         # Routenverfolgung
nslookup domain.tld                          # DNS-Infos anzeigen
ss -tulnp                                    # Offene Ports & Dienste

ssh benutzer@host                            # Per SSH verbinden
scp datei benutzer@host:/pfad/               # Datei übertragen
rsync -av quelle/ benutzer@host:/pfad/       # Ordner synchronisieren
```

---

## Prozesse & System

```
ps aux                                      # Liste aller Prozesse
ps aux | grep string (z.B. fdisk)                # gibt alle Prozess aus die z.B. fdisk enthalten
top / htop                                  # Systemauslastung (live)
kill PID                                    # Prozess gezielt beenden
uname -a                                    # Kernel & Systeminfo
uptime                                      # Systemlaufzeit & Last
whoami                                      # Angemeldeter Benutzer
```

---

## Tipps

- Nutze `tail -f` zur Log-Überwachung (z. B. Docker-Container)
- Kombiniere alles mit `|` (Pipes) für flexible Log-Filterung
- Mit `man befehl` bekommst du Hilfe zu jedem Kommando
- Erstelle eigene Aliase für häufige Befehle in `~/.zshrc`

---


