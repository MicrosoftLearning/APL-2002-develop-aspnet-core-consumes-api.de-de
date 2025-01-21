---
lab:
  title: 'Übung: Rendern von API-Antworten in ASP.NET Core Blazor-Web-Apps'
  module: 'Module: Render API responses in ASP.NET Core Blazor Web apps'
---

In dieser Übung erfahren Sie, wie Sie Code zu einer ASP.NET Core Blazor-Web-App hinzufügen, um Ergebnisse von HTTP-Vorgängen zu rendern. Dieser Code wird zu den *.razor*-Dateien hinzugefügt. Der Code, der die Vorgänge in den *.razor.cs*-Dateien ausführt, ist vollständig.

## Ziele

In dieser Übung lernen Sie Folgendes:

* Implementieren von Razor-Syntax in einer App
* Integrieren von C#-Code in Razor-Syntax

## Voraussetzungen

Um die Übung durchzuführen, müssen die folgenden Komponenten auf Ihrem System installiert sein:

* [Visual Studio Code](https://code.visualstudio.com)
* [Das aktuelle .NET 8.0 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)
* [Die C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) für Visual Studio Code

**Geschätzte Bearbeitungszeit dieser Übung:** 30 Minuten

## Übungsszenario

Diese Übung verfügt über zwei Komponenten:

* Eine Web-App, die HTTP-Anforderungen an eine API sendet. Die App wird auf `http://localhost:5010` ausgeführt
* Eine API, die auf HTTP-Anforderungen antwortet. Die API wird auf `http://localhost:5050` ausgeführt

![Dekorativ](media/02-architecture.png)

## Laden Sie den Code herunter.

In diesem Abschnitt laden Sie den Code für die Fruit-Web-App und die Fruit-API herunter. Sie führen die Fruit-API auch lokal aus, damit sie für die Web-App verfügbar ist.

### Aufgabe 1: Herunterladen und Ausführen des API-Codes

1. Klicken Sie mit der rechten Maustaste auf den folgenden Link, und wählen Sie **Link speichern unter** aus. 

    * [FruitAPI-Projektcode](https://raw.githubusercontent.com/MicrosoftLearning/APL-2002-develop-aspnet-core-consumes-api/master/Allfiles/Downloads/FruitAPI.zip)

1. Starten Sie **Explorer**, und navigieren Sie zum Speicherort der Datei.

1. Entzippen Sie die Datei in ihren eigenen Ordner.

1. Öffnen Sie **Windows-Terminal** oder eine **Eingabeaufforderung**, und navigieren Sie zu dem Speicherort, an dem Sie den Code für die API extrahiert haben.

1. Führen Sie im Bereich **Windows-Terminal** folgenden `dotnet`-Befehl aus:

    ```
    dotnet run
    ```

1. Im Folgenden finden Sie ein Beispiel für die generierte Ausgabe. Beachten Sie die Zeile `Now listening on: http://localhost:5050` in der Ausgabe. Sie identifiziert den Host und den Port für die API.

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

1. Entzippen Sie die Datei in ihren eigenen Ordner.

1. Starten Sie Visual Studio Code, und wählen Sie in der Menüleiste **Datei** und dann **Ordner öffnen** aus.

1. Navigieren Sie zu dem Speicherort, an dem Sie die Projektdateien extrahiert haben, und wählen Sie den Ordner *FruitWebApp-render* aus.

1. Die Projektstruktur im Bereich **Explorer** sollte dem folgenden Screenshot ähneln. Sollte der Bereich **Explorer** nicht sichtbar sein, wählen Sie in der Menüleiste **Ansicht** und anschließend **Explorer** aus.

    ![Screenshot der Projektstruktur der Fruit-Web-App.](media/03-web-app-render-structure.png)

>**Hinweis:** Nehmen Sie sich Zeit, um den Code in den einzelnen Dateien zu überprüfen, die während dieser Übung bearbeitet werden. Der Code ist stark kommentiert und kann Ihnen dabei helfen, die Codebasis zu verstehen.

## Implementieren von Code zum Rendern von Daten auf der Startseite

Die Web-App Fruit zeigt die API-Beispieldaten auf der Startseite an. Sie müssen Code hinzufügen, um die Beispieldaten zu durchlaufen, die von dem HTTP `GET`-Vorgang zurückgegeben werden, der in der CodeBehind-Datei ausgeführt wird.

### Aufgabe 1: Hinzufügen von Code zum Rendern von Daten in einer Tabelle

1. Wählen Sie im Bereich **Explorer** die Datei *Home.razor* aus, um sie zur Bearbeitung zu öffnen.

1. Fügen Sie den folgenden Code zwischen den Kommentaren `@* Begin render API data code block *@` und `@* End render API data code block *@` hinzu.

    ```razor
    <tbody>
        @*  The Razor explicit expression @foreach is used to iterate through the
            data returned to the data model from the HTTP operations. *@
        @foreach (var obj in _fruitList ?? [])
        {
            <tr>
                @* Display the name of the fruit. *@
                <td width="50%">@obj.name</td>
                @*  The following if statment changes the true/false of instock to Yes/No. *@
                @{
                    if (@obj.instock)
                    {
                        <td width="20%" class="text-md-center">
                            Yes
                        </td>
                    }
                    else
                    {
                        <td width="20%" class="text-md-center">
                            No
                        </td>
                    }
                }
                <td width="30%" class="text-center">
                    @* The following div renders the Edit and Delete buttons that pass the Id 
                        to a function that handles the navigation and passes the Id to the page. *@
                    <div class="w-75 btn-group btn-group-sm" role="group" style="text-align:center">
                        <button class="btn btn-primary  mx-2" @onclick="() => EditButton(obj.id)">
                            Edit
                        </button>
                        <button class="btn btn-danger mx-2" @onclick="() => DeleteButton(obj.id)">
                            Delete
                        </button>
                    </div>
                </td>
            </tr>
        }
    </tbody>
    ```

1. Speichern Sie die Änderungen in *Home.razor* und überprüfen Sie die Kommentare im Code.

1. Wählen Sie im oberen Menü in Visual Studio Code **Ausführen \| Debuggen starten** aus, oder drücken Sie **F5**. Nachdem das Projekt fertiggestellt ist, sollte ein Browserfenster mit der ausgeführten Webanwendung geöffnet werden.

1. Überprüfen Sie, ob auf der Seite „Index“ die Beispieldaten aus der API angezeigt werden.

    >**Hinweis:** Die Funktionen **Zu Liste hinzufügen**, **Bearbeiten** und **Löschen** funktionieren erst, wenn Sie später in dieser Übung Code für sie hinzufügen.

    >**Hinweis:** Sie können die folgende Eingabeaufforderung problemlos ignorieren, wenn sie während der Ausführung der App angezeigt wird.

    ![Screenshot der Eingabeaufforderung zum Installieren eines selbstsignierten Zertifikats.](media/install-cert.png)

1. Wenn Sie mit der Übung fortfahren möchten, schließen Sie den Browser oder die Browserregisterkarte, und wählen Sie in Visual Studio Code **Ausführen \| Debuggen beenden** aus, oder drücken Sie **UMSCHALT+F5**.

## Implementieren von Code zur Behandlung der Funktion **Zu Liste hinzufügen**

Die Vorgänge zum Hinzufügen, Bearbeiten und Löschen werden jeweils auf einer separaten *.razor*-Seite im Projekt behandelt. In diesem Abschnitt fügen Sie Code zum Erstellen eines Formulars in der Datei *Add.razor* hinzu, um das Hinzufügen von Daten zur Liste zu ermöglichen.

### Aufgabe 1: Hinzufügen von Code zum Erstellen des Formulars für das Hinzufügen von Daten

1. Wählen Sie im Bereich **Explorer** die Datei *Add.razor* aus, um sie zum Bearbeiten zu öffnen.

1. Fügen Sie den folgenden Code zwischen den Kommentaren `@* Begin render Add code block *@` und `@* End render Add code block *@` hinzu.

    ```csharp
    @* Data is added using a Razor form, the data model is bound to the form.*@
    <EditForm OnSubmit="Submit" FormName="AddFruit" Model="_fruitList">
        @*  The _fruitList.id is here so the full data model is represented on the page.
            The database behind the API will assign the id. *@
        <InputNumber hidden="true" @bind-Value="_fruitList!.id" />
        <div class="border p-3 mt-4" style="width:50%">
            <div class="row pb-2">
                <h2 class="text-primary pl-3">Add Fruit</h2>
                <hr />
            </div>
            <div class="mb-3">
                <label class="h5"></label><br />
                @* Empty text box for the name of the fruit to be added. *@
                <InputText @bind-Value="_fruitList!.name" />
            </div>
            <div class="mb-3">
                <label class="h5"></label><br />
                @* Render the true/false instock state from the record in an editable checkbox. *@
                <InputCheckbox @bind-Value="_fruitList!.instock" style="width:20px; height:20px" />
                <label class="h7">Check the box if it's available.</label>
            </div>
            @* Submit the addition or return to the Index page if the Add is cancelled.*@
            <button @onclick="() => Submit()" class="btn btn-primary" style="width:150px;">Create</button>
            <a class="btn btn-secondary" style="width:150px;" href="/">Cancel</a>
        </div>
    </EditForm>
    ```
    
1. Speichern Sie die Änderungen in *Add.razor* und überprüfen Sie die Kommentare im Code.

1. Wählen Sie im oberen Menü in Visual Studio Code **Ausführen \| Debuggen starten** aus, oder drücken Sie **F5**. Nachdem das Projekt fertiggestellt ist, sollte ein Browserfenster mit der ausgeführten Webanwendung geöffnet werden.

1. Wählen Sie **Zu Liste hinzufügen** auf der Seite aus.

1. Geben Sie den Namen einer Frucht ein, die Sie zur Liste hinzufügen möchten, und aktivieren Sie das Kontrollkästchen, um anzugeben, dass sie verfügbar ist.

1. Wählen Sie **Erstellen** aus, um den Eintrag zur Liste hinzuzufügen. Anschließend werden Sie zurück zur Startseite geleitet. Überprüfen Sie, ob Ihr Eintrag zur Liste hinzugefügt wurde.

1. Wenn Sie mit der Übung fortfahren möchten, schließen Sie den Browser oder die Browserregisterkarte, und wählen Sie in Visual Studio Code **Ausführen \| Debuggen beenden** aus, oder drücken Sie **UMSCHALT+F5**.

## Implementieren von Code zum Behandeln der Funktion **Bearbeiten**

In diesem Abschnitt fügen Sie Code zum Erstellen eines Formulars in der Datei *Edit.cshtml* hinzu, um die Bearbeitung von Daten in der Liste zu ermöglichen.

### Aufgabe 1: Hinzufügen von Code für das Bearbeitungsformular

1. Wählen Sie die Datei *Edit.razor* im Bereich **Explorer** aus, um sie zur Bearbeitung zu öffnen.

1. Fügen Sie den folgenden Code zwischen den Kommentaren `@* Begin render Edit code block *@` und `@* End render Edit code block *@` hinzu.

    ```csharp
    @* Data is edited using a Razor form, the data model is bound to the form.*@
    <EditForm OnSubmit="Submit" FormName="EditFruit" Model="_fruitList">
        @*  The id for the data record is hidden because it needs to be available to the 
            code-behind processing, but it's not displayed. *@
        <InputNumber hidden="true" @bind-Value="_fruitList!.id" />
        <div class="border p-3 mt-4" style="width:50%">
            <div class="row pb-2">
                <h2 class="text-primary pl-3">Edit Fruit</h2>
                <hr />
            </div>
            <div class="mb-3">
                <label asp-for="FruitModels.name" class="h5"></label><br/>
                @* Render the name of the fruit in an editable text box. *@
                <InputText @bind-Value="_fruitList!.name" />
            </div>
            <div class="mb-3">
                <label  class="h5"></label><br/>
                @* Render the true/false instock state from the record in an editable checkbox. *@
                <InputCheckbox @bind-Value="_fruitList!.instock" style="width:20px; height:20px" />
                <label class="h7"><i class="bi bi-arrow-left"></i>  Check the box if available.</label>
            </div>
            @* Submit the changes or return to the Index page if the Edit is cancelled.*@
            <button type="submit" class="btn btn-danger " style="width:150px;">Save</button>
            <a class="btn btn-secondary" style="width:150px;" href="/">Cancel</a>
        </div>
    </EditForm>
    ```

1. Speichern Sie die Änderungen in *Edit.razor* und überprüfen Sie die Kommentare im Code.

1. Wählen Sie im oberen Menü in Visual Studio Code **Ausführen \| Debuggen starten** aus, oder drücken Sie **F5**. Nachdem das Projekt fertiggestellt ist, sollte ein Browserfenster mit der ausgeführten Webanwendung geöffnet werden.

1. Wählen Sie in der Liste ein Element aus, das geändert werden soll, und wählen Sie in der betreffenden Zeile **Bearbeiten** aus.

1. Bearbeiten Sie den Namen der Frucht, und aktivieren Sie das Kontrollkästchen, um ihren Verfügbarkeitsstatus zu ändern.

1. Wählen Sie **Aktualisieren** aus, um Ihre Änderungen zu speichern. Daraufhin werden Sie zurück zur Startseite geleitet. Überprüfen Sie, ob Ihre Änderung in der Liste angezeigt wird.

1. Wenn Sie mit der Übung fortfahren möchten, schließen Sie den Browser oder die Browserregisterkarte, und wählen Sie in Visual Studio Code **Ausführen \| Debuggen beenden** aus, oder drücken Sie **UMSCHALT+F5**.

## Implementieren von Code zum Behandeln der Funktion **Löschen**

In diesem Abschnitt fügen Sie Code zum Erstellen eines Formulars in der Datei *Delete.cshtml* hinzu, um das Löschen von Daten aus der Liste zu ermöglichen.

### Aufgabe 1: Hinzufügen von Code für das Löschformular

1. Wählen Sie die Datei *Delete.razor* im Bereich **Explorer** aus, um sie zur Bearbeitung zu öffnen.

1. Fügen Sie den folgenden Code zwischen den Kommentaren `@* Begin render Delete code block *@` und `@* End render Delete code block *@` hinzu.

    ```csharp
    @* Data is deleted using a Razor form, the data model is bound to the form.*@
    <EditForm OnSubmit="Submit" FormName="DeleteFruit" Model="_fruitList">
        @*  The id for the data record is hidden because it needs to be available to the 
            code-behind processing, but it's not displayed. *@
        <InputNumber hidden="true" @bind-Value="_fruitList!.id" />
        <div class="border p-3 mt-4" style="width:50%">
            <div class="row pb-2">
                <h2 class="text-primary pl-3">Delete Fruit</h2>
                <hr />
            </div>
            <div class="mb-3">
                <label asp-for="FruitModels.name" class="h5"></label><br/>
                @* Render the name of the fruit in a non-editable text box. *@
                <InputText @bind-Value="_fruitList!.name" Disabled/>
            </div>
            <div class="mb-3">
                <label  class="h5"></label><br/>
                @* Render the true/false instock state from the record in a non-editable checkbox. *@
                <InputCheckbox @bind-Value="_fruitList!.instock" style="width:20px; height:20px" Disabled  />
                <label class="h7">Check the box if available.</label>
            </div>
            @* Submit the changes or return to the Index page if the delete is cancelled.*@
            <button type="submit" class="btn btn-danger " style="width:150px;">Delete</button>
            <a class="btn btn-secondary" style="width:150px;" href="/">Cancel</a>
        </div>
    </EditForm>
    ```

1. Speichern Sie die Änderungen in *Delete.razor* und überprüfen Sie die Kommentare im Code.

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
