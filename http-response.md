# HTTP-Response

Die HTTP-Response Klasse bietet ein Standardformat für die Antworten der REST API. Jeder Endpoint sollte eine Instanz von HTTP-Response zurückgeben. Es exsitieren 3 unterschiedliche Arten von Antworten, die jeweils eine andere Daten zurücksenden.

```typescript
new HttpResponse(status?: HttpResponseStatus, data?: any, messages?: HttpResponseMessage[]);
```

| Typ | Beschreibung | Erforderliche Eigenschaften | Optionale Eigenschaften |
|-----|--------------|-----------------------------|-------------------------|
| success | Erfolgreicher Request | status, data | messages |
| fail | Fehlerhafter Request, wodurch eine Vorbedingung der API nicht erfüllt wurde. | status | data, messages |
| error | Ein Fehler ist während der Bearbeitung aufgetreten.  z.B.: Datenbankfehler, Exception, ... | status | data, message |
_____________

Beispiel:  Response bei erfolgreichem Request

```typescript
new HttpResponse(
	HttpResponseStatus.SUCCESS,
	{id: 1},
	[
		new HttpResponseMessage(HttpResponseMessageSeverity.success, "message")
	]
);
```

Beispiel: Reponse bei fehlerhaftem Request
```typescript
new HttpResponse(
	HttpResponseStatus.FAIL,
	{},
	[
		new HttpResponseMessage(HttpResponseMessageSeverity.error, "fail")
	]
);
```

Beispiel: Response bei serverseitigem Fehler
```typescript
new HttpResponse(
	HttpResponseStatus.ERROR,
	{},
	[
		new HttpResponseMessage(HttpResponseMessageSeverity.error, "error")
	]
);
```

Mögliche Response Status:
 - *SUCCESS*
 - *FAIL*
 - *ERROR*

Angelehnt an: [https://github.com/omniti-labs/jsend](https://github.com/omniti-labs/jsend)

## HttpResponseMessage
 
Eine HTTP-Response Message ist eine Statusmeldung. Severity gibt an, wie schwerwiegend der Fehler ist. Die Severities repräsentieren die Farben der Alerts in Ionic. Standardmäßig werden nur die **DANGER** Nachrichten angezeigt. Wenn die Variable *visible* auf *true* gesetzt wird, dann werden auch die anderen angezeigt.

```typescript
new HttpResponseMessage(severity?: HttpResponseMessageSeverity, message?: string, visible: boolean);
```

Mögliche Message Severities:
 - *SUCCESS*
 - *WARNING*
 - *DANGER*
 - *INFO*


