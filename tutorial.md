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

```bash
npm install -g @sap/generator-fiori
```

3. Erstelle ein neues Projekt:

```bash
yo @sap/fiori
```

4. Folge dem Assistenten und wähle:
   - Wähle "Basic" als Template
   - Data Source: None
   - View Name: MainView
   - Modul Name: my-first-app
   - Application Title: Meine erste SAPUI5 App
   - Application namespace: sap.ui5.tutorial
   - Description: Grundlagenprojekt für UI5
   - Enable TypeScript: Geht beides
   - Add deployment configuration: Nein
   - Add FLP configuration: Nein
   - Configure advanced options: Nein

## 2. Projektstruktur verstehen

Nach dem Erstellen des Projekts solltest du folgende Struktur sehen:

```
my-first-app/
│
├── webapp/              # Hauptverzeichnis der App
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
cd my-first-app
```

2. Installiere die Abhängigkeiten:

```bash
npm install
```

3. Navigiere jetzt in deine Webapp
```bash
cd webapp
```

4. Starte den Entwicklungsserver:

```bash
npm start
```

Dies startet einen lokalen Server und öffnet deine App im Browser.

### **Wichtig!**

Überprüfe ob du die richtige Adresse offen hast.
```
http://localhost:8080/index.html
```

## 4. Praktische Übungen mit SAPUI5

### Übung 1: Einfache Liste mit Mockdaten anzeigen

In dieser Übung zeigen wir eine Liste von Produkten auf unserer Webseite an.

1. Erstelle eine Mockdaten-Datei unter `webapp/model/products.json`:

```json
{
  "Products": [
    {
      "ID": "1",
      "Name": "Laptop",
      "Price": 999.99,
      "Currency": "EUR",
      "Category": "Electronics"
    },
    {
      "ID": "2",
      "Name": "Smartphone",
      "Price": 699.99,
      "Currency": "EUR",
      "Category": "Electronics"
    },
    {
      "ID": "3",
      "Name": "Kaffeemaschine",
      "Price": 129.99,
      "Currency": "EUR",
      "Category": "Household"
    },
    {
      "ID": "4",
      "Name": "Tastatur",
      "Price": 49.99,
      "Currency": "EUR",
      "Category": "Computer"
    }
  ]
}
```

2. Öffne die View-Datei `webapp/view/MainView.view.xml` und ersetze den Inhalt mit:

```xml
<mvc:View
   controllerName="sap.ui5.tutorial.controller.MainView"
   xmlns="sap.m"
   xmlns:mvc="sap.ui.core.mvc">
   <Page title="Produktübersicht">
      <List
         id="productList"
         items="{/Products}">
         <StandardListItem
            title="{Name}"
            description="{Category}"
            info="{Price} {Currency}"
            infoState="Success"/>
      </List>
   </Page>
</mvc:View>
```

3. Bearbeite den Controller `webapp/controller/MainView.controller.js`:

```javascript
sap.ui.define([
   "sap/ui/core/mvc/Controller",
   "sap/ui/model/json/JSONModel"
], function (Controller, JSONModel) {
   "use strict";

   return Controller.extend("sap.ui5.tutorial.controller.MainView", {
      onInit: function () {
         // Lade die Mockdaten
         var oModel = new JSONModel("model/products.json");
         this.getView().setModel(oModel);
      }
   });
});
```

4. Starte deine App mit `npm start` und prüfe, ob die Produktliste angezeigt wird.

### Übung 2: Hinzufügen eines Suchfelds

Wir erweitern unsere App um eine Suchfunktion:

1. Bearbeite die View-Datei `webapp/view/MainView.view.xml`:

```xml
<mvc:View
   controllerName="sap.ui5.tutorial.controller.MainView"
   xmlns="sap.m"
   xmlns:mvc="sap.ui.core.mvc">
   <Page title="Produktübersicht">
      <Toolbar>
         <SearchField id="searchField" width="100%" search=".onSearch" />
      </Toolbar>
      <List
         id="productList"
         items="{/Products}">
         <StandardListItem
            title="{Name}"
            description="{Category}"
            info="{Price} {Currency}"
            infoState="Success"/>
      </List>
   </Page>
</mvc:View>
```

