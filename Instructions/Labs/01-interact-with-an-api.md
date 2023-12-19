---
lab:
  title: 'Übung: Interagieren mit einer minimalen ASP.NET Core-API'
  module: 'Module: Interact with an ASP.NET Core minimal API'
---

In dieser Übung führen Sie eine minimale ASP.NET Core-API lokal aus und erkunden die API und den zugrunde liegenden Code. Sie veröffentlichen die API-App auch in Azure App Service. 

Nach Abschluss dieser Übung können Sie Folgendes:

* Navigieren in einer dokumentierten API
* Ermitteln von Endpunkten für HTTP-Vorgänge
* Identifizieren der API-Anforderungen für HTTP-Vorgänge
* Veröffentlichen einer App in Azure App Service

## Voraussetzungen

Um die Übung durchzuführen, müssen die folgenden Komponenten auf Ihrem System installiert sein:

* [Visual Studio Code](https://code.visualstudio.com)
* [Das aktuelle .NET 7.0 SDK](https://dotnet.microsoft.com/download/dotnet/7.0)
* [Die C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) für Visual Studio Code
* Die Erweiterung [Azure-Ressourcen](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureresourcegroups) für Visual Studio Code
* Die Erweiterung [Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) für Visual Studio Code.
* Ein Azure-Konto mit einem aktiven Abonnement. Wenn Sie noch keines haben, können Sie sich unter [https://azure.com/free](https://azure.com/free) für eine kostenlose Testversion registrieren.

**Geschätzte Bearbeitungszeit dieser Übung:** 30 Minuten

## API-Informationen

Die API interagiert mit einer In-Memory-Datenbank, die die folgenden Felder enthält:

Feld | Typ | Beschreibung
--- | --- | ---
`id` | integer | Schlüssel für die Daten
`name` | Zeichenfolge | Name der Frucht
`instock` | boolean | Gibt an, ob die Frucht auf Lager ist

Die Swagger-Dokumentation wurde mit dem Swashbuckle-Paket erstellt.

>**Hinweis:** Beispieldaten werden jedes Mal erstellt, wenn die API gestartet wird.


## Herunterladen und Ausführen des Fruit-API-Codes

In diesem Abschnitt führen Sie folgende Schritte aus:

* Herunterladen des API-Codes
* Lokales Ausführen der API
* Öffnen der API-Dokumentation in einem Browser

### Aufgabe 1: Herunterladen des API-Codes

1. Klicken Sie mit der rechten Maustaste auf den folgenden Link, und wählen Sie die Option **Link speichern unter** aus. 

    * Code [FruitAPI-Projektcode](https://raw.githubusercontent.com/MicrosoftLearning/APL-2002-develop-aspnet-core-consumes-api/master/Allfiles/Downloads/FruitAPI.zip)

1. Starten Sie **Datei-Explorer **, und navigieren Sie zu dem Speicherort, an dem die Datei gespeichert wurde.

1. Entzippen Sie die Datei in einen eigenen Ordner.

#### Aufgabe 2: Lokales Ausführen der API

1. Starten Sie Visual Studio Code, und wählen Sie in der Menüleiste **Datei** und dann **Ordner öffnen** aus.

1. Navigieren Sie zu dem Speicherort, an dem Sie die Projektdateien entzippt haben, und wählen Sie den Ordner *FruitAPI* aus.

1. Die Projektstruktur im Bereich **Explorer** sollte dem folgenden Screenshot ähneln. Sollte der Bereich **Explorer** nicht sichtbar sein, wählen Sie in der Menüleiste **Ansicht** und anschließend **Explorer** aus.

    ![Screenshot der FruitAPI-Projektstruktur.](media/api-project-structure.png)

1. Öffnen Sie ein Terminal, indem Sie **Terminal** und dann **Neues Terminal** auswählen oder die Tastenkombination **STRG+UMSCHALT+`** verwenden.

1. Führen Sie im Bereich **Terminal** den folgenden `dotnet`-Befehl aus:

    ```
    dotnet run
    ```

1. Es folgt ein Beispiel für die Ausgabe, die im Bereich **Terminal** angezeigt wird. Beachten Sie die Zeile `Now listening on: http://localhost:5050` in der Ausgabe. Sie identifiziert den Host und Port für die API.

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

1. Um die API anzuzeigen, können Sie entweder `http://localhost:5050` in die Adressleiste eingeben, oder Sie können mit **STRG+Klicken** den Link `Now listening on: http://localhost:5050` im zuvor gezeigten **Terminal** öffnen. Auf der Seite wird die Meldung „Diese localhost-Seite kann nicht gefunden werden“ angezeigt.

1. Fügen Sie die URL im Browser mit `/swagger` an. Der Endpunkt `/swagger` ist in der Regel die Stelle, an der Sie die Dokumentation für eine Swagger-API finden. Die vollständige URL für die Swagger-Dokumentation lautet `http://localhost:5050/swagger`. Im Browser sollte jetzt eine Webseite angezeigt werden, die dem folgenden Screenshot ähnelt:

    ![Screenshot der API-Dokumentationsseite.](media/api-home-page.png)

## Ausführen von Vorgängen in der API

In diesem Abschnitt führen Sie folgende Schritte aus:

* Ausführen mehrerer Vorgänge mit den Beispieldaten
* Identifizieren von Endpunkt- und Datenanforderungen für Vorgänge

### Aufgabe 1: Ausführen eines `GET`-Vorgangs

1. Erweitern Sie den **GET**-Vorgang im Abschnitt **Get all fruit**, indem Sie auf eine beliebige Stelle im Feld für den **GET**-Vorgang klicken.

1. Untersuchen Sie die Abschnitte des Vorgangs, und notieren Sie sich die in der folgenden Tabelle gezeigten Informationen.

    | `Section` | Beschreibung |
    |---|--|
    | **Endpunkt** | Wird in der Kopfzeile des Vorgangs angezeigt. Der Endpunkt wird als `/fruitlist` angezeigt. Der vollständige URI ist die Basis-URL für die API, an die der angegebene Endpunkt angehängt ist, unserem Beispiel lautet sie `http://localhost:5050/fruitlist`. |
    | **Parameter** | Für diesen Vorgang ist keine Angabe erforderlich. |
    | **Medientyp** | Gibt die Medientypcodierung an, die der Vorgang zurückgibt. |
    | **Beispielwert** | Zeigt das Schema der vom Vorgang zurückgegebenen Daten an. Beachten Sie, dass dieser Vorgang ein JSON-Array zurückgibt. |

1. Führen Sie den Vorgang aus, indem Sie die Schaltfläche **Jetzt ausprobieren** und dann **Ausführen** auswählen.

1. Der Abschnitt **Antworten** des Vorgangs wurde mit neuen Informationen aktualisiert. Beachten Sie Folgendes:

    * **Anforderungs-URL:** Die URL, auf die im Vorgang zugegriffen wurde.
    * **Serverantwort:** Zeigt den Erfolgscode aus dem Vorgang an, und der **Antworttext** zeigt die drei Beispieldatensätze an.

### Aufgabe 2: Ausführen eines `POST`-Vorgangs

1. Erweitern Sie den **POST**-Vorgang im Abschnitt **Add fruit to list**, indem Sie auf eine beliebige Stelle im Feld für den Vorgang **POST** klicken.

1. Untersuchen Sie die Abschnitte des Vorgangs, und notieren Sie sich die in der folgenden Tabelle gezeigten Informationen.

    | `Section` | Beschreibung |
    |---|--|
    | **Endpunkt** | Der Endpunkt wird als `/fruitlist` angezeigt. Der vollständige URI ist die Basis-URL für die API, an die der angegebene Endpunkt angehängt ist, unserem Beispiel lautet sie `http://localhost:5050/fruitlist`. |
    | **Parameter** | Für diesen Vorgang ist keine Angabe erforderlich. |
    | **Anforderungstext** | Der **Anforderungstext** ist erforderlich, da die API erwartet, dass Daten zur Liste hinzugefügt werden, und sie erwartet den Medientyp `application/json`. |
    | **Beispielwert** | Zeigt das Schema der Daten an, die die API erwartet. |  

1. Um den Vorgang auszuführen, wählen Sie die Schaltfläche **Jetzt ausprobieren** aus. 

1. Ersetzen Sie den JSON-Code im Eingabefeld unter dem Abschnitt **Anforderungstext** durch folgenden Code:

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
    * **Serverantwort:** Zeigt den Erfolgscode aus dem Vorgang an, und der **Antworttext** zeigt den Datensatz an, der zur Datenbank hinzugefügt wurde.

1. Führen Sie den Befehl `GET` im Abschnitt **Get all fruit in list** aus, und beachten Sie, dass jetzt ein Eintrag für *Pear* enthalten ist.

### Aufgabe 3: Ausführen eines `DELETE`-Vorgangs

1. Erweitern Sie den Vorgang **DELETE** im Abschnitt **Delete fruit by Id**, indem Sie auf eine beliebige Stelle im Feld für den Vorgang **DELETE** klicken.

1. Untersuchen Sie die Abschnitte des Vorgangs, und notieren Sie sich die in der folgenden Tabelle gezeigten Informationen.

    | `Section` | Beschreibung |
    |---|--|
    | **Endpunkt** | Der Endpunkt wird als `/fruitlist/{id}` angezeigt. Der vollständige URI ist die Basis-URL für die API, der die angegebene `id` zum Löschen angefügt wurde. Beispielsweise zeigt `http://localhost:5050/fruitlist/1` auf den Datensatz, für den `id` gleich `1` ist.
    | **Parameter** | Erfordert, dass die `id` des Datensatzes in der Anforderungs-URL übergeben wird. |

1. Um den Vorgang auszuführen, wählen Sie die Schaltfläche **Jetzt ausprobieren** aus. 

1. Löschen Sie den Datensatz `Apple` in den Beispieldaten, indem Sie `1` im Feld `id` im Abschnitt **Parameter** eingeben und dann **Ausführen** auswählen.

1. Der Abschnitt **Antworten** des Vorgangs wurde mit neuen Informationen aktualisiert. Beachten Sie Folgendes:

    * **Anforderungs-URL:** Die URL, auf die im Vorgang zugegriffen wurde.
    * **Antworttext:** Zeigt den gelöschten Datensatz an.
    * **Code:** Zeigt den Erfolgscode aus dem Vorgang an.

1. Führen Sie den Befehl `GET` im Abschnitt **Get all fruit in list** aus, und beachten Sie, dass der Datensatz für *Apple* jetzt gelöscht wird.

Wenn Sie bereit sind, mit dem nächsten Abschnitt der Übung fortzufahren:

* Schließen Sie den Browser, und beenden Sie die Fruit-API, indem Sie in dem Terminal, in dem sie ausgeführt wird, **STRG+C** drücken.

## Veröffentlichen der API in Azure App Service

In diesem Abschnitt führen Sie folgende Schritte aus:

* Verwenden der Azure-Ressourcen-Erweiterung zum Herstellen einer Verbindung mit Azure
* Verwenden der Azure App Service-Erweiterung zum Veröffentlichen der API in App Service

### Aufgabe 1: Anmelden bei Azure

1. Wählen Sie die Azure-Ressourcen-Erweiterung aus, um den Bereich zu öffnen.

    ![Screenshot des Symbols der Azure-Ressourcen-Erweiterung und der ersten Optionen.](media/01-azure-resources-ext.png)

1. Wählen Sie **Bei Azure anmelden** aus.

    Es wird ein Browserfenster geöffnet, in dem Sie aufgefordert werden, sich bei Ihrem Azure-Konto anzumelden. Sie können dieses Fenster schließen, nachdem der Anmeldevorgang abgeschlossen wurde. 

1. Nachdem die Anmeldung abgeschlossen ist, zeigt die Erweiterung eine Liste der in Ihrem Konto verfügbaren Abonnements an. Ein Beispiel ist im folgenden Screenshot zu sehen.

    ![Screenshot mit dem Inhalt des Erweiterungsbereichs nach der Anmeldung.](media/01-azure-subscriptions.png)

### Aufgabe 2: Erstellen einer neuen Web-App

1. Wählen Sie **STRG+UMSCHALT+P** aus, um die Befehlspalette zu öffnen, und geben Sie **Neue Web-App erstellen** ein, um die Liste zu filtern. Wählen Sie dann die Option **Azure App Service: Neue Web App erstellen... (erweitert)** aus. 

1. Wenn Ihr Konto über mehrere Abonnements verfügt, werden Sie aufgefordert, das Abonnement auszuwählen, das Sie für die Bereitstellung verwenden möchten. 

1. Geben Sie einen global eindeutigen Namen für die neue Web-App ein. Sie können `fruitapi-<name>` ausprobieren und `<name>` durch Ihren Namen oder Ihre Initialen ersetzen.

1. Wählen Sie **+Neue Ressourcengruppe erstellen** aus, und akzeptieren Sie den Standardwert, oder geben Sie `fruitapi-rg` ein.

1. Wählen Sie **.NET 7 (STS)** als Laufzeitstapel aus.

1. Wählen Sie **Linux** als Betriebssystem aus.

1. Wählen Sie einen Standort in Ihrer Nähe für neue Ressourcen aus.

1. Wählen Sie **Einen neuen App Service-Plan erstellen** aus, und akzeptieren Sie den Standardwert, oder geben Sie einen anderen Namen ein. 

1. Wählen Sie **Kostenlos (F1) Azure kostenlos ausprobieren** als Preistarif aus.

1. Wählen Sie **Vorerst überspringen** aus, wenn Sie aufgefordert werden, eine neue Application Insights-Ressource anzugeben.

Das Tool erstellt die erforderlichen Ressourcen in Azure und kompiliert den Code.

### Aufgabe 3: Bereitstellen der Web-App und Durchsuchen der ausgeführten Website

1. Nachdem die Ressourcen erstellt wurden und die Kompilierung des Codes abgeschlossen ist, wird ein Fenster angezeigt, in dem Sie zum **Bereitstellen** aufgefordert werden. Wählen Sie die Option **Bereitstellen** aus. 

    Das System erstellt eine Releaseversion des Codes und stellt sie für die Ressourcen, die Sie zuvor erstellt haben, bereit.

1. Wenn die Bereitstellung abgeschlossen wurde, wird ein neues Popupfenster mit der Option **Website durchsuchen** angezeigt. Wählen Sie **Website durchsuchen** aus.

1. Fügen Sie in dem Browserfenster, das geöffnet wird, am Ende der URL `/swagger` hinzu. 

Herzlichen Glückwunsch! Sie haben die API in Azure App Service bereitgestellt.

>**Hinweis:** Es empfiehlt sich, mehr benötigte Ressourcen aus Azure zu löschen. Sie können alle in diesem Abschnitt der Übung erstellten Ressourcen entfernen, indem Sie die zuvor im Azure-Portal erstellte Ressourcengruppe löschen.

## Überprüfung

In dieser Übung haben Sie Folgendes gelernt:

* Navigieren in einer dokumentierten API
* Ermitteln von Endpunkten für HTTP-Vorgänge
* Identifizieren der API-Anforderungen für HTTP-Vorgänge
* Veröffentlichen einer App in Azure App Service 
