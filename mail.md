# Mails-versenden

E-Mails können mit der Applikation einfach versendet werden. Dazu muss zuerst eine neue Instanz der Klasse **Mail** erzeugt werden.

```typescript
new Mail(to: Recipient[], messageTemplate: SmtpMessage, replacementParams: string[]);
```

Empfänger müssen als Instanz der Klasse **Recipient** übergeben werden. 

```typescript
new Recipient(name: string, address: string);
```

Danach wird die Mail Vorlage übergeben. Die Vorlage ist eine Instanz der Klasse **SmtpMessage**. Die SMTP-Message beinhaltet den Betreff, die Nachricht als HTML und die Nachricht als Plain-text. Im Body der SMTP-Message befinden sich Platzhalter, die durch konkrete Werte ersetzt werden. Die Ersetzungsvariablen werden durch ein Array übergeben. Wenn zu wenige Platzhalter übergeben werden, dann wirft die Funktion einen Fehler. Die Variablen werden in der Reihenfolge ersetzt wie sie im Template und im Array zu finden sind.

```typescript
Beispiel:

const m = new Mail([user.recipient], passwordReset, [user.fullNameWithSirOrMadam, user.resetcode.toString(), formatDate(user.resetcodeValidUntil)]);
  
mailTransport.sendMail(m);
```

SmtpMessage-Templates mit Platzhaltervariablen werden folgendermaßen erstellt:

```typescript
const template = new SmtpMessage();

template.subject = "Betreff";

template.html = "<h1>Test ::placeholder:: test ::placeholder::</h1>";

template.text = "Test ::placeholder:: test ::placeholder::";
```
Es wird immer eine HTML-Variante und eine Plain-text Variante der Mail definiert, damit die E-Mail auf jedem Client betrachtet werden kann. Platzhalter Variablen werden folgendermaßen definiert: *::name::*

Der **Mail**-Klasse kann noch weitere Eigenschaften annehmen. Dazu zählen unter anderem:
 - cc: Recipient[]
 - bcc: Recipient[]
 - attachments: Attachment[]
 - replyTo: string
 - priority: MailPriority
 - headers: object

MailPriorities:
 - LOW
 - NORMAL
 - HIGH

## Attachments
WIP