2. Erweitere den Controller `webapp/controller/MainView.controller.js`:

```javascript
sap.ui.define([
   "sap/ui/core/mvc/Controller",
   "sap/ui/model/json/JSONModel",
   "sap/ui/model/Filter",
   "sap/ui/model/FilterOperator"
], function (Controller, JSONModel, Filter, FilterOperator) {
   "use strict";

   return Controller.extend("sap.ui5.tutorial.controller.MainView", {
      onInit: function () {
         // Lade die Mockdaten
         var oModel = new JSONModel("model/products.json");
         this.getView().setModel(oModel);
      },
      
      onSearch: function (oEvent) {
         // Erstelle Filter basierend auf dem Suchbegriff
         var sQuery = oEvent.getParameter("query");
         var aFilters = [];
         
         if (sQuery) {
            aFilters.push(new Filter("Name", FilterOperator.Contains, sQuery));
         }
         
         // Hole die Liste und wende den Filter an
         var oList = this.byId("productList");
         var oBinding = oList.getBinding("items");
         oBinding.filter(aFilters);
      }
   });
});
```

### Übung 3: Detailansicht eines Produkts anzeigen

In dieser Übung fügen wir eine Detailansicht hinzu, wenn ein Produkt ausgewählt wird:

1. Erstelle einen Dialog in der View-Datei `webapp/view/MainView.view.xml`:

```xml
<mvc:View
   controllerName="sap.ui5.tutorial.controller.MainView"
   xmlns="sap.m"
   xmlns:mvc="sap.ui.core.mvc">
   <Page title="Produktübersicht">
      <Toolbar>
         <SearchField id="searchField" width="100%" search=".onSearch" />
      </Toolbar>
      <List
         id="productList"
         items="{/Products}"
         itemPress=".onItemPress">
         <StandardListItem
            title="{Name}"
            description="{Category}"
            info="{Price} {Currency}"
            infoState="Success"
            type="Active"/>
      </List>
   </Page>
   
   <Dialog id="productDetailDialog" title="Produktdetails" contentWidth="50%">
      <VBox class="sapUiSmallMargin">
         <Label text="ID" />
         <Text id="productID" />
         <Label text="Name" />
         <Text id="productName" />
         <Label text="Kategorie" />
         <Text id="productCategory" />
         <Label text="Preis" />
         <Text id="productPrice" />
      </VBox>
      <buttons>
         <Button text="Schliessen" press=".onCloseDialog" />
      </buttons>
   </Dialog>
</mvc:View>
```

2. Erweitere den Controller `webapp/controller/MainView.controller.js`:

```javascript
sap.ui.define([
   "sap/ui/core/mvc/Controller",
   "sap/ui/model/json/JSONModel",
   "sap/ui/model/Filter",
   "sap/ui/model/FilterOperator"
], function (Controller, JSONModel, Filter, FilterOperator) {
   "use strict";

   return Controller.extend("sap.ui5.tutorial.controller.MainView", {
      onInit: function () {
         // Lade die Mockdaten
         var oModel = new JSONModel("model/products.json");
         this.getView().setModel(oModel);
      },
      
      onSearch: function (oEvent) {
         // Erstelle Filter basierend auf dem Suchbegriff
         var sQuery = oEvent.getParameter("query");
         var aFilters = [];
         
         if (sQuery) {
            aFilters.push(new Filter("Name", FilterOperator.Contains, sQuery));
         }
         
         // Hole die Liste und wende den Filter an
         var oList = this.byId("productList");
         var oBinding = oList.getBinding("items");
         oBinding.filter(aFilters);
      },
      
      onItemPress: function (oEvent) {
         var oItem = oEvent.getParameter("listItem");
         var oContext = oItem.getBindingContext();
         var oProduct = oContext.getObject();
         
         // Setze die Werte im Dialog
         this.byId("productID").setText(oProduct.ID);
         this.byId("productName").setText(oProduct.Name);
         this.byId("productCategory").setText(oProduct.Category);
         this.byId("productPrice").setText(oProduct.Price + " " + oProduct.Currency);
         
         // Öffne den Dialog
         this.byId("productDetailDialog").open();
      },
      
      onCloseDialog: function () {
         this.byId("productDetailDialog").close();
      }
   });
});
```

