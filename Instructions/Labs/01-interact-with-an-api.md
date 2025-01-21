---
lab:
  title: 'Übung: Interagieren mit einer minimalen ASP.NET Core-API'
  module: 'Module: Interact with an ASP.NET Core minimal API'
---

In dieser Übung führen Sie eine minimale ASP.NET Core-API lokal aus und erkunden die API und den zugrunde liegenden Code. Sie veröffentlichen auch die API in Azure App Service. 

Nach Abschluss dieser Übung können Sie Folgendes:

* Navigieren in einer dokumentierten API
* Ermitteln von Endpunkten für HTTP-Vorgänge
* Identifizieren der API-Anforderungen für HTTP-Vorgänge
* Veröffentlichen einer App in Azure App Service

## Voraussetzungen

Um die Übung durchzuführen, müssen die folgenden Komponenten auf Ihrem System installiert sein:

* [Visual Studio Code](https://code.visualstudio.com)
* [Das aktuelle .NET 8.0 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)
* [Die C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) für Visual Studio Code
* Die Erweiterung [Azure-Ressourcen](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureresourcegroups) für Visual Studio Code
* Die Erweiterung [Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) für Visual Studio Code.
* Ein Azure-Konto mit einem aktiven Abonnement. Wenn Sie noch keines haben, können Sie sich unter [https://azure.com/free](https://azure.com/free) für eine kostenlose Testversion registrieren.

**Geschätzte Bearbeitungszeit dieser Übung:** 30 Minuten

## API-Informationen

Die API interagiert mit einer In-Memory Database, die die folgenden Felder enthält:

Feld | type | Beschreibung
--- | --- | ---
`id` | integer | Schlüssel für die Daten
`name` | Zeichenfolge | Name der Frucht
`instock` | boolean | Gibt an, ob die Frucht vorrätig ist

Die Swagger-Dokumentation wurde mit dem Swashbuckle-Paket erstellt.

>**Hinweis:** Beispieldaten werden jedes Mal erstellt, wenn die API gestartet wird.


## Herunterladen und Ausführen des Fruit-API-Codes

In diesem Abschnitt führen Sie folgende Schritte aus:

* Herunterladen des API-Codes
* Lokales Ausführen der API
* Öffnen der API-Dokumentation in einem Browser

### Aufgabe 1: Herunterladen des API-Codes

1. Klicken Sie mit der rechten Maustaste auf den folgenden Link, und wählen Sie **Link speichern unter** aus. 

    * [FruitAPI-Projektcode](https://raw.githubusercontent.com/MicrosoftLearning/APL-2002-develop-aspnet-core-consumes-api/master/Allfiles/Downloads/FruitAPI.zip)

1. Starten Sie **Explorer**, und navigieren Sie zum Speicherort der Datei.

1. Entzippen Sie die Datei in ihren eigenen Ordner.

#### Aufgabe 2: Lokales Ausführen der API

1. Starten Sie Visual Studio Code, und wählen Sie in der Menüleiste **Datei** und dann **Ordner öffnen** aus.

1. Navigieren Sie zu dem Speicherort, an dem Sie die Projektdateien entzippt haben, und wählen Sie den Ordner *FruitAPI* aus.

1. Die Projektstruktur im Bereich **Explorer** sollte dem folgenden Screenshot ähneln. Sollte der Bereich **Explorer** nicht sichtbar sein, wählen Sie in der Menüleiste **Ansicht** und anschließend **Explorer** aus.

    ![Screenshot der FruitAPI-Projektstruktur.](media/api-project-structure.png)

1. Öffnen Sie ein Terminal, indem Sie **Terminal** und dann **Neues Terminal** auswählen oder die Tastenkombination **STRG+UMSCHALT+`** verwenden.

1. Führen Sie im Bereich **Terminal** den folgenden `dotnet`-Befehl aus:

    ```
    dotnet run
    ```

1. Es folgt ein Beispiel für die Ausgabe, die im Bereich **Terminal** angezeigt wird. Beachten Sie die Zeile `Now listening on: http://localhost:5050` in der Ausgabe. Sie identifiziert den Host und den Port für die API.

    ```
    info: Microsoft.EntityFrameworkCore.Update[30100]
          Saved 3 entities to in-memory store.
    info: Microsoft.Hosting.Lifetime[14]
          Now listening on: http://localhost:5050
    info: Microsoft.Hosting.Lifetime[0]
          Application started. Press Ctrl+C to shut down.
    info: Microsoft.Hosting.Lifetime[0]
          Hosting environment: Development
    info: Microsoft.Hosting.Lifetime[0]
          Content root path: 
          <project location>
    ```

### Aufgabe 3: Öffnen der API-Dokumentation in einem Browser

1. Um die API anzuzeigen, können Sie entweder `http://localhost:5050` in die Adressleiste eingeben, oder Sie können **STRG+Klicken** auf den Link `Now listening on: http://localhost:5050` im zuvor gezeigten **Terminal** anwenden. Auf der Seite wird die Meldung „Diese Localhost-Seite kann nicht gefunden werden“ angezeigt.

1. Hängen Sie `/swagger` an die URL im Browser an. Der `/swagger`-Endpunkt ist in der Regel der Ort, an dem Sie die Dokumentation für eine Swagger-API finden. Die vollständige URL für die Swagger-Dokumentation lautet `http://localhost:5050/swagger`. Im Browser sollte jetzt eine Webseite angezeigt werden, die dem folgenden Screenshot ähnelt:

    ![Screenshot der API-Dokumentationsseite.](media/api-home-page.png)

## Ausführen von Vorgängen in der API

In diesem Abschnitt führen Sie folgende Schritte aus:

* Ausführen mehrerer Vorgänge für die Beispieldaten
* Identifizieren von Endpunkt- und Datenanforderungen für Vorgänge

### Aufgabe 1: Ausführen eines `GET`-Vorgangs

1. Erweitern Sie den **GET**-Vorgang mit dem Deskriptor **Get all fruits**, indem Sie auf eine beliebige Stelle im Feld des **GET**-Vorgangs klicken.

1. Untersuchen Sie die Abschnitte des Vorgangs, und notieren Sie sich die in der folgenden Tabelle gezeigten Informationen.

    | Abschnitt | Beschreibung |
    |---|--|
    | **Endpunkt** | Wird in der Kopfzeile des Vorgangs angezeigt. Der Endpunkt wird als `/fruits` angezeigt. Der vollständige URI ist die Basis-URL für die API, dem der angegebene Endpunkt angefügt ist, in unserem Beispiel `http://localhost:5050/fruits`. |
    | **Parameter** | Für diesen Vorgang sind keine erforderlich. |
    | **Medientyp** | Gibt die Medientypcodierung an, die der Vorgang zurückgibt. |
    | **Beispielwert** | Zeigt das Schema der vom Vorgang zurückgegebenen Daten an. Beachten Sie, dass dieser Vorgang ein JSON-Array zurückgibt. |

1. Führen Sie den Vorgang aus, indem Sie die Schaltfläche **Jetzt ausprobieren** und dann **Ausführen** auswählen.

1. Der Abschnitt **Antworten** des Vorgangs wurde mit neuen Informationen aktualisiert. Beachten Sie Folgendes:

    * **Anforderungs-URL:** Die URL, auf die im Vorgang zugegriffen wurde.
    * **Serverantwort:** Zeigt den Erfolgscode aus dem Vorgang an, und der  **Antworttext** zeigt die drei Beispieleinträge an.

### Aufgabe 2: Ausführen eines `POST`-Vorgangs

1. Erweitern Sie den Vorgang **POST** mit dem Deskriptor **Create a new fruit**, indem Sie auf eine beliebige Stelle im Feld des **POST**-Vorgangs klicken.

1. Untersuchen Sie die Abschnitte des Vorgangs, und notieren Sie sich die in der folgenden Tabelle gezeigten Informationen.

    | Abschnitt | Beschreibung |
    |---|--|
    | **Endpunkt** | Der Endpunkt wird als `/fruits` angezeigt. Der vollständige URI ist die Basis-URL für die API, dem der angegebene Endpunkt angefügt ist, in unserem Beispiel `http://localhost:5050/fruits`. |
    | **Parameter** | Für diesen Vorgang sind keine erforderlich. |
    | **Anforderungstext** | Der **Anforderungstext** ist erforderlich, da die API erwartet, dass Daten zur Liste hinzugefügt werden, und sie erwartet den Medientyp `application/json`. |
    | **Beispielwert** | Zeigt das Schema der Daten an, die die API erwartet. |  

1. Um den Vorgang auszuführen, wählen Sie die Schaltfläche **Jetzt ausprobieren** aus. 

1. Ersetzen Sie den JSON-Code im Eingabefeld unter dem Abschnitt **Anforderungstext** durch Folgendes:

    ```json
    {
        "id": 0,
        "name": "Pear",
        "instock": true
    }
    ```

    >**Hinweis:** Die Datenbank weist beim Hinzufügen von Daten einen eigenen Indexwert zu, sodass nur ein Wert im Feld `id` vorhanden sein muss.

1. Der Abschnitt **Antworten** des Vorgangs wurde mit neuen Informationen aktualisiert. Beachten Sie Folgendes:

    * **Anforderungs-URL:** Die URL, auf die im Vorgang zugegriffen wurde.
    * **Serverantwort:** Zeigt den Erfolgscode aus dem Vorgang an, und der **Antworttext** zeigt den Eintrag an, der der Datenbank hinzugefügt wurde.

1. Führen Sie den Befehl `GET` im Abschnitt **Get all fruits** aus, und beachten Sie, dass jetzt ein Eintrag für *Pear* enthalten ist.

### Aufgabe 3: Ausführen eines `DELETE`-Vorgangs

1. Erweitern Sie den Vorgang **DELETE** mit dem Deskriptor **Delete a fruit by Id**, indem Sie auf eine beliebige Stelle im Feld des Vorgangs **DELETE** klicken.

1. Untersuchen Sie die Abschnitte des Vorgangs, und notieren Sie sich die in der folgenden Tabelle gezeigten Informationen.

    | Abschnitt | Beschreibung |
    |---|--|
    | **Endpunkt** | Der Endpunkt wird als `/fruits/{id}` angezeigt. Der vollständige URI ist die Basis-URL für die API, dem `id` zum Löschen angefügt wurde. Beispielsweise verweist `http://localhost:5050/fruits/1` auf den Eintrag, wobei `id` gleich `1` ist.
    | **Parameter** | Erfordert, dass die `id` des Eintrags in der Anforderungs-URL übergeben wird. |

1. Um den Vorgang auszuführen, wählen Sie die Schaltfläche **Jetzt ausprobieren** aus. 

1. Löschen Sie Eintrag `Apple` in den Beispieldaten, indem Sie im Feld `id` im Abschnitt **Parameter** einen `1` eingeben und dann **Ausführen** auswählen.

1. Der Abschnitt **Antworten** des Vorgangs wurde mit neuen Informationen aktualisiert. Beachten Sie Folgendes:

    * **Anforderungs-URL:** Die URL, auf die im Vorgang zugegriffen wurde.
    * **Antworttext:** Zeigt den gelöschten Eintrag an.
    * **Code:** Zeigt den Erfolgscode aus dem Vorgang an.

1. Führen Sie den Befehl `GET` im Abschnitt **Get all fruits** aus, und beachten Sie, dass der Eintrag für *Apple* jetzt gelöscht ist.

Wenn Sie bereit sind, mit dem nächsten Abschnitt der Übung fortzufahren:

* Schließen Sie den Browser, und beenden Sie die Fruit-API, indem Sie in dem Terminal, in dem sie ausgeführt wird, **STRG+C** drücken.

## Veröffentlichen der API in Azure App Service

In diesem Abschnitt führen Sie folgende Schritte aus:

* Verwenden der Erweiterung „Azure-Ressourcen“ zum Herstellen einer Verbindung mit Azure
* Verwenden der Erweiterung „Azure App Service“ zum Veröffentlichen der API in App Service

### Aufgabe 1: Anmelden bei Azure

1. Wählen Sie die Erweiterung „Azure-Ressourcen“ aus, um den Bereich zu öffnen.

    ![Screenshot mit dem Symbol der Erweiterung „Azure Ressourcen“ und den anfänglichen Optionen.](media/01-azure-resources-ext.png)

1. Wählen Sie **Bei Azure anmelden** aus.

    Es wird ein Browserfenster geöffnet, in dem Sie aufgefordert werden, sich bei Ihrem Azure-Konto anzumelden. Sie können dieses Fenster schließen, wenn der Anmeldevorgang abgeschlossen ist. 

1. Wenn die Anmeldung abgeschlossen ist, zeigt die Erweiterung eine Liste der in Ihrem Konto verfügbaren Abonnements an. Ein Beispiel ist im folgenden Screenshot zu sehen.

    ![Screenshot mit dem Inhalt des Erweiterungsbereichs nach der Anmeldung.](media/01-azure-subscriptions.png)

### Aufgabe 2: Erstellen einer neuen Web-App

1. Wählen Sie **STRG+UMSCHALT+P** aus, um die Befehlspalette zu öffnen, und geben Sie **Neue Web-App erstellen** ein, um die Liste zu filtern, und wählen Sie die Option **Azure App Service: Neue Web App erstellen (erweitert)** aus. 

1. Wenn Ihr Konto über mehrere Abonnements verfügt, werden Sie aufgefordert, das Abonnement auszuwählen, das Sie für die Bereitstellung verwenden möchten. 

1. Geben Sie einen global eindeutigen Namen für die neue Web-App ein. Sie können es mit `fruitapi-<name>` versuchen und `<name>` durch Ihren Namen oder Ihre Initialen ersetzen.

1. Wählen Sie **+Neue Ressourcengruppe erstellen** aus, und akzeptieren Sie den Standardwert oder geben Sie `fruitapi-rg` ein.

1. Wählen Sie **.NET 8 (LTS)** als Laufzeitstapel aus.

1. Wählen Sie **Linux** als Betriebssystem aus.

1. Wählen Sie einen Standort in Ihrer Nähe für die neuen Ressourcen aus.

1. Wählen Sie **Einen neuen App Services-Plan erstellen** aus, und akzeptieren Sie den Standardwert oder geben Sie einen anderen Namen ein. 

1. Wählen Sie **Free (F1) Azure kostenlos ausprobieren** als Tarif aus.

1. Wählen Sie **Vorerst überspringen** aus, wenn Sie aufgefordert werden, eine neue Application Insights-Ressource auszuwählen.

Das Tool erstellt die erforderlichen Ressourcen in Azure und kompiliert den Code.

### Aufgabe 3: Bereitstellen der Web-App und Durchsuchen der ausgeführten Website

1. Nachdem die Ressourcen erstellt wurden und der Code die Kompilierung eines Fensters abgeschlossen hat, werden Sie zum **Bereitstellen** aufgefordert. Wählen Sie die Option **Bereitstellen** aus. 

    Das System erstellt eine Releaseversion des Codes und stellt sie für die Ressourcen bereit, die Sie zuvor erstellt haben.

1. Wenn die Bereitstellung abgeschlossen ist, wird ein neues Popup mit der Option zum **Durchsuchen der Website** angezeigt. Wählen Sie **Website durchsuchen** aus.

1. In dem Browserfenster, das geöffnet wird, müssen Sie am Ende der URL möglicherweise `/fruits` hinzufügen. Die unformatierte Ausgabe aus der API zeigt alle Daten an.

    >**HINWEIS:** Die Swagger-Benutzeroberfläche ist deaktiviert, da sie nur für Entwicklungsumgebungen aktiviert ist. Die Bereitstellung in App Service gilt als Nicht-Entwicklungsumgebung, es sei denn, Sie führen eine zusätzliche Konfiguration aus.

Herzlichen Glückwunsch! Sie haben die API in Azure App Service bereitgestellt.

>**Hinweis:** Es empfiehlt sich, Ressourcen aus Azure zu löschen, die Sie nicht mehr benötigen. Sie können alle Ressourcen entfernen, die in diesem Abschnitt der Übung erstellt wurden, indem Sie die Ressourcengruppe löschen, die zuvor im Azure-Portal erstellt wurde.

## Überprüfung

In dieser Übung haben Sie Folgendes gelernt:

* Navigieren in einer dokumentierten API
* Ermitteln von Endpunkten für HTTP-Vorgänge
* Identifizieren der API-Anforderungen für HTTP-Vorgänge
* Veröffentlichen einer App in Azure App Service 
