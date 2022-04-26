# Evaluierung serverseitige Programmiersprachen

## Konzept

Ein Embedded System soll Messwerte über eine Webschnittstelle in einer Datenbank ablegen.

Im Fall des Prototypen soll eine URL `http://localhost/put?temp=35.7` aufgerufen werden und die Temperatur mit aktuellem Zeitstempel in der Datenbank abgelegt werden.

## Auswahlkriterien

Bekanntheit der Programmiersprache?

Verfügbarkeit von Hilfestellungen und Dokumentation?

Eigene Erfahrungen mit der Programmiersprache?

## Prototypen

Die Datenbank hat den folgenden Aufbau: 

Folgende Zugangsdaten werden verwendet:

### PHP

```PHP
<?php
// Host, User, Password, Database
$mysql = new mysqli("localhost", "tgmrocks", "svJAuS1TGM!", "tgmrocks");

$stmt = $mysql->prepare("INSERT INTO temperatures(time, temperatur) VALUES (NOW(),?)");
$stmt->bind_param("d", $_GET["temp"]);

var_dump($stmt->execute());
?>
```

Dazu eine kurze Erklärung:

Mit `new mysqli()` wird eine Verbindung zur Datenbank aufgebaut, siehe [PHP: Datenbankverbindungen](https://www.php.net/manual/de/mysqli.quickstart.connections.php)

Mit `$mysql->prepare()` wird ein sogenanntes _Prepared Statement_ erzeugt, bei dem die Variablen anschließend mit `bind_param()` zugewiesen werden müssen.

Mit `$stmt->execute()` wird die Datenbankabfrage ausgeführt (d.h. die Daten in der Datenbank gespeichert).

### Python

```Python
from flask import request
import mysql.connector

mydb = mysql.connector.connect(
  host="localhost",
  user="tgmrocks",
  password="svJAuS1TGM!"
)

mycursor = mydb.cursor()

@app.route('put')
def addData():
    temp = request.args.get('temp')
    
    sql = "INSERT INTO temperatures (time, temperatur) VALUES (NOW(), %d)"
    val = (temp)
    mycursor.execute(sql, val)

    mydb.commit()
```

`flask` ist ein Web-Framework für Python. Mit `@app.route` wird der Aufruf der url `.../put` zur entsprechenden Python-Methode `addData` zugeordnet.

...

## Erkenntnisse

Bis auf die `$` bei den Variablennamen ist PHP _eh_ C. Python hingegen ist syntaktisch mühsam und benötigt ein extra Framework für die Verarbeitung von URL-Parametern.

Die Bewertung der Prototypen anhand der obigen Auswahlkriterien ist wie folgt:


Sprache | Dokumentation | Erfahrung
--- | --- | --- 
PHP | 5.0 | 3
Python | 4.0 | 1
--- | --- | ---
Ergebnis | 9 | 4


## Schlussfolgerung

Wird haben aufgrund ... PHP gewählt.