### Übung 4: Formular zum Hinzufügen neuer Produkte

Wir fügen ein Formular hinzu, mit dem neue Produkte angelegt werden können:

1. Ergänze die View-Datei `webapp/view/MainView.view.xml`:

```xml
<mvc:View
   controllerName="sap.ui5.tutorial.controller.MainView"
   xmlns="sap.m"
   xmlns:f="sap.ui.layout.form"
   xmlns:mvc="sap.ui.core.mvc">
   <Page title="Produktübersicht">
      <Toolbar>
         <SearchField id="searchField" width="70%" search=".onSearch" />
         <ToolbarSpacer />
         <Button icon="sap-icon://add" text="Neu" press=".onOpenAddDialog" />
      </Toolbar>
      <List
         id="productList"
         items="{/Products}"
         itemPress=".onItemPress">
         <StandardListItem
            title="{Name}"
            description="{Category}"
            info="{Price} {Currency}"
            infoState="Success"
            type="Active"/>
      </List>
   </Page>
   
   <!-- Produkt-Detail-Dialog -->
   <Dialog id="productDetailDialog" title="Produktdetails" contentWidth="50%">
      <VBox class="sapUiSmallMargin">
         <Label text="ID" />
         <Text id="productID" />
         <Label text="Name" />
         <Text id="productName" />
         <Label text="Kategorie" />
         <Text id="productCategory" />
         <Label text="Preis" />
         <Text id="productPrice" />
      </VBox>
      <buttons>
         <Button text="Schliessen" press=".onCloseDialog" />
      </buttons>
   </Dialog>
   
   <!-- Neues Produkt Dialog -->
   <Dialog id="addProductDialog" title="Neues Produkt hinzufügen">
      <f:SimpleForm editable="true" layout="ColumnLayout">
         <f:content>
            <Label text="Name" />
            <Input id="inputName" />
            
            <Label text="Kategorie" />
            <Select id="selectCategory">
               <items>
                  <core:Item text="Electronics" key="Electronics" xmlns:core="sap.ui.core" />
                  <core:Item text="Household" key="Household" xmlns:core="sap.ui.core" />
                  <core:Item text="Computer" key="Computer" xmlns:core="sap.ui.core" />
               </items>
            </Select>
            
            <Label text="Preis" />
            <Input id="inputPrice" type="Number" />
         </f:content>
      </f:SimpleForm>
      <buttons>
         <Button text="Abbrechen" press=".onCloseAddDialog" />
         <Button text="Hinzufügen" type="Emphasized" press=".onAddProduct" />
      </buttons>
   </Dialog>
</mvc:View>
```

2. Erweitere den Controller `webapp/controller/MainView.controller.js`:

