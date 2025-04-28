# SAPUI5 Tutorial für Anfänger mit Visual Studio Code

## Einleitung

SAPUI5 ist ein Framework von SAP für die Entwicklung von responsiven Webanwendungen mit modernem UI-Design. Dieses Tutorial führt dich durch die ersten Schritte mit SAPUI5 unter Verwendung von Visual Studio Code (VS Code).

## Voraussetzungen

- [Visual Studio Code](https://code.visualstudio.com/) installiert
- [Node.js](https://nodejs.org/) (mindestens Version 14) installiert

## 1. Einrichtung der Entwicklungsumgebung

### Erstellen eines neuen Projekts

Wir verwenden das SAP Fiori Tools CLI, um ein neues Projekt zu erstellen:

1. Öffne ein Terminal in VS Code (über `Terminal > New Terminal`)
2. Installiere das SAP Fiori Tools CLI:

```bash
npm install -g yo generator-easy-ui5
```
```
npm install @sap/generator-fiori
```
3. Erstelle ein neues Projekt:

```bash
yo @sap/fiori
```

4. Folge dem Assistenten und wähle:
   - Wähle "Basic" als Template
   - Data Source: None
   - View Name: meinProjekt
   - Modul Name: meinProjekt
   - Application Title: meinProjekt
   - Application namespace: meinProjekt
   - Descripion: meine erste SAP App
   - Enable TypeScrip: Geht beides
   - Add deployment configuration: Nein
   - Add FLP configuration: Nein
   - Configure advanced options: Nein
     

## 2. Projektstruktur verstehen

Nach dem Erstellen des Projekts solltest du folgende Struktur sehen:

```
meineersteapp/
│
├── webapp/               # Hauptverzeichnis der App
│   ├── controller/      # Controller-Dateien
│   ├── i18n/            # Internationalisierungsdateien
│   ├── model/           # Dateimodelle
│   ├── view/            # Views (XML-Dateien)
│   ├── Component.js     # Haupt-Komponente der App
│   ├── index.html       # Startseite für lokale Entwicklung
│   └── manifest.json    # App-Konfiguration
│
├── package.json         # NPM Abhängigkeiten
└── ui5.yaml             # UI5 Tooling Konfiguration
```

## 3. Lokale Entwicklung starten

1. Navigiere im Terminal in dein Projektverzeichnis:

```bash
cd meineersteapp
```

2. Installiere die Abhängigkeiten:

```bash
npm install
```

3. Starte den Entwicklungsserver:

```bash
npm start
```

Dies startet einen lokalen Server und öffnet deine App im Browser.
