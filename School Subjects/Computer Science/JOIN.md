# JOIN
-  **Kreuzprodukt** aus beiden Tabellen
## Schreibweise Nr.1
```SQL
SELECT Dozent.Name FROM Kurs INNER JOIN Dozent ON Kurs.PersNr = Dozent.PersNr
WHERE Kurs.Name = 'Java' ;
```

## Schreibweise Nr.2
```SQL
SELECT Dozent.Name FROM Kurs, Dozent WHERE Kurs.PersNr = Dozent.PersNr AND Kurs.Name = 'Java' ;
```

## JOINs über mehrere Tabellen
```SQL
SELECT Kurs.Titel FROM Student, besucht, Kurs
WHERE Student.MatrNr = besucht.MatrNr 
AND besucht.KursNr = Kurs.KursNr 
AND Student.Name = 'Jansen' ;
```