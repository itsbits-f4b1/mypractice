
```markdown
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
```

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
```

---

## Speicherplatz, Rechte & Besitzer

```
du -sh ordner/                               # Größe des Ordners
df -h                                        # Freier Speicher auf Laufwerken
chmod 755 datei                              # Rechte setzen (z. B. rwxr-xr-x)
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


