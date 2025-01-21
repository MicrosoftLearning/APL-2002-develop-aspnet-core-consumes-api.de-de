---
lab:
  title: 'Übung: Implementieren von HTTP-Vorgängen in ASP.NET Core Blazor Web-Apps'
  module: 'Module: Implement HTTP operations in ASP.NET Core Blazor Web apps'
---

In dieser Übung erfahren Sie, wie Sie einer ASP.NET Core Blazor Web-App Code hinzufügen, um den HTTP-Client zu erstellen und `GET`-, `POST`-, `PUT`- und `DELETE`-Vorgänge auszuführen. Dieser Code wird den *.razor.cs*-CodeBehind-Dateien hinzugefügt. Der Code zum Rendern der Daten in den *.razor*-Dateien ist fertig.

## Ziele

In dieser Übung lernen Sie Folgendes:

* Implementieren von `IHttpClientFactory` als HTTP-Client
* Implementieren von HTTP-Vorgängen in ASP.NET Blazor-Web-Apps

## Voraussetzungen

Um die Übung durchzuführen, müssen die folgenden Komponenten auf Ihrem System installiert sein:

* [Visual Studio Code](https://code.visualstudio.com)
* [Das aktuelle .NET 8.0 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)
* [Die C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) für Visual Studio Code

**Geschätzte Bearbeitungszeit dieser Übung:** 30 Minuten

## Übungsszenario

Diese Übung verfügt über zwei Komponenten:

* Eine App, die HTTP-Anforderungen an eine API sendet. Die Web-App wird auf `http://localhost:5010` ausgeführt.
* Eine API, die auf HTTP-Anforderungen antwortet. Die API wird auf `http://localhost:5050` ausgeführt.

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

1. Klicken Sie mit der rechten Maustaste auf den folgenden Link, und wählen Sie **Link speichern unter** aus. 

    * [CodeBehind-Projektcode für die Fruit-Web-App](https://raw.githubusercontent.com/MicrosoftLearning/APL-2002-develop-aspnet-core-consumes-api/master/Allfiles/Downloads/FruitWebApp-codebehind.zip)

1. Starten Sie **Explorer**, und navigieren Sie zum Speicherort der Datei.

1. Entzippen Sie die Datei in ihren eigenen Ordner.

1. Starten Sie Visual Studio Code, und wählen Sie in der Menüleiste **Datei** und dann **Ordner öffnen** aus.

1. Navigieren Sie zu dem Speicherort, an dem Sie die Projektdateien entzippt haben, und wählen Sie den Ordner *FruitWebApp-codebehind* aus.

1. Die Projektstruktur im Bereich **Explorer** sollte dem folgenden Screenshot ähneln. Sollte der Bereich **Explorer** nicht sichtbar sein, wählen Sie in der Menüleiste **Ansicht** und anschließend **Explorer** aus.

    ![Screenshot der Projektstruktur der Fruit-Web-App.](media/02-web-app-cb-struture.png)

>**Hinweis:** Nehmen Sie sich Zeit, um den Code in den einzelnen Dateien zu überprüfen, die während dieser Übung bearbeitet werden. Der Code ist stark kommentiert und kann Ihnen dabei helfen, die Codebasis zu verstehen.

## Implementieren von Code für den HTTP-Client und HTTP-Vorgänge

Die Web-App „Fruit“ zeigt die API-Beispieldaten auf der Startseite an und verfügt über Funktionen zum Hinzufügen, Bearbeiten und Löschen. Sie müssen Code hinzufügen, um die HTTP-Clientvorgänge zu implementieren. 

### Aufgabe 1: Implementieren des HTTP-Clients

1. Wählen Sie im Bereich **Explorer** die Datei *Program.cs* aus, um sie zur Bearbeitung zu öffnen.

1. Fügen Sie den folgenden Code zwischen den Kommentaren `// Begin HTTP client code` und `// End of HTTP client code` hinzu.

    ```csharp
    // Add IHttpClientFactory to the container and set the name of the factory
    // to "FruitAPI". The base address for API requests is also set.
    builder.Services.AddHttpClient("FruitAPI", httpClient =>
    {
        httpClient.BaseAddress = new Uri("http://localhost:5050/");
    });
    ```

1. Speichern Sie die Änderungen an *Program.cs*.

### Aufgabe 2: Implementieren des GET-Vorgangs

1. Wählen Sie im Bereich **Explorer** die Datei *home.razor.cs* aus, um sie zur Bearbeitung zu öffnen. Es befindet sich im `Components/Pages`-Ordner.

1. Fügen Sie den folgenden Code zwischen den Kommentaren `// Begin GET operation code` und `// End GET operation code` hinzu.

    ```csharp
    protected override async Task OnInitializedAsync()
    {
        // Create the HTTP client using the FruitAPI named factory
        var httpClient = HttpClientFactory.CreateClient("FruitAPI");

        // Perform the GET request and store the response. The parameter
        // in GetAsync specifies the endpoint in the API 
        using HttpResponseMessage response = await httpClient.GetAsync("/fruits");

        // If the request is successful deserialize the results into the data model
        if (response.IsSuccessStatusCode)
        {
            using var contentStream = await response.Content.ReadAsStreamAsync();
            _fruitList = await JsonSerializer.DeserializeAsync<IEnumerable<FruitModel>>(contentStream);
        }
        else
        {
            // If the request is unsuccessful, log the error message
            Console.WriteLine($"Failed to load fruit list. Status code: {response.StatusCode}");
        }
    }
    ```

1. Speichern Sie die Änderungen an *Home.razor.cs*.

1. Überprüfen Sie den Code in der Datei *Home.razor.cs*. Beachten Sie, wo `IHttpClientFactory` zur Seite mit Abhängigkeitsinjektion hinzugefügt wird.

### Aufgabe 3: Implementieren des POST-Vorgangs

1. Wählen Sie im Bereich **Explorer** die Datei *Add.razor.cs* aus, um sie zum Bearbeiten zu öffnen.

1. Fügen Sie den folgenden Code zwischen den Kommentaren `// Begin POST operation code` und `// End POST operation code` hinzu.

    ```csharp
    private async Task Submit()
    {
        // Serialize the information to be added to the database
        var jsonContent = new StringContent(JsonSerializer.Serialize(_fruitList),
            Encoding.UTF8,
            "application/json");

        // Create the HTTP client using the FruitAPI named factory
        var httpClient = HttpClientFactory.CreateClient("FruitAPI");

        // Execute the POST request and store the response. The response will contain the new record's ID
        using HttpResponseMessage response = await httpClient.PostAsync("/fruits", jsonContent);

        // Check if the operation was successful, and navigate to the home page if it was
        if (response.IsSuccessStatusCode)
        {
            NavigationManager?.NavigateTo("/");
        }
        else
        {
            Console.WriteLine("Failed to add fruit. Status code: {response.StatusCode}");
        }
    }
    ```

1. Speichern Sie die Änderungen in *Add.razor.cs* und überprüfen Sie die Kommentare im Code.

### Aufgabe 4: Implementieren des PUT-Vorgangs

1. Wählen Sie die Datei *Edit.razor.cs* im Bereich **Explorer** aus, um sie zur Bearbeitung zu öffnen.

1. Fügen Sie den folgenden Code zwischen den Kommentaren `// Begin PUT operation code` und `// End PUT operation code` hinzu.

    ```csharp
    private async Task Submit()
    {
        // Create the HTTP client using the FruitAPI named factory
        var httpClient = HttpClientFactory.CreateClient("FruitAPI");

        // Store the updated data in a JSON object
        var jsonContent = new StringContent(JsonSerializer.Serialize(_fruitList), 
            Encoding.UTF8, "application/json");

        // Execute the PUT request
        using HttpResponseMessage response = await httpClient.PutAsync($"/fruits/{Id}", jsonContent);

        // If the response is successful, navigate back to the home page 
        if (response.IsSuccessStatusCode)
        {
            NavigationManager?.NavigateTo("/");
        }
        else
        {
            Console.WriteLine("Failed to update fruit with edits. Status code: {response.StatusCode}");
        }
    }
    ```

1. Speichern Sie die Änderungen in *Edit.razor.cs* und überprüfen Sie die Kommentare im Code.

### Aufgabe 5: Implementieren des DELETE-Vorgangs

1. Wählen Sie die Datei *Delete.razor.cs* im Bereich **Explorer** aus, um sie zur Bearbeitung zu öffnen.

1. Fügen Sie den folgenden Code zwischen den Kommentaren `// Begin DELETE operation code` und `// End DELETE operation code` hinzu.

    ```csharp
    private async Task Submit()
    {
        // Create the HTTP client using the FruitAPI named factory
        var httpClient = HttpClientFactory.CreateClient("FruitAPI");

        // Execute the DELETE request and store the response
        using HttpResponseMessage response = await httpClient.DeleteAsync("/fruits/" + Id.ToString());

        // Return to the home page 
        if (response.IsSuccessStatusCode)
        {
            NavigationManager?.NavigateTo("/");
        }
        else
        {
            Console.WriteLine("Failed to delete fruit. Status code: {response.StatusCode}");
        }
    }
    ```

1. Speichern Sie die Änderungen in *Delete.razor.cs* und überprüfen Sie die Kommentare im Code.

## Ausführen und Testen der Web-App

### Aufgabe 1: Ausführen der Web-App

1. Wählen Sie im oberen Menü in Visual Studio Code **Ausführen \| Debuggen starten** aus, oder drücken Sie **F5**. Nachdem das Projekt das Erstellen eines Browserfensters abgeschlossen hat, sollte die ausgeführte Web-App gestartet werden und die API-Beispieldaten sollten angezeigt werden, wie im folgenden Screenshot dargestellt.

    ![Screenshot der Web-App, in der die Beispieldaten angezeigt werden.](media/02-web-app-get-sample-data.png)

    >**Hinweis:** Sie können die folgende Eingabeaufforderung problemlos ignorieren, wenn sie bei der Ausführung der App angezeigt wird.

    ![Screenshot der Eingabeaufforderung zum Installieren eines selbstsignierten Zertifikats.](media/install-cert.png)

### Aufgabe 1: Testen der Web-App

1. Wählen Sie die Schaltfläche **Zu Liste hinzufügen** aus, und füllen Sie das generierte Formular aus. Wählen Sie dann die Schaltfläche **Erstellen** aus.

1. Vergewissern Sie sich, dass Ihre Hinzufügung am Ende der Liste angezeigt wird.

1. Wählen Sie in der Liste ein Element aus, das bearbeitet werden soll, und wählen Sie die Schaltfläche **Bearbeiten** aus. 
1. Bearbeiten Sie den **Namen der Frucht** und das Feld **Verfügbar?**, und wählen Sie dann **Aktualisieren** aus.

1. Überprüfen Sie, ob Ihre Änderungen in der Liste angezeigt werden. 

1. Wählen Sie in der Liste ein Element aus, das gelöscht werden soll, und wählen Sie die Schaltfläche **Löschen** aus.

1. Überprüfen Sie auf der Seite „Löschen“, ob das ausgewählte Element angezeigt wird, und klicken Sie auf die Schaltfläche **Löschen**.

1. Vergewissern Sie sich, dass das Element nicht mehr in der Liste angezeigt wird.

Wenn Sie bereit sind, die Übung zu beenden:

* Schließen Sie den Browser oder die Browserregisterkarte, und wählen Sie in Visual Studio Code **Ausführen \| Debuggen beenden** aus, oder drücken Sie **UMSCHALT+F5**. 

* Beenden Sie die Fruit-API, indem Sie in dem Terminal, in dem sie ausgeführt wird, **STRG+C** drücken.

## Überprüfung

In dieser Übung haben Sie Folgendes gelernt:

* Implementieren von `IHttpClientFactory` als HTTP-Client
* Implementieren von HTTP-Vorgängen in CodeBehind-Dateien von ASP.NET Core Blazor
