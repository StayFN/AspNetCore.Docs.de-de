<a name="ddav"></a>
### <a name="disable-default-account-verification"></a>Standardkonto Überprüfung deaktivieren

Mit den Standardvorlagen wird der Benutzer an den umgeleitet, `Account.RegisterConfirmation` wo er einen Link auswählen kann, um das Konto zu bestätigen. Der Standardwert `Account.RegisterConfirmation` wird ***nur*** für Tests verwendet, die automatische Kontoüberprüfung sollte in einer Produktions-App deaktiviert werden.

Wenn Sie ein bestätigtes Konto benötigen und eine sofortige Anmeldung bei der Registrierung verhindern möchten, legen Sie `DisplayConfirmAccountLink = false` in */Areas/Identity/Pages/Account/RegisterConfirmation.cshtml.cs*fest:

[!code-csharp[](~/security/authentication/identity/sample/WebApp3/Areas/Identity/Pages/Account/RegisterConfirmation.cshtml.cs?name=snippet&highlight=34)]