# 🧠 React + Electron Cheat Sheet für Linux Mint Desktop
> Aufbau für effiziente lokale Entwicklung mit **React**, **Visual Studio Code**, **Electron** – ideal für dein HomeLab oder portable Desktop-Apps auf Linux und Windows-Systemen.

## 📦 Voraussetzungen & Grundinstallation

### 🧰 Node.js & npm auf Linux Mint installieren

```zsh
# Node installieren (empfohlen: LTS)
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs

# Version prüfen
node -v
npm -v
```

- `Node.js`: JavaScript-Laufzeitumgebung für Backend-/Tooling-Tasks
- `npm`: Node Package Manager – installiert JS-Abhängigkeiten

## 🧑💻 Visual Studio Code + Extensions

### 💻 Installation auf Linux Mint

```zsh
sudo apt install wget gpg
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo install -o root -g root -m 644 microsoft.gpg /etc/apt/trusted.gpg.d/
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'
sudo apt update
sudo apt install code
```

### 🌟 Nützliche Extensions für React/Electron

| Extension                       | Zweck                                         |
|--------------------------------|-----------------------------------------------|
| `dsznajder.es7-react-js-snippets` | React/Redux Code-Snippets                   |
| `esbenp.prettier-vscode`         | Code-Formatter (einheitlicher Stil)         |
| `dbaeumer.vscode-eslint`         | Linting und Fehlererkennung                 |
| `formulahendry.auto-close-tag`  | Tags automatisch schließen                   |
| `vscode-icons-team.vscode-icons` | Bessere Übersicht durch Icons                |
| `GitLens`                        | Git-Historie, Blame, Änderungen im Editor    |
| `electron-snippets`             | Electron-spezifische Codeblöcke              |

## ⚛️ React erklärt – Grundlagen & Architektur

> React ist eine JavaScript-Bibliothek für den Aufbau interaktiver Benutzeroberflächen.

### 🧩 Was ist React?

- Erstellt von Facebook, heute Open Source
- Fokus: **Komponentenbasiertes UI-Design**
- Verwaltet **State** und **DOM**-Änderungen effizient

### 🚀 React-Kernthemen im Überblick

| Konzept        | Erklärung |
|----------------|-----------|
| **JSX**        | XML-ähnliche Syntax in JavaScript (`<div>Hello</div>`) |
| **Komponenten** | Wiederverwendbare UI-Bausteine (funktional oder Klassen-Komponenten) |
| **State**      | Interner Zustand einer Komponente (z.B. Zählerstand) |
| **Props**      | Eingabewerte von außen, vergleichbar mit Parametern |
| **Virtual DOM** | Abstraktionsschicht, um DOM-Änderungen effizient darzustellen |
| **Hooks**      | Funktionen, die React-Funktionen wie State oder Lifecycle in funktionale Komponenten bringen (`useState`, `useEffect`, etc.) |

### 🔍 Beispiel: Einfacher Counter mit Hooks

```tsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Du hast {count} Mal geklickt</p>
      <button onClick={() => setCount(count + 1)}>Klick mich</button>
    </div>
  );
}

export default Counter;
```

## 🛠️ Neues React-Projekt erstellen

```zsh
npx create-react-app mein-projekt --template typescript
cd mein-projekt
code .
npm start
```

**Erklärung:**
- `npx`: Führt Create-React-App aus ohne globale Installation  
- `--template typescript`: Optional, für TS-Vorlage
- `npm start`: Startet Dev-Server unter `http://localhost:3000`

## ⚙️ Integration von Electron

### 🔌 Electron installieren & vorbereiten

```zsh
npm install --save-dev electron
```

### 📁 Projektstruktur anpassen

#### 🔧 `public/electron.js` erstellen:

```js
const { app, BrowserWindow } = require('electron');

function createWindow() {
  const win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: { nodeIntegration: true }
  });

  // Für Dev:
  win.loadURL('http://localhost:3000');

  // Für Production:
  // win.loadFile('build/index.html');
}

app.whenReady().then(createWindow);
```

#### 📦 `package.json` anpassen:

```json
{
  "main": "public/electron.js",
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "electron": "electron ."
  }
}
```

## 🧪 App lokal testen

```zsh
# React starten
npm start

# Electron lokal starten
npm run electron
```

- Dies öffnet ein Desktop-Fenster mit deiner laufenden React-Anwendung.

## 📤 App als Desktop-Anwendung ausliefern

### 🔨 Electron-Builder installieren

```zsh
npm install --save-dev electron-builder
```

### 🛠️ electron.js für Build anpassen

```js
win.loadFile('build/index.html'); // Für Build statt Dev-URL
```

### 📝 package.json konfigurieren

```json
"build": {
  "appId": "dein.reactapp",
  "productName": "MeineReactApp",
  "linux": {
    "target": "AppImage"
  },
  "win": {
    "target": "nsis"
  }
}
```

### 🚀 Build erzeugen

```zsh
npm run build       # React-Bundle erzeugen
npm run dist        # Electron-App-Build (verwendet electron-builder)
```

### Ergebnis:

- Du findest dann im `dist/`-Ordner:
  - Linux: `.AppImage`, `.deb`, …
  - Windows: `.exe`-Installer

## ⚡ Bonus: zsh + Aliases + HomeLab Bezug

Du nutzt `zsh` mit `oh-my-zsh`? Hier ein praktischer Alias:

```zsh
alias react-start="npm start & sleep 2 && npm run electron"
```

Wenn du das Projekt im HomeLab per Docker betreiben willst, kann die öffentlich gebaute React-App auch in einem nginx-Image ausgeliefert werden – siehe [Dockerfile in früherem Schritt](#).

## ✅ Zusammenfassung

```zsh
# React + Electron Setup (Linux Mint Edition)
sudo apt install nodejs npm git
npm install -g create-react-app

# Projekt starten
npx create-react-app mein-projekt
cd mein-projekt
npm start

# Electron hinzufügen
npm install --save-dev electron electron-builder
npm run electron

# Production-Desktop-App bauen
npm run build
npm run dist
```

## 📚 Weiterführende Lernressourcen (für dein Ziel)

- **React Grundlagen**: Suche auf YouTube nach „React Grundlagen deutsch“ oder „React Hooks einfach“
- **Electron Tutorials**: z.B. „Build Desktop App with Electron and React“ (engl., viele gute Walkthroughs)
- **TypeScript & React**: TS ist in React durch Interfaces & Enums sehr wirkungsvoll – du wirst es lieben!
- **Netzwerk im HomeLab**: React-Frontend + Express-Backend + Docker = Mini-Fullstack im HomeLab
