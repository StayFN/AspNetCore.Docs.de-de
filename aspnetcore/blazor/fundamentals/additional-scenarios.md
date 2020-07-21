---
title: Hostingmodellkonfiguration für ASP.NET Core Blazor
author: guardrex
description: In diesem Artikel erhalten Sie Informationen zu zusätzlichen Szenarios für die Blazor-Hostingmodellkonfiguration in ASP.NET Core.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/fundamentals/additional-scenarios
ms.openlocfilehash: b28e4e43b88fcf8eab9e8959142cca21223c57ff
ms.sourcegitcommit: e216e8f4afa21215dc38124c28d5ee19f5ed7b1e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2020
ms.locfileid: "86239633"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a>Hostingmodellkonfiguration für ASP.NET Core Blazor

Von [Daniel Roth](https://github.com/danroth27) und [Luke Latham](https://github.com/guardrex)

Dieser Artikel behandelt die Hostingmodellkonfiguration.

### <a name="signalr-cross-origin-negotiation-for-authentication"></a>Ursprungsübergreifende SignalR-Aushandlung für die Authentifizierung

*Dieser Abschnitt gilt für Blazor WebAssembly.*

So konfigurieren Sie den zugrunde liegenden SignalR-Client zum Senden von Anmeldeinformationen (z. B. Cookies oder HTTP-Authentifizierungsheader):

* Verwenden Sie <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserRequestCredentials%2A>, um <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.BrowserRequestCredentials.Include> auf ursprungsübergreifende [`fetch`](https://developer.mozilla.org/docs/Web/API/Fetch_API/Using_Fetch)-Anforderungen festzulegen:

  ```csharp
  public class IncludeRequestCredentialsMessageHandler : DelegatingHandler
  {
      protected override Task<HttpResponseMessage> SendAsync(
          HttpRequestMessage request, CancellationToken cancellationToken)
      {
          request.SetBrowserRequestCredentials(BrowserRequestCredentials.Include);
          return base.SendAsync(request, cancellationToken);
      }
  }
  ```

* Weisen Sie der <xref:Microsoft.AspNetCore.Http.Connections.Client.HttpConnectionOptions.HttpMessageHandlerFactory>-Eigenschaft die <xref:System.Net.Http.HttpMessageHandler>-Klasse hinzu:

  ```csharp
  var connection = new HubConnectionBuilder()
      .WithUrl(new Uri("http://signalr.example.com"), options =>
      {
          options.HttpMessageHandlerFactory = innerHandler => 
              new IncludeRequestCredentialsMessageHandler { InnerHandler = innerHandler };
      }).Build();
  ```

Weitere Informationen finden Sie unter <xref:signalr/configuration#configure-additional-options>.

## <a name="reflect-the-connection-state-in-the-ui"></a>Reflektieren des Verbindungszustands auf der Benutzeroberfläche

*Dieser Abschnitt gilt für Blazor Server.*

Wenn der Client erkennt, dass keine Verbindung mehr besteht, wird dem Benutzer eine Standardbenutzeroberfläche angezeigt, während der Client versucht, eine neue Verbindung herzustellen. Wenn die Wiederherstellung der Verbindung fehlschlägt, wird dem Benutzer die Option angezeigt, es noch mal zu versuchen.

Wenn Sie die Benutzeroberfläche anpassen möchten, definieren Sie ein Element mit einer `id` von `components-reconnect-modal` im `<body>` auf der Razor-Seite von `_Host.cshtml`:

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

Fügen Sie dem Stylesheet (`wwwroot/css/app.css` oder `wwwroot/css/site.css`) der App Folgendes hinzu:

```css
#components-reconnect-modal {
    display: none;
}

#components-reconnect-modal.components-reconnect-show {
    display: block;
}
```

In der folgenden Tabelle werden die CSS-Klassen beschrieben, die auf das `components-reconnect-modal`-Element angewendet werden.

| CSS-Klasse                       | Steht für&hellip; |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | Die Verbindung wurde getrennt. Der Client versucht, die Verbindung wiederherzustellen. Die modale Seite wird angezeigt. |
| `components-reconnect-hide`     | Auf dem Server wird eine aktive Verbindung wiederhergestellt. Die modale Seite wird ausgeblendet. |
| `components-reconnect-failed`   | Die Wiederherstellung der Verbindung ist wahrscheinlich aufgrund eines Netzwerkfehlers fehlgeschlagen. Rufen Sie `window.Blazor.reconnect()` auf, um einen neuen Verbindungsversuch zu unternehmen. |
| `components-reconnect-rejected` | Die Wiederherstellung der Verbindung wurde abgelehnt. Der Server wurde zwar erreicht, jedoch hat dieser die Verbindung verweigert. Der Status des Benutzers auf dem Server wurde verworfen. Rufen Sie `location.reload()` auf, um die App neu zu laden. Dieser Verbindungszustand kann aus folgenden Gründen auftreten:<ul><li>Aufgetretener Absturz auf der serverseitigen Verbindung.</li><li>Der Client war lange nicht verbunden, sodass der Server den Benutzerstatus verworfen hat. Komponenteninstanzen, mit denen der Benutzer interagiert, werden verworfen.</li><li>Der Server wird neu gestartet, oder der Workerprozess der App wird wiederverwendet.</li></ul> |

## <a name="render-mode"></a>Rendermodus

*Dieser Abschnitt gilt für Blazor Server.*

Blazor Server-Apps werden standardmäßig eingerichtet, um die Benutzeroberfläche auf dem Server schon vor der Einrichtung der Clientverbindung auf dem Server zu rendern. Diese Einrichtung erfolgt auf der Razor-Seite von `_Host.cshtml`:

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper.RenderMode> konfiguriert folgende Einstellungen für die Komponente:

* Ob die Komponente zuvor für die Seite gerendert wird
* Ob die Komponente als statische HTML auf der Seite gerendert wird oder ob sie die nötigen Informationen für das Bootstrapping einer Blazor-App über den Benutzer-Agent enthält.

| Rendermodus | Beschreibung |
| --- | --- |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | Rendert die Komponente in statisches HTML und fügt einen Marker für eine Blazor Server-App hinzu. Wenn der Benutzer-Agent gestartet wird, wird der Marker zum Bootstrapping einer Blazor-App verwendet. |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | Rendert einen Marker für eine Blazor Server-App. Die Ausgabe der Komponente ist nicht enthalten. Wenn der Benutzer-Agent gestartet wird, wird der Marker zum Bootstrapping einer Blazor-App verwendet. |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | Rendert die Komponente in statischen HTML-Code. |

Das Rendern von Serverkomponenten über eine statische HTML-Seite wird nicht unterstützt.

## <a name="configure-the-signalr-client-for-blazor-server-apps"></a>Konfigurieren Sie den SignalR-Client für Blazor Server-Apps.

*Dieser Abschnitt gilt für Blazor Server.*

Konfigurieren Sie den SignalR-Client, der von Blazor Server-Apps verwendet wird, in der Datei `Pages/_Host.cshtml`. Platzieren Sie ein Skript, das `Blazor.start` aufruft, nach dem Skript `_framework/blazor.server.js` und innerhalb des Tags `</body>`.

### <a name="logging"></a>Protokollierung

So konfigurieren Sie die SignalR-Clientprotokollierung:

* Fügen Sie ein `autostart="false"`-Attribut zum `<script>`-Tag für das `blazor.server.js`-Skript hinzu.
* Übergeben Sie ein Konfigurationsobjekt (`configureSignalR`), das `configureLogging` mit der Protokollebene im Clientgenerator aufruft.

```cshtml
    ...

    <script src="_framework/blazor.server.js" autostart="false"></script>
    <script>
      Blazor.start({
        configureSignalR: function (builder) {
          builder.configureLogging("information");
        }
      });
    </script>
</body>
```

Im vorherigen Beispiel entspricht `information` der Protokollebene <xref:Microsoft.Extensions.Logging.LogLevel.Information?displayProperty=nameWithType>.

### <a name="modify-the-reconnection-handler"></a>Ändern des Handlers für die Wiederherstellung einer Verbindung

Die Verbindungsereignisse des Handlers für die Wiederherstellung einer Verbindung können geändert werden, um benutzerdefinierte Verhaltensweisen zu erzeugen, z. B. für Folgendes:

* Benachrichtigung an einen Benutzer, wenn die Verbindung unterbrochen wird
* Ausführen der Protokollierung (vom Client), wenn eine Verbindung besteht

So ändern Sie die Verbindungsereignisse:

* Fügen Sie ein `autostart="false"`-Attribut zum `<script>`-Tag für das `blazor.server.js`-Skript hinzu.
* Registrieren Sie Rückrufe für Verbindungsänderungen für unterbrochene Verbindungen (`onConnectionDown`) und hergestellte bzw. wiederhergestellte Verbindungen (`onConnectionUp`). `onConnectionDown` und `onConnectionUp` sind **obligatorisch**.

```cshtml
    ...

    <script src="_framework/blazor.server.js" autostart="false"></script>
    <script>
      Blazor.start({
        reconnectionHandler: {
          onConnectionDown: (options, error) => console.error(error);
          onConnectionUp: () => console.log("Up, up, and away!");
        }
      });
    </script>
</body>
```

### <a name="adjust-the-reconnection-retry-count-and-interval"></a>Passen Sie die Anzahl und das Intervall der Wiederholungsversuche zum erneuten Herstellen einer Verbindung an.

So passen Sie die Anzahl und das Intervall für die Wiederholungsversuche an:

* Fügen Sie ein `autostart="false"`-Attribut zum `<script>`-Tag für das `blazor.server.js`-Skript hinzu.
* Legen Sie die zulässige Anzahl von Wiederholungsversuchen (`maxRetries`) und den zulässigen Zeitraum in Millisekunden (`retryIntervalMilliseconds`) für jeden Versuch fest.

```cshtml
    ...

    <script src="_framework/blazor.server.js" autostart="false"></script>
    <script>
      Blazor.start({
        reconnectionOptions: {
          maxRetries: 3,
          retryIntervalMilliseconds: 2000
        }
      });
    </script>
</body>
```

### <a name="hide-or-replace-the-reconnection-display"></a>Ausblenden oder Ersetzen der Anzeige einer erneuten Verbindung

So blenden Sie die Anzeige einer erneuten Verbindung aus:

* Fügen Sie ein `autostart="false"`-Attribut zum `<script>`-Tag für das `blazor.server.js`-Skript hinzu.
* Legen Sie das `_reconnectionDisplay` des Handlers für die erneuten Verbindung auf ein leeres Objekt fest (`{}` oder `new Object()`).

```cshtml
    ...

    <script src="_framework/blazor.server.js" autostart="false"></script>
    <script>
      window.addEventListener('beforeunload', function () {
        Blazor.defaultReconnectionHandler._reconnectionDisplay = {};
      });
    </script>
</body>
```

Um die Anzeige einer erneuten Verbindung zu ersetzen, legen Sie im vorherigen Beispiel `_reconnectionDisplay` auf das anzuzeigende Element fest:

```javascript
Blazor.defaultReconnectionHandler._reconnectionDisplay = 
  document.getElementById("{ELEMENT ID}");
```

Der Platzhalter `{ELEMENT ID}` ist die ID des HTML-Elements, das angezeigt werden soll.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* <xref:fundamentals/logging/index>
