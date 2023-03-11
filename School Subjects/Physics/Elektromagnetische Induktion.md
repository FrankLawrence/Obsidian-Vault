Status: 
Tags: [[Elektrizitätslehre]]
Links: [[Physics⚛]]
___
# Induktionspannung
- Elektronen bewegen sich in einem Leiter / elektrischer Strom-> magnetisches Feld
- Bewegung von Ladungsträgern in einem Magnetfled -> Strom 
	**elektromagnetische Induktion**
- Eine Induktionsspannung wird hervorgerufen wenn:
	- sich die magentische Feldstärke $B$ des homogenen magnetischen Feldes, in dem sich Teile oder die ganze Leiterschleife befindet **ändert**
	- sich der Flächeninhalt $A$ der (Teil-)Fläche der Leiterschleife, die sich im magnetischen Feld befindet, **ändert**
	- sich die Weite $\varphi$ des Winkels zwischen Feldstärkevektor $\vec B$ und Flächenvektor $\vec A$ **ändert**
- Wenn die Feldstärke $B$/ Flächeninhalt $A$ vergrößert wird, dann ist die Induktionsspannung $U_{ind}$ **negativ**
- Je schneller die Feldstärke/Flächeninhalt verändert wird, desto größer ist der Betrag der Induktionsspannung

> [!Formula] 
> $$U_{ind}=B\cdot l\cdot v$$

Spule wird von einem sich **zeitlich ändernden Magnetfeld** durchsetzt
> [!Formula] 
> $$U=n\cdot A_s \cdot \dot B$$
> $$\rightarrow U_{ind}=-n\cdot\dot \Phi$$
> $$\Phi = B\cdot A \cdot \cos(\varphi)$$

## [[Lenzische Regel]]
# [[Selbstinduktion]]
## Einschaltvorgang
- Der Strom geht nicht sofort auf seinen stationären Endwert $I_0=\frac {U_0}R$, sondern steigt allmählich auf diesen Endwert an.
- Mit dem Stromanstieg ist eine Zunahme des magnetischen Flusses in der Spule verbunden: $\frac{d\Phi}{dt}\gt 0$.
- Die Flussänderung ruft eine induzierte Spannung $U_i$ hervor, die ([[Lenzische Regel|nach der Regel von LENZ]]) der von außen angelegten Spannung $U_0$ entgegengerichtet ist. Die an der idealen Spule $L$ anliegende Spannung $U_L$ ist gegengleich zu dieser Induktionsspannung.
### Diagramme
![](https://www.leifiphysik.de/sites/default/files/medien/Ein_und_Ausschalten_von_Spulen_Einschalten_I.svg)
![](https://www.leifiphysik.de/sites/default/files/medien/Ein_und_Ausschalten_von_Spulen_Einschalten_UR.svg)
![](https://www.leifiphysik.de/sites/default/files/medien/Ein_und_Ausschalten_von_Spulen_Einschalten_UL.svg)
## Ausschaltvorgang
- Der Strom geht nicht sofort auf Null zurück, sondern sinkt allmählich auf Null ab.
- Mit dem Stromabfall ist eine Abnahme des magnetischen Flusses in der Spule verbunden: $\frac{d\Phi}{dt}\lt 0$.
- Die Flussänderung ruft eine induzierte Spannung $U_i$ hervor, die die aufgrund des Induktionsgesetzes in differentieller Form positiv ist.
### Diagramme
![](https://www.leifiphysik.de/sites/default/files/medien/Ein_und_Ausschalten_von_Spulen_Ausschalten_I.svg)
![](https://www.leifiphysik.de/sites/default/files/medien/Ein_und_Ausschalten_von_Spulen_Ausschalten_UR.svg)
![](https://www.leifiphysik.de/sites/default/files/medien/Ein_und_Ausschalten_von_Spulen_Ausschalten_UL.svg)
___
Reference:
https://www.leifiphysik.de/elektrizitaetslehre/elektromagnetische-induktion
Created: 2022-04-12 00:27