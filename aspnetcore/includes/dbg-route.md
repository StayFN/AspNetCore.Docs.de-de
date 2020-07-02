## <a name="debug-diagnostics"></a><span data-ttu-id="ca2b3-101">Debugdiagnose</span><span class="sxs-lookup"><span data-stu-id="ca2b3-101">Debug diagnostics</span></span>

<span data-ttu-id="ca2b3-102">Legen Sie für eine ausführliche Routingdiagnoseausgabe `Logging:LogLevel:Microsoft` auf `Debug` fest.</span><span class="sxs-lookup"><span data-stu-id="ca2b3-102">For detailed routing diagnostic output, set `Logging:LogLevel:Microsoft` to `Debug`.</span></span> <span data-ttu-id="ca2b3-103">Legen Sie in der Entwicklungsumgebung die Protokollebene in *appsettings.Development.jsauf*fest:</span><span class="sxs-lookup"><span data-stu-id="ca2b3-103">In the development environment, set the log level in *appsettings.Development.json*:</span></span>

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Debug",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  }
}
```
