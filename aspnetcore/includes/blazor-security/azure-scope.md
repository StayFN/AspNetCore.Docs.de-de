> [!NOTE]
> Wenn das Azure-Portal den Bereichs-URI für die App bereitstellt und die App beim Empfang einer *401 – nicht autorisiert*-Antwort von der API einen Ausnahmefehler auslöst, versuchen Sie, einen Bereichs-URI zu verwenden, der Schema und Host nicht enthält. Ein Beispiel: Das Azure-Portal stellt eins der folgenden Formate für Bereichs-URIs bereit:
>
> * `https://{TENANT}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{SERVER API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> Versuchen Sie, den Bereichs-URI ohne Schema und Host anzugeben:
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "{SERVER API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```
>
> Zum Beispiel:
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "41451fa7-82d9-4673-8fa5-69eff5a761fd/API.Access");
> ```