```javascript
sap.ui.define([
   "sap/ui/core/mvc/Controller",
   "sap/ui/model/json/JSONModel",
   "sap/ui/model/Filter",
   "sap/ui/model/FilterOperator",
   "sap/m/MessageToast"
], function (Controller, JSONModel, Filter, FilterOperator, MessageToast) {
   "use strict";

   return Controller.extend("sap.ui5.tutorial.controller.MainView", {
      onInit: function () {
         // Lade die Mockdaten
         var oModel = new JSONModel("model/products.json");
         this.getView().setModel(oModel);
      },
      
      onSearch: function (oEvent) {
         // Erstelle Filter basierend auf dem Suchbegriff
         var sQuery = oEvent.getParameter("query");
         var aFilters = [];
         
         if (sQuery) {
            aFilters.push(new Filter("Name", FilterOperator.Contains, sQuery));
         }
         
         // Hole die Liste und wende den Filter an
         var oList = this.byId("productList");
         var oBinding = oList.getBinding("items");
         oBinding.filter(aFilters);
      },
      
      onItemPress: function (oEvent) {
         var oItem = oEvent.getParameter("listItem");
         var oContext = oItem.getBindingContext();
         var oProduct = oContext.getObject();
         
         // Setze die Werte im Dialog
         this.byId("productID").setText(oProduct.ID);
         this.byId("productName").setText(oProduct.Name);
         this.byId("productCategory").setText(oProduct.Category);
         this.byId("productPrice").setText(oProduct.Price + " " + oProduct.Currency);
         
         // Öffne den Dialog
         this.byId("productDetailDialog").open();
      },
      
      onCloseDialog: function () {
         this.byId("productDetailDialog").close();
      },
      
      onOpenAddDialog: function () {
         // Öffne den Dialog zum Hinzufügen eines neuen Produkts
         this.byId("addProductDialog").open();
      },
      
      onCloseAddDialog: function () {
         this.byId("addProductDialog").close();
      },
      
      onAddProduct: function () {
         // Sammle Daten aus dem Formular
         var sName = this.byId("inputName").getValue();
         var sCategory = this.byId("selectCategory").getSelectedKey();
         var fPrice = parseFloat(this.byId("inputPrice").getValue());
         
         // Validiere Eingaben
         if (!sName || !sCategory || isNaN(fPrice)) {
            MessageToast.show("Bitte alle Felder korrekt ausfüllen");
            return;
         }
         
         // Hole bestehende Produkte
         var oModel = this.getView().getModel();
         var aProducts = oModel.getProperty("/Products");
         
         // Erstelle neue ID (einfach für dieses Beispiel)
         var iHighestID = 0;
         aProducts.forEach(function (oProduct) {
            var iID = parseInt(oProduct.ID);
            if (iID > iHighestID) {
               iHighestID = iID;
            }
         });
         
         // Füge neues Produkt hinzu
         aProducts.push({
            "ID": (iHighestID + 1).toString(),
            "Name": sName,
            "Price": fPrice,
            "Currency": "EUR",
            "Category": 
         });
         
         // Aktualisiere Modell
         oModel.setProperty("/Products", aProducts);
         
         // Zeige Bestätigung und schliesse Dialog
         MessageToast.show("Produkt erfolgreich hinzugefügt");
         this.onCloseAddDialog();
         
         // Zurücksetzen des Formulars
         this.byId("inputName").setValue("");
         this.byId("selectCategory").setSelectedKey("");
         this.byId("inputPrice").setValue("");
      }
   });
});
```

### Übung 5: Formular zum Löschen der Produkte

Wir fügen ein Button hinzu mit welchem wir ein Produkt löschen können.

1. Ergänze die View-Datei mit einem weiterem Button `webapp/view/MainView.view.xml`:

```xml
<Button text="Löschen" press=".onDeleteProduct" />  
```

2. Erweitere den Controller mit der onDeleteProduct Funktion `webapp/controller/MainView.controller.js`:

```js
onDeleteProduct: function () { 
         var sProductID = this.byId("productID").getText(); 
          
         var oModel = this.getView().getModel("products"); 
         var aProducts = oModel.getProperty("/Products"); 
          
         var aFilteredProducts = aProducts.filter(function (oProduct) { 
            return oProduct.ID !== sProductID; 
         }); 
          
         oModel.setProperty("/Products", aFilteredProducts); 
          
         MessageToast.show("Produkt erfolgreich gelöscht"); 
         this.onCloseDialog(); 
      }
```

## 5. Zusammenfassung und nächste Schritte

In diesem Tutorial hast du gelernt:
- Wie du ein SAPUI5-Projekt mit Visual Studio Code erstellst
- Die Grundstruktur eines SAPUI5-Projekts kennengelernt
- Eine einfache Liste mit Mockdaten angezeigt
- Eine Suchfunktion implementiert
- Eine Detailansicht für Listenobjekte eingebaut
- Ein Formular zum Hinzufügen neuer Datensätze erstellt

Als nächste Schritte könntest du:
- Die Anwendung um eine Bearbeitungsfunktion erweitern
- Das UI mit zusätzlichen SAPUI5-Controls erweitern
