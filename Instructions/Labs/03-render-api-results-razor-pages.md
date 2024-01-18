---
lab:
  title: Rendern von API-Antworten in ASP.NET Core Razor Pages
  module: 'Module: Render API responses in ASP.NET Core Razor Pages'
---

In dieser Übung erfahren Sie, wie Sie Antworten von einer API in einer ASP.NET Core Razor Pages-App rendern. Dieser Code wird den *CSHTML-Dateien* hinzugefügt. Der Code, der die Vorgänge in den *CSHTML.cs-Dateien* ausführt, ist abgeschlossen.

## Ziele

In diesem Lab lernen Sie Folgendes:

* Implementieren von Razor-Schlüsselwort (keyword) in einer App
* Integrieren von C#-Code in razor Pages-Syntax

## Voraussetzungen

Um die Übung durchzuführen, müssen die folgenden Tools auf Ihrem Computer installiert sein:

* [Visual Studio Code](https://code.visualstudio.com)
* [Das aktuelle .NET 7.0 SDK.](https://dotnet.microsoft.com/download/dotnet/7.0)
* Die C#-Erweiterung für Visual Studio Code

**Geschätzte Zeit bis zum Abschluss dieser Übung:** 15 bis 20 Minuten

## Übungsszenario

Diese Übung verfügt über zwei Komponenten:

* Eine Web-App, die HTTP-Anforderungen an eine API sendet. Die App wird auf `http://localhost:5010`
* Eine API, die auf HTTP-Anforderungen antwortet. Die API wird auf `http://localhost:5050`

![Dekorativ](media/02-architecture.png)

## Laden Sie den Code herunter.

In diesem Abschnitt laden Sie den Code für die Fruit Web App und die Fruit-API herunter. Sie führen die Fruit-API auch lokal aus, damit sie für die Web-App verfügbar ist.

### Aufgabe 1: Herunterladen und Ausführen des API-Codes

1. Klicken Sie mit der rechten Maustaste auf den folgenden Link, und wählen Sie **Link speichern unter...** aus: 

    * [FruitAPI-Projektcode](https://raw.githubusercontent.com/MicrosoftLearning/APL-2002-develop-aspnet-core-consumes-api/master/Allfiles/Downloads/FruitAPI.zip)

1. Starten Sie Explorer **, und navigieren Sie **zum Speicherort, an dem die Datei gespeichert wurde.

1. Entpacken Sie die Datei in ihren eigenen Ordner.

1. Öffnen Sie **Windows-Terminal** oder eine **Eingabeaufforderung**, und navigieren Sie zu dem Speicherort, an dem Sie den Code für die API extrahiert haben.

1. Führen Sie in einem Terminalfenster folgenden Befehl aus:

    ```
    dotnet run
    ```

1. Im Folgenden finden Sie ein Beispiel für die Ausgabe. Notieren Sie sich die `Now listening on: http://localhost:5050` Zeile in der Ausgabe. Er identifiziert den Host und port für die API.

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

>**Hinweis:** Lassen Sie die Frucht-API während der restlichen Übung laufen. 

### Aufgabe 2: Herunterladen und Öffnen des Web-App-Projekts

1. Klicken Sie mit der rechten Maustaste auf den folgenden Link, und wählen Sie **Link speichern unter...** aus: 

    * [Obst-Web-App-Renderprojektcode](https://raw.githubusercontent.com/MicrosoftLearning/APL-2002-develop-aspnet-core-consumes-api/master/Allfiles/Downloads/FruitWebApp-render.zip)

1. Starten Sie Explorer **, und navigieren Sie **zum Speicherort, an dem die Datei gespeichert wurde.

1. Entpacken Sie die Datei in ihren eigenen Ordner.

1. Wählen Sie in Visual Studio Code das Menü **Datei** und dann **Ordner öffnen** aus.

1. Navigieren Sie zu dem Speicherort, an dem Sie die Projektdateien entzippt haben, und wählen Sie den *FruitWebApp-Renderordner* aus.

1. Die Projektstruktur im **Explorer-Bereich** sollte dem folgenden Screenshot ähneln. Sollte der Bereich „Server-Explorer“ nicht sichtbar sein, wählen Sie im oberen Menü die Option Ansicht und anschließend Server-Explorer aus.

    ![Screenshot der Projektstruktur der Fruit Web App.](media/03-web-app-render-structure.png)

>**Hinweis:** Nehmen Sie sich Zeit, um den Code in den einzelnen Dateien zu überprüfen, die während dieser Übung bearbeitet werden. Der Code ist stark kommentiert und kann Ihnen dabei helfen, die Codebasis zu verstehen.

## Implementieren von Code zum Rendern von Daten auf der `Index` Seite

Die Fruit Web App zeigt die API-Beispieldaten auf der Startseite an. Sie müssen Code hinzufügen, um die Beispieldaten zu durchlaufen, die von dem HTTP-Vorgang `GET` zurückgegeben werden, der in der CodeBehind-Datei ausgeführt wird.

### Aufgabe 1: Hinzufügen von Code zum Rendern von Daten in einer Tabelle

1. Wählen Sie im Explorer-Bereich die Datei Program.cs aus, um sie im Editor zu öffnen.

1. Fügen Sie den folgenden Code vor dem Kommentar  hinzu:

    ```csharp
    <tbody>
        
        @*  The Razor keyword @foreach is used to iterate through the
            data returned to the data model from the HTTP operations. *@
        @foreach (var obj in Model.FruitModels)
        {
            <tr>
                @* Display the name of the fruit. *@
                <td width="50%">@obj.name</td>
                @*  The following if statment is a Razor code block that changes the color 
                    and icon of the available indicator in the page rendering. *@
                @{
                    if (@obj.instock)
                    {
                        <td width="20%" class="text-md-center">
                            <i class="bi bi-check-circle" style="font-size: 1rem; color: green;"></i>&nbsp;Yes
                        </td>
                    }
                    else
                    {
                        <td width="20%" class="text-md-center">
                            <i class="bi bi-dash-circle" style="font-size: 1rem; color:red;"></i>&nbsp;No
                        </td>
                    }
                }
                <td width="30%" class="text-center">
                    @*  The following div contains information to handle the edit and delete functions. *@
                    <div class="w-75 btn-group btn-group-sm" role="group" style="text-align:center">
                        @* Routes to the Edit page and passes the id of the record. *@
                        <a asp-page="Edit" asp-route-id="@obj.id" class="btn btn-primary  mx-2">
                            <i class="bi bi-pencil-square"></i> Edit
                        </a>
                        @* Routes to the Delete page and passes the id of the record. *@
                        <a asp-page="Delete" asp-route-id="@obj.id" class="btn btn-danger mx-2">
                            <i class="bi bi-trash"></i> Delete
                        </a>
                    </div>
                </td>
            </tr>
        }
    </tbody>
    ```

1. Speichern Sie die Änderungen in *Index.cshtml*, und überprüfen Sie die Kommentare im Code.

1. Drücken Sie in Visual Studio Code F5, oder wählen Sie im Menü Ausführen die Option Debuggen starten aus. Nach Abschluss des Projekts sollte das Erstellen eines Browserfensters mit der ausgeführten Web-App gestartet werden.

1. Überprüfen Sie, ob auf der Seite "Index" die Beispieldaten aus der API angezeigt werden.

    >**Hinweis:** Die **Funktionen "Zu Liste** hinzufügen", **"Bearbeiten**" und **"Löschen** " funktionieren erst, wenn Sie später in dieser Übung Code für sie hinzufügen.

    >**Hinweis:** Sie können die folgende Eingabeaufforderung sicher ignorieren, wenn sie angezeigt wird, wenn Sie die App ausführen.

    ![Screenshot: Option zum Verwenden eines selbstsignierten Zertifikats.](media/install-cert.png)

1. Wenn Sie mit der Übung fortfahren möchten, schließen Sie den Browser oder die Registerkarte "Browser", und wählen Sie **\| in Visual Studio Code "Debugging beenden"** oder **"UMSCHALT+F5**" aus.

## Implementieren von Code zur Behandlung der `Add to list` Funktionalität

Die Vorgänge zum Hinzufügen, Bearbeiten und Löschen werden jeweils auf einer separaten *CSHTML-Seite* im Projekt behandelt. In diesem Abschnitt fügen Sie Code zum Erstellen eines Formulars in der *Datei "Add.cshtml* " hinzu, um das Hinzufügen von Daten zur Liste zu ermöglichen.

### Aufgabe 1: Hinzufügen von Code zum Erstellen des Formulars zum Hinzufügen von Daten

1. Wählen Sie im Explorer-Bereich die Datei Program.cs aus, um sie im Editor zu öffnen.

1. Fügen Sie den folgenden Code vor dem Kommentar  hinzu:

    ```csharp
    <form method="post">
        @*  The FruitModels.id is here so the full data model is represented on the page.
            The database behind the API will assign the id. *@
        <input hidden asp-for="FruitModels.id" />
        <div class="border p-3 mt-4" style="width:50%">
            <div class="row pb-2">
                <h2 class="text-primary pl-3">Add Fruit</h2>
                <hr />
            </div>
            <div class="mb-3">
                <label asp-for="FruitModels.name" class="h5"></label><br/>
                @* Empty text box for the name of the fruit to be added. *@
                <input type="text" asp-for="FruitModels.name" />
                <span asp-validation-for="FruitModels.name" class="text-danger"></span>
            </div>
            <div class="mb-3">
                <label asp-for="FruitModels.instock" class="h5"></label><br/>
                @* Render the true/false instock state from the record in an editable checkbox. *@
                <input type="checkbox" asp-for="FruitModels.instock" style="width:20px; height:20px" />
                <label class="h7"><i class="bi bi-arrow-left"></i>  Check the box if it's available.</label>
                <span asp-validation-for="FruitModels.instock" class="text-danger"></span>
            </div>
            @* Submit the addition or return to the Index page if the Add is cancelled.*@
            <button type="submit" class="btn btn-primary" style="width:150px;">Create</button>
            <a asp-page="Index" class="btn btn-secondary" style="width:150px;">Cancel</a>
        </div>
    </form>
    ```

1. Speichern Sie die Änderungen in *"Add.cshtml*", und überprüfen Sie die Kommentare im Code.

1. Drücken Sie in Visual Studio Code F5, oder wählen Sie im Menü Ausführen die Option Debuggen starten aus. Nach Abschluss des Projekts sollte das Erstellen eines Browserfensters mit der ausgeführten Web-App gestartet werden.

1. Wählen Sie auf der Listenseite **Neu** aus.

1. Geben Sie den Namen einer Frucht ein, die Sie der Liste hinzufügen möchten, und aktivieren Sie das Kontrollkästchen, um anzugeben, dass es verfügbar ist.

1. Wählen Sie " **Erstellen"** aus, um den Eintrag zur Liste hinzuzufügen, und Sie werden zurück zur Startseite weitergeleitet. Überprüfen Sie, ob Der Eintrag der Liste hinzugefügt wurde.

1. Wenn Sie mit der Übung fortfahren möchten, schließen Sie den Browser oder die Registerkarte "Browser", und wählen Sie **\| in Visual Studio Code "Debugging beenden"** oder **"UMSCHALT+F5**" aus.

## Implementieren von Code zur Behandlung der `Edit` Funktionalität

In diesem Abschnitt fügen Sie Code zum Erstellen eines Formulars in der *Datei Edit.cshtml* hinzu, um die Bearbeitung von Daten in der Liste zu ermöglichen.

### Aufgabe 1: Hinzufügen von Code für das Bearbeitungsformular

1. Wählen Sie die *Datei Edit.cshtml* im  **Explorer-Bereich** aus, um sie zur Bearbeitung zu öffnen.

1. Fügen Sie den folgenden Code vor dem Kommentar  hinzu:

    ```csharp
    <form method="post">
        @*  The id for the data record is hidden because it needs to be available to the 
            code-behind processing, but it's not displayed. *@
        <input hidden asp-for="FruitModels.id" />
        <div class="border p-3 mt-4" style="width:50%">
            <div class="row pb-2">
                <h2 class="text-primary pl-3">Edit Fruit</h2>
                <hr />
            </div>
            <div class="mb-3">
                <label asp-for="FruitModels.name" class="h5"></label><br/>
                @* Render the current name of the fruit in an editable text box. *@
                <input type="text" asp-for="FruitModels.name" />
                <span asp-validation-for="FruitModels.name" class="text-danger"></span>
            </div>
            <div class="mb-3">
                <label asp-for="FruitModels.instock" class="h5"></label><br/>
                @* Render the true/false instock state from the record in an editable checkbox. *@
                <input type="checkbox" asp-for="FruitModels.instock" style="width:20px; height:20px" />
                <label class="h7"><i class="bi bi-arrow-left"></i>  Check the box if available.</label>
                <span asp-validation-for="FruitModels.instock" class="text-danger"></span>
            </div>
            @* Submit the changes or return to the Index page if the edit is cancelled.*@
            <button type="submit" class="btn btn-primary" style="width:150px;">Update</button>
            <a asp-page="Index" class="btn btn-secondary" style="width:150px;">Cancel</a>
        </div>
    </form>
    ```

1. Speichern Sie die Änderungen in *Edit.cshtml*, und überprüfen Sie die Kommentare im Code.

1. Drücken Sie in Visual Studio Code F5, oder wählen Sie im Menü Ausführen die Option Debuggen starten aus. Nach Abschluss des Projekts sollte das Erstellen eines Browserfensters mit der ausgeführten Web-App gestartet werden.

1. Wählen Sie ein Element in der Liste aus, um es zu ändern, und wählen Sie **"Bearbeiten** " in dieser Zeile aus.

1. Bearbeiten Sie den Namen der Frucht, und aktivieren Sie das Kontrollkästchen, um den Verfügbarkeitsstatus zu ändern.

1. Wählen Sie **"Aktualisieren"** aus, um Ihre Änderungen zu speichern, und Sie werden zurück zur Startseite weitergeleitet. Überprüfen Sie, ob Ihre Änderung in der Liste angezeigt wird.

1. Wenn Sie mit der Übung fortfahren möchten, schließen Sie den Browser oder die Registerkarte "Browser", und wählen Sie **\| in Visual Studio Code "Debugging beenden"** oder **"UMSCHALT+F5**" aus.

## Implementieren von Code zur Behandlung der `Delete` Funktionalität

In diesem Abschnitt fügen Sie Code zum Erstellen eines Formulars in der *Datei Delete.cshtml* hinzu, um das Löschen von Daten aus der Liste zu ermöglichen.

### Aufgabe 1: Hinzufügen von Code für das Löschformular

1. Wählen Sie die *Datei "Delete.cshtml* " im  **Explorer-Bereich** aus, um sie zur Bearbeitung zu öffnen.

1. Fügen Sie den folgenden Code vor dem Kommentar  hinzu:

    ```csharp
    <form method="post">
        @*  The id for the data record is hidden because it needs to be avaialable to the 
            code-behind processing, but it's not displayed. *@
        <input hidden asp-for="FruitModels.id" />
        <div class="border p-3 mt-4" style="width:50%">
            <div class="row pb-2">
                <h2 class="text-primary pl-3">Delete Fruit</h2>
                <hr />
            </div>
            <div class="mb-3">
                <label asp-for="FruitModels.name" class="h5"></label><br/>
                @* Render the name of the fruit in a non-editable text box. *@
                <input type="text" asp-for="FruitModels.name" disabled/>
                <span asp-validation-for="FruitModels.name" class="text-danger"></span>
            </div>
            <div class="mb-3">
                <label asp-for="FruitModels.instock" class="h5"></label><br/>
                @* Render the true/false instock state from the record in a non-editable checkbox. *@
                <input type="checkbox" asp-for="FruitModels.instock" style="width:20px; height:20px" disabled  />
                <span asp-validation-for="FruitModels.instock" class="text-danger"></span>
            </div>
            @* Submit the changes or return to the Index page if the delete is cancelled.*@
            <button type="submit" class="btn btn-danger " style="width:150px;">Delete</button>
            <a asp-page="Index" class="btn btn-secondary" style="width:150px;">Cancel</a>
        </div>
    </form>
    ```

1. Speichern Sie die Änderungen in *Delete.cshtml*, und überprüfen Sie die Kommentare im Code.

1. Drücken Sie in Visual Studio Code F5, oder wählen Sie im Menü Ausführen die Option Debuggen starten aus. Nach Abschluss des Projekts sollte das Erstellen eines Browserfensters mit der ausgeführten Web-App gestartet werden.

1. Wählen Sie ein Element in der Liste aus, das Gelöscht werden soll, und wählen Sie **"Löschen** " in dieser Zeile aus.

1. Wählen Sie **"Löschen"** aus, und Sie werden zurück zur Startseite weitergeleitet. Vergewissern Sie sich, dass das gelöschte Element nicht mehr in der Liste angezeigt wird.

Wenn Sie bereit sind, die Übung zu beenden:

* Schließen Sie die Registerkarte "Browser" oder "Browser", und wählen Sie **\| in Visual Studio Code "Debugging beenden"** oder **"UMSCHALT+F5**" aus. 

* Beenden Sie die Frucht-API, indem Sie in das Terminal gelangen  `Ctrl + C` , in dem es ausgeführt wird.

## Überprüfung

In dieser Übung lernen Sie Folgendes:

* Implementieren von Razor-Schlüsselwort (keyword) in einer App
* Integrieren von C#-Code in razor Pages-Syntax
