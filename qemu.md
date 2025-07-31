# QEMU Shell Befehle – Schnellreferenz

Willkommen zu einer Übersicht der wichtigsten QEMU-Befehle für dein HomeLab und Shell-Setup!  
Hier findest du kompakte, kommentierte Beispiele für die gängigsten Workflows – perfekt zum Nachschlagen oder als Einstieg.

---

## Kapitel 1: Image-Konvertierung mit qemu-img (Essentiell!)

**Zentraler Workflow für die Migration und Kompatibilität von Disk-Images zwischen Hypervisoren:**

```
qemu-img convert -p -f vpc -O vmdk quelle.vhd ziel.vmdk
```

- **Was macht der Befehl?**
  - Konvertiert eine VHD-Datei (z.B. Hyper-V, Microsoft; Quellformat) zu einer VMDK-Datei (z.B. VMware; Zielformat).
- **Parameter:**
  - `-p`: Zeigt eine Fortschrittsanzeige.
  - `-f vpc`: Definiert das Quellformat als „vpc“ (steht für VHD).
  - `-O vmdk`: Legt das Zielformat auf VMDK fest.
  - `quelle.vhd` und `ziel.vmdk`: Pfade zur Eingabe- und Ausgabedatei.
- **Wofür?**  
  - Unverzichtbar zum Austausch von VMs zwischen Plattformen.  
  - Vereinfacht Backups und Testing verschiedener Systeme.

---

## Kapitel 2: Weitere nützliche QEMU-Befehle

### Virtuelle Maschine starten (Systememulation)

```
qemu-system-x86_64 -m 2048 -cdrom pfad/zu/iso.iso -hda pfad/zu/disk.img -boot d
```
- Startet eine x86-VM mit 2GB RAM, ISO-Image als Bootquelle und einer virtuellen Festplatte.

---

### Neues Image erstellen

```
qemu-img create -f qcow2 disk.img 20G
```
- Erzeugt ein leeres QCOW2-Image mit 20GB Kapazität für den Einsatz als virtuelle Festplatte.

---

### Informationen zu einem bestehenden Image anzeigen

```
qemu-img info disk.img
```
- Zeigt Format, Größe, verwendete Features und ggf. Snapshots zum gegebenen Image.

---

### Snapshot anlegen

```
qemu-img snapshot -c snapshot_name disk.img
```
- Erstellt (je nach Image-Format) einen beschrifteten Snapshot deiner Disk.

---

### Liste der unterstützten Formate und Optionen

```
qemu-img --help
```
- Zeigt alle verfügbaren Formate und weiterführende Optionen für Image-Operationen an.

---

### Virtuelle Maschine mit Netzwerk, USB & mehr

```
qemu-system-x86_64 -m 4096 -net nic -net user -usb -hda disk.img
```
- Startet eine VM mit Netzwerkemulation, USB und 4GB RAM – ideal für Netzwerk- und Protokoll-Tests.

---

## Tipps für HomeLab & Automatisierung

- **ZSH & Shell-Scripting:** Integriere diese Befehle in Skripte oder kombiniere sie in Workflows mit zsh und oh-my-zsh für Effizienz!
- **Docker & CI/CD:**  
  Verwende Image-Befehle in Docker-Containern oder zur Automatisierung typischer Backup-, Test- und Migrationsroutinen.
- **Entdecke Netzwerkoptionen:** Viele QEMU-Befehle lassen sich durch zusätzliche Netzwerk-Flags oder Device-Passthrough erweitern – probiere z.B. '-net bridge' oder USB-Passthrough!
- **Ideal für Weiterbildung:** Nutze diese Szenarien als praktische Basis für das Erlernen einfacher Pentesting-Techniken, Netzwerkprotokolle sowie Shell-Expertise in deinem wachsenden HomeLab!

---

> Diese Markdown-Datei eignet sich als persönlicher Quick-Guide & Cheatsheet für deine zsh-Umgebung oder als Dokumentation im Repo!