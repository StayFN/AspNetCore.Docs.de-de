> [!NOTE]
> <span data-ttu-id="68615-101">Wenn das Azure-Portal den Bereichs-URI für die App bereitstellt und die App beim Empfang einer *401 – nicht autorisiert*-Antwort von der API einen Ausnahmefehler auslöst, versuchen Sie, einen Bereichs-URI zu verwenden, der Schema und Host nicht enthält.</span><span class="sxs-lookup"><span data-stu-id="68615-101">If the Azure portal provides the scope URI for the app and the app throws an unhandled exception when it receives a *401 Unauthorized* response from the API, try using a scope URI that doesn't include the scheme and host.</span></span> <span data-ttu-id="68615-102">Ein Beispiel: Das Azure-Portal stellt eins der folgenden Formate für Bereichs-URIs bereit:</span><span class="sxs-lookup"><span data-stu-id="68615-102">For example, the Azure portal may provide one of the following scope URI formats:</span></span>
>
> * `https://{TENANT}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{SERVER API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> <span data-ttu-id="68615-103">Versuchen Sie, den Bereichs-URI ohne Schema und Host anzugeben:</span><span class="sxs-lookup"><span data-stu-id="68615-103">Try supplying the scope URI without the scheme and host:</span></span>
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "{SERVER API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```
>
> <span data-ttu-id="68615-104">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="68615-104">For example:</span></span>
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "41451fa7-82d9-4673-8fa5-69eff5a761fd/API.Access");
> ```
