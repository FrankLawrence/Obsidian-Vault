---
Tags: 
Created: 2022-11-30 18:38:02
---
(Links:: [[Astronomy]])
# Scheinbare Helligkeit $m$
```ad-definition
collapse: none
Helligkeit mit der die Sterne von der Erde aus erscheinen
```
![[Helligkeiten 2022-11-30 18.43.34.excalidraw]]
- Einheit **Magnitude** $m$
- rückwärts laufender Skala
- Erweiterung
	- Sonne: -26,3
	- Vollmond: -12
	- Venus: -4
	- Sirius: -1,5 (Hellster Stern am Himmel)
	- Wega: 0
## Mathematische Beschreibung der scheinbaren Helligkeit
Wie hängt unser Empfinden v. Helligkeit mit der Intensität des Lichtes zusammen?

- Schall und Licht werden **logarithmisch wahrgenommen** (10 mal so viele Lautstärker -> doppelte Lautstärke wird empfunden)
- Unterschied 2er scheinbarer Helligkeiten:  $$m_1-m_2=-2,5\log\left(\frac{I_1}{I_2}\right)$$
- Strahlungsintensität $I$ in $\frac{J}{sm^2}$
  messbare Größe
- Wahrnehmung $m_1$
$$\Rightarrow \frac{I_1}{I_2}=10^{\frac{m_2-m_1}{2,5}}$$

- Magnitudenunterschied $$\Delta m=1 \rightarrow \frac{I_1}{I_2}=10^{\frac{1}{2,5}}=2,5$$ $$\Delta m=5\rightarrow\frac{I_1}{I_2}=10^2=100$$

Festlegung eines **absoluten Skala**: $$\text{Wega},\; m_{\text{Wega}}=0$$ $$m_{\text{Stern}}=m_{\text{Wega}}-2,5\log\left(\frac{I_{Sonne}}{I_{Wega}}\right)\Rightarrow m_*=0-2,5\log\left(\frac{I_{*}}{I_{Wega}}\right)$$
# Absolute Helligkeiten
**Idee: Ordne alle Sterne in der gleichen Entfernung ($10\,pc=32,3\,LJ$) an**
```ad-important
collapse: none
absolute Helligkeit $M$ = scheinbarer Helligkeit $m$ 10pc-Entfernung
```
## Mathematische Beschreibung der absoluten Helligkeit $M$
1. quadrat. Abstandsgesetz: $I=\frac{L}{4\pi r^2}$
   Leuchtkraft $L$ (gesamte Strahlungsleistung)
2. Es gilt: $m_1-m_2=-2,5\log\left(\frac{I_1}{I_2}\right)$
3. Wir beziehen das ganze auf 10pc 
   $$\begin{align}
   m_1-m_2&=-2,5\log\left(\frac{\frac{L_1}{4\pi(10pc)^2}}{\frac{L_2}{4\pi(10pc)^2}}\right)\\
   & = -2,5\log\left(\frac{L_1}{L_2}\right)\\
   & = M_1-M_2
   \end{align}$$
4. Zusammenhang zwischen scheinbarer Helligkeit und absoluter Helligkeit: Entfernungsmodul
   $$\begin{align}
   m-M&=-2,5\cdot\log\left(\frac{\frac{L_1}{4\pi r^2}}{\frac{L_1}{4\pi(10pc)^2}}\right) \\
   &=-2,5\log\left(\frac{(10pc)^2}{r^2}\right) \\
   & = -5 \log\left(\frac{10pc}{r}\right)\\
   & = 5 \log\left(\frac{r}{10pc}\right)
   \end{align}$$
- Falls $r$ und $m$ bekannt: **Bestimmung von $M$ möglich**
- Falls $m$ und $M$ bekannt: **Bestimmung der Sternentfernung**

---
References: