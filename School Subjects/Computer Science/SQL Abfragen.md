### Abfragen mit SELECT über eine Tabelle
- SELECT-Abfrage: Gibt an aus welcher Tabelle und welchen Spalten Daten gesucht werden
_Allgemein Form eines SELECT-Befehls_:

```SQL
SELECT [Spalte(n)] FROM [Tabelle(n)] WHERE  [Suchausdruck];
```
_Operatoren für die where-Klausel_
= (gleich)
<> (ungleich)

_Logische Verknüpfungen_
AND
OR
NOT
BETWEEN a AND b

_Beispeiel_:
**SELECT** Titel, PersNr **FROM** Kurs
**WHERE** Titel = 'Datenbank' **OR** Titel = 'HTML';

_Textmuster_:
**SELECT** * **FROM** Student **WHERE** Name **LIKE** 'M%';

_Menge_:
**SELECT** KursNr, Titel **FROM** Kurs **WHERE** Titel **IN** ('HTML', 'Java');

### Reihenfolge der Ergebnisse ordnen

**ORDER BY** [Spalte(n)] **ASC** | **DESC**;

### UNION: Ergebnisse zweier SELECT-Befehle verbinden

```SQL
SELECT ... FROM ... WHERE ...
UNION
SELECT ... FROM ... WHERE ... ;
```