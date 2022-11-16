# Was ist ein Datenbankschema
kompakte Schreibweise um Tabellen mit ihren Spaltnamen und Schlüsseln anzugeben: **relationales Datenbankschema**
Datenbankschema besteht aus **Realtionschemata**
## Schreibweise
`Tabellenname(_Primärschlüssel_, ↑Fremdschlüssel, Attribut3, Atribut4, …)`
### 1:n-Beziehungstyp
Für einen 1-n-Beziehungstyp wird ein _Fremdschlüssel_ auf Seite des 'abhängigen' Entitätstyps hinzugefügt (dort, wo im Diagramm das 'n' steht)
### n:m-Beziehungstyp
Es wird für ==_jede n:m-Beziehung eine zusätzliche Tabelle_== angelegt