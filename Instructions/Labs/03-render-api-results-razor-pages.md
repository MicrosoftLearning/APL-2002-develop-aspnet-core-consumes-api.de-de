---
lab:
  title: 'Übung: Rendern von API-Antworten in ASP.NET Core Razor Pages'
  module: 'Module: Render API responses in ASP.NET Core Razor Pages'
---

In dieser Übung erfahren Sie, wie Sie einer ASP.NET Core Razor Pages-App Code hinzufügen, um Ergebnisse von HTTP-Vorgängen zu rendern. Dieser Code wird zu den *CSHTML*-Dateien hinzugefügt. Der Code, der die Vorgänge in den *CSHTML.CS*-Dateien ausführt, ist vollständig.

## Ziele

Nachdem Sie diese Übung abgeschlossen haben, können Sie Folgendes:

* Implementieren von Razor-Schlüsselwörtern in einer App
* Integrieren von C#-Code in Razor Pages-Syntax

## Voraussetzungen

Um die Übung durchzuführen, müssen die folgenden Komponenten auf Ihrem System installiert sein:

* [Visual Studio Code](https://code.visualstudio.com)
* [Das aktuelle .NET 7.0 SDK](https://dotnet.microsoft.com/download/dotnet/7.0)
* [Die C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) für Visual Studio Code

**Geschätzte Bearbeitungszeit dieser Übung:** 30 Minuten

## Übungsszenario

Diese Übung verfügt über zwei Komponenten:

* Eine Web-App, die HTTP-Anforderungen an eine API sendet. Die App wird auf `http://localhost:5010` ausgeführt
* Eine API, die auf HTTP-Anforderungen antwortet. Die API wird auf `http://localhost:5050` ausgeführt

![Dekorativ](media/02-architecture.png)

## Laden Sie den Code herunter.

In diesem Abschnitt laden Sie den Code für die Web-App Fruit und die Fruit-API herunter. Sie führen die Fruit-API auch lokal aus, damit sie für die Web-App verfügbar ist.

### Aufgabe 1: Herunterladen und Ausführen des API-Codes

1. Klicken Sie mit der rechten Maustaste auf den folgenden Link, und wählen Sie die Option **Link speichern unter** aus. 

    * Code [FruitAPI-Projektcode](https://raw.githubusercontent.com/MicrosoftLearning/APL-2002-develop-aspnet-core-consumes-api/master/Allfiles/Downloads/FruitAPI.zip)

1. Starten Sie **Datei-Explorer **, und navigieren Sie zu dem Speicherort, an dem die Datei gespeichert wurde.

1. Entpacken Sie die Datei in einen eigenen Ordner.

1. Öffnen Sie **Windows-Terminal** oder eine **Eingabeaufforderung**, und navigieren Sie zu dem Speicherort, an dem Sie den Code für die API extrahiert haben.

1. Führen Sie im Bereich **Windows-Terminal** folgenden `dotnet`-Befehl aus:

    ```
    dotnet run
    ```

1. Im Folgenden finden Sie ein Beispiel für die generierte Ausgabe. Beachten Sie die Zeile `Now listening on: http://localhost:5050` in der Ausgabe. Sie identifiziert den Host und Port für die API.

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

>**Hinweis:** Lassen Sie die Fruit-API während der restlichen Übung laufen. 

### Aufgabe 2: Herunterladen und Öffnen des Web-App-Projekts

1. Klicken Sie mit der rechten Maustaste auf den folgenden Link, und wählen Sie die Option **Link speichern unter** aus. 

    * [Fruit-Web-App-Renderprojektcode](https://raw.githubusercontent.com/MicrosoftLearning/APL-2002-develop-aspnet-core-consumes-api/master/Allfiles/Downloads/FruitWebApp-render.zip)

1. Starten Sie **Datei-Explorer **, und navigieren Sie zu dem Speicherort, an dem die Datei gespeichert wurde.

1. Entpacken Sie die Datei in einen eigenen Ordner.

1. Starten Sie Visual Studio Code, und wählen Sie in der Menüleiste **Datei** und dann **Ordner öffnen** aus.

1. Navigieren Sie zu dem Speicherort, an dem Sie die Projektdateien extrahiert haben, und wählen Sie den Ordner *FruitWebApp-render* aus.

1. Die Projektstruktur im Bereich **Explorer** sollte dem folgenden Screenshot ähneln. Sollte der Bereich **Explorer** nicht sichtbar sein, wählen Sie in der Menüleiste **Ansicht** und anschließend **Explorer** aus.

    ![Screenshot der Struktur des Web-App-Projekts Fruit.](media/03-web-app-render-structure.png)

>**Hinweis:** Nehmen Sie sich Zeit, um den Code in den einzelnen Dateien zu überprüfen, die während dieser Übung bearbeitet werden. Der Code enthält viele Kommentare, und dies kann Ihnen helfen, die Codebasis zu verstehen.

## Implementieren von Code zum Rendern von Daten auf der Indexseite

Die Web-App Fruit zeigt die API-Beispieldaten auf der Startseite an. Sie müssen Code hinzufügen, um die Beispieldaten zu durchlaufen, die von dem HTTP `GET`-Vorgang zurückgegeben werden, der in der CodeBehind-Datei ausgeführt wird.

### Aufgabe 1: Hinzufügen von Code zum Rendern von Daten in einer Tabelle

1. Wählen Sie die Datei *Index.cshtml* im Bereich **Explorer** aus, um sie zum Bearbeiten zu öffnen.

1. Fügen Sie den folgenden Code zwischen den Kommentaren `@* Begin render API data code block *@` und `@* End render API data code block *@` hinzu.

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

1. Speichern Sie die Änderungen an der Datei *Index.cshtml*, und überprüfen Sie die Kommentare im Code.

1. Wählen Sie im oberen Menü in Visual Studio Code **Ausführen \| Debuggen starten** aus, oder drücken Sie **F5**. Nachdem das Projekt fertiggestellt ist, sollte ein Browserfenster mit der ausgeführten Webanwendung geöffnet werden.

1. Überprüfen Sie, ob auf der Seite „Index“ die Beispieldaten aus der API angezeigt werden.

    >**Hinweis:** Die Funktionen **Zu Liste hinzufügen**, **Bearbeiten** und **Löschen** funktionieren erst, wenn Sie später in dieser Übung Code für sie hinzufügen.

    >**Hinweis:** Sie können die folgende Eingabeaufforderung problemlos ignorieren, wenn sie während der Ausführung der App angezeigt wird.

    ![Screenshot der Eingabeaufforderung zum Installieren eines selbstsignierten Zertifikats.](media/install-cert.png)

1. Wenn Sie mit der Übung fortfahren möchten, schließen Sie den Browser oder die Browserregisterkarte, und wählen Sie in Visual Studio Code **Ausführen \| Debuggen beenden** aus, oder drücken Sie **UMSCHALT+F5**.

## Implementieren von Code zur Behandlung der Funktion **Zu Liste hinzufügen**

Die Vorgänge zum Hinzufügen, Bearbeiten und Löschen werden jeweils auf einer separaten *CSHTML*-Seite im Projekt behandelt. In diesem Abschnitt fügen Sie Code zum Erstellen eines Formulars in der Datei *Add.cshtml* hinzu, um das Hinzufügen von Daten zur Liste zu ermöglichen.

### Aufgabe 1: Hinzufügen von Code zum Erstellen des Formulars für das Hinzufügen von Daten

1. Wählen Sie im Bereich **Explorer** die Datei *Add.cshtml* aus, um sie zum Bearbeiten zu öffnen.

1. Fügen Sie den folgenden Code zwischen den Kommentaren `@* Begin render Add code block *@` und `@* End render Add code block *@` hinzu.

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

1. Speichern Sie die Änderungen in *Add.cshtml*, und überprüfen Sie die Kommentare im Code.

1. Wählen Sie im oberen Menü in Visual Studio Code **Ausführen \| Debuggen starten** aus, oder drücken Sie **F5**. Nachdem das Projekt fertiggestellt ist, sollte ein Browserfenster mit der ausgeführten Webanwendung geöffnet werden.

1. Wählen Sie **Zu Liste hinzufügen** auf der Seite aus.

1. Geben Sie den Namen einer Frucht ein, die Sie zur Liste hinzufügen möchten, und aktivieren Sie das Kontrollkästchen, um anzugeben, dass sie verfügbar ist.

1. Wählen Sie **Erstellen** aus, um den Eintrag zur Liste hinzuzufügen. Anschließend werden Sie zurück zur Startseite geleitet. Überprüfen Sie, ob Ihr Eintrag zur Liste hinzugefügt wurde.

1. Wenn Sie mit der Übung fortfahren möchten, schließen Sie den Browser oder die Browserregisterkarte, und wählen Sie in Visual Studio Code **Ausführen \| Debuggen beenden** aus, oder drücken Sie **UMSCHALT+F5**.

## Implementieren von Code zum Behandeln der Funktion **Bearbeiten**

In diesem Abschnitt fügen Sie Code zum Erstellen eines Formulars in der Datei *Edit.cshtml* hinzu, um die Bearbeitung von Daten in der Liste zu ermöglichen.

### Aufgabe 1: Hinzufügen von Code für das Bearbeitungsformular

1. Wählen Sie die Datei *Edit.cshtml* im Bereich **Explorer** aus, um sie zur Bearbeitung zu öffnen.

1. Fügen Sie den folgenden Code zwischen den Kommentaren `@* Begin render Edit code block *@` und `@* End render Edit code block *@` hinzu.

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

1. Wählen Sie im oberen Menü in Visual Studio Code **Ausführen \| Debuggen starten** aus, oder drücken Sie **F5**. Nachdem das Projekt fertiggestellt ist, sollte ein Browserfenster mit der ausgeführten Webanwendung geöffnet werden.

1. Wählen Sie in der Liste ein Element aus, das geändert werden soll, und wählen Sie in der betreffenden Zeile **Bearbeiten** aus.

1. Bearbeiten Sie den Namen der Frucht, und aktivieren Sie das Kontrollkästchen, um ihren Verfügbarkeitsstatus zu ändern.

1. Wählen Sie **Aktualisieren** aus, um Ihre Änderungen zu speichern. Daraufhin werden Sie zurück zur Startseite geleitet. Überprüfen Sie, ob Ihre Änderung in der Liste angezeigt wird.

1. Wenn Sie mit der Übung fortfahren möchten, schließen Sie den Browser oder die Browserregisterkarte, und wählen Sie in Visual Studio Code **Ausführen \| Debuggen beenden** aus, oder drücken Sie **UMSCHALT+F5**.

## Implementieren von Code zum Behandeln der Funktion **Löschen**

In diesem Abschnitt fügen Sie Code zum Erstellen eines Formulars in der Datei *Delete.cshtml* hinzu, um das Löschen von Daten aus der Liste zu ermöglichen.

### Aufgabe 1: Hinzufügen von Code für das Löschformular

1. Wählen Sie die Datei *Delete.cshtml* im Bereich **Explorer** aus, um sie zur Bearbeitung zu öffnen.

1. Fügen Sie den folgenden Code zwischen den Kommentaren `@* Begin render Delete code block *@` und `@* End render Delete code block *@` hinzu.

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

1. Wählen Sie im oberen Menü in Visual Studio Code **Ausführen \| Debuggen starten** aus, oder drücken Sie **F5**. Nachdem das Projekt fertiggestellt ist, sollte ein Browserfenster mit der ausgeführten Webanwendung geöffnet werden.

1. Wählen Sie in der Liste ein Element aus, das gelöscht werden soll, und wählen Sie in der betreffenden Zeile **Löschen** aus.

1. Wählen Sie **Löschen** aus. Daraufhin werden Sie zurück zur Startseite geleitet. Vergewissern Sie sich, dass das gelöschte Element nicht mehr in der Liste angezeigt wird.

Wenn Sie bereit sind, die Übung zu beenden:

* Schließen Sie den Browser oder die Browserregisterkarte, und wählen Sie in Visual Studio Code **Ausführen \| Debuggen beenden** aus, oder drücken Sie **UMSCHALT+F5**. 

* Beenden Sie die Fruit-API, indem Sie in dem Terminal, in dem sie ausgeführt wird, **STRG+C** drücken.

## Überprüfung

In dieser Übung haben Sie Folgendes gelernt:

* Implementieren von Razor-Schlüsselwörtern in einer App
* Integrieren von C#-Code in Razor Pages-Syntax
