<svg viewBox="0 0 800 800" width="100%" height="100vh" xmlns="http://www.w3.org/2000/svg">
	<!------------------------ (a) ------------------------>
	<text x="188" y="700" font-size="20" font-family="Arial" fill="var(--text-normal)">(c)</text>
	
	<circle cx="200" cy="450" r="5" fill="var(--text-normal)"></circle> <!-- 0 -->
	<text x="194" y="440" font-size="20" font-family="Arial" fill="var(--text-normal)">0</text>
	<circle cx="257" cy="475" r="5" fill="var(--text-normal)"></circle> <!-- 1 -->
	<text x="267" y="467" font-size="20" font-family="Arial" fill="var(--text-normal)">1</text>
	<circle cx="290" cy="525" r="5" fill="var(--text-normal)"></circle> <!-- 2 -->
	<text x="305" y="523" font-size="20" font-family="Arial" fill="var(--text-normal)">2</text>
	<circle cx="275" cy="585" r="5" fill="var(--text-normal)"></circle> <!-- 3 -->
	<text x="288" y="605" font-size="20" font-family="Arial" fill="var(--text-normal)">3</text>
	<circle cx="230" cy="630" r="5" fill="var(--text-normal)"></circle> <!-- 4 -->
	<text x="235" y="655" font-size="20" font-family="Arial" fill="var(--text-normal)">4</text>
	<circle cx="170" cy="630" r="5" fill="var(--text-normal)"></circle> <!-- 5 -->
	<text x="155" y="655" font-size="20" font-family="Arial" fill="var(--text-normal)">5</text>
	<circle cx="125" cy="585" r="5" fill="var(--text-normal)"></circle> <!-- 6 -->
	<text x="102" y="605" font-size="20" font-family="Arial" fill="var(--text-normal)">6</text>
	<circle cx="110" cy="525" r="5" fill="var(--text-normal)"></circle> <!-- 7 -->
	<text x="85" y="523" font-size="20" font-family="Arial" fill="var(--text-normal)">7</text>
	<circle cx="143" cy="475" r="5" fill="var(--text-normal)"></circle> <!-- 8 -->
	<text x="121" y="467" font-size="20" font-family="Arial" fill="var(--text-normal)">8</text>
	
	<!-- 0 -> 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 0 -->
	<polyline points="200,450 257,475 290,525 275,585 230,630 170,630 125,585 110,525 143,475 200,450" fill="none" stroke="var(--text-normal)"/>
	<!-- 0 -> 2 -> 4 -> 6 -> 8 -> 1 -> 3 -> 5 -> 7 -> 0 -->
	<polyline points="200,450 290,525 230,630 125,585 143,475 257,475 275,585 170,630 110,525 200,450" fill="none" stroke="var(--text-normal)"/>
	<!------------------------ (d) ------------------------>
	<text x="588" y="700" font-size="20" font-family="Arial" fill="var(--text-normal)">(d)</text>
	
	<circle cx="600" cy="450" r="5" fill="var(--text-normal)"></circle> <!-- 0 -->
	<text x="594" y="440" font-size="20" font-family="Arial" fill="var(--text-normal)">0</text>
	<circle cx="657" cy="475" r="5" fill="var(--text-normal)"></circle> <!-- 1 -->
	<text x="667" y="467" font-size="20" font-family="Arial" fill="var(--text-normal)">1</text>
	<circle cx="690" cy="525" r="5" fill="var(--text-normal)"></circle> <!-- 2 -->
	<text x="705" y="523" font-size="20" font-family="Arial" fill="var(--text-normal)">2</text>
	<circle cx="675" cy="585" r="5" fill="var(--text-normal)"></circle> <!-- 3 -->
	<text x="688" y="605" font-size="20" font-family="Arial" fill="var(--text-normal)">3</text>
	<circle cx="630" cy="630" r="5" fill="var(--text-normal)"></circle> <!-- 4 -->
	<text x="635" y="655" font-size="20" font-family="Arial" fill="var(--text-normal)">4</text>
	<circle cx="570" cy="630" r="5" fill="var(--text-normal)"></circle> <!-- 5 -->
	<text x="555" y="655" font-size="20" font-family="Arial" fill="var(--text-normal)">5</text>
	<circle cx="525" cy="585" r="5" fill="var(--text-normal)"></circle> <!-- 6 -->
	<text x="502" y="605" font-size="20" font-family="Arial" fill="var(--text-normal)">6</text>
	<circle cx="510" cy="525" r="5" fill="var(--text-normal)"></circle> <!-- 7 -->
	<text x="485" y="523" font-size="20" font-family="Arial" fill="var(--text-normal)">7</text>
	<circle cx="543" cy="475" r="5" fill="var(--text-normal)"></circle> <!-- 8 -->
	<text x="521" y="467" font-size="20" font-family="Arial" fill="var(--text-normal)">8</text>
	
	<!-- 0 -> 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 0 -->
	<polyline points="600,450 657,475 690,525 675,585 630,630 570,630 525,585 510,525 543,475 600,450" fill="none" stroke="var(--text-normal)"/>
	<!-- 0 -> 2 -> 4 -> 6 -> 8 -> 1 -> 3 -> 5 -> 7 -> 0 -->
	<polyline points="600,450 690,525 630,630 525,585 543,475 657,475 675,585 570,630 510,525 600,450" fill="none" stroke="var(--text-normal)"/>
	<!-- dotted -->
	<line x1="600" y1="450" x2="630" y2="630" stroke-dasharray="5,5" stroke="var(--text-normal)"></line> <!-- 0 -> 4 -->
	<line x1="630" y1="630" x2="543" y2="475" stroke-dasharray="5,5" stroke="var(--text-normal)"></line> <!-- 4 -> 8 -->
	<line x1="657" y1="475" x2="570" y2="630" stroke-dasharray="5,5" stroke="var(--text-normal)"></line> <!-- 1 -> 5 -->
	<line x1="690" y1="525" x2="525" y2="585" stroke-dasharray="5,5" stroke="var(--text-normal)"></line> <!-- 2 -> 6 -->
	<line x1="675" y1="585" x2="510" y2="525" stroke-dasharray="5,5" stroke="var(--text-normal)"></line> <!-- 3 -> 7 -->
	
	<!------------------------ (a) ------------------------>
	<circle cx="200" cy="50" r="5" fill="var(--text-normal)"></circle> <!-- 0 -->
	<text x="194" y="38" font-size="20" font-family="Arial" fill="var(--text-normal)">0</text>
	<circle cx="260" cy="80" r="5" fill="var(--text-normal)"></circle> <!-- 1 -->
	<text x="270" y="70" font-size="20" font-family="Arial" fill="var(--text-normal)">1</text>
	<circle cx="290" cy="140" r="5" fill="var(--text-normal)"></circle> <!-- 2 -->
	<text x="305" y="147" font-size="20" font-family="Arial" fill="var(--text-normal)">2</text>
	<circle cx="260" cy="200" r="5" fill="var(--text-normal)"></circle> <!-- 3 -->
	<text x="272" y="225" font-size="20" font-family="Arial" fill="var(--text-normal)">3</text>
	<circle cx="200" cy="230" r="5" fill="var(--text-normal)"></circle> <!-- 4 -->
	<text x="194" y="260" font-size="20" font-family="Arial" fill="var(--text-normal)">4</text>
	<circle cx="140" cy="200" r="5" fill="var(--text-normal)"></circle> <!-- 5 -->
	<text x="115" y="225" font-size="20" font-family="Arial" fill="var(--text-normal)">5</text>
	<circle cx="110" cy="140" r="5" fill="var(--text-normal)"></circle> <!-- 6 -->
	<text x="85" y="147" font-size="20" font-family="Arial" fill="var(--text-normal)">6</text>
	<circle cx="140" cy="80" r="5" fill="var(--text-normal)"></circle> <!-- 7 -->
	<text x="115" y="70" font-size="20" font-family="Arial" fill="var(--text-normal)">7</text>
	<text x="188" y="300" font-size="20" font-family="Arial" fill="var(--text-normal)">(a)</text>
	
	<!-- 0 -> 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 0 -->
	<polyline points="200,50 260,80 290,140 260,200 200,230 140,200 110,140 140,80 200,50" fill="none" stroke="var(--text-normal)"/>
	<!-- 0 -> 2 -> 4 -> 6 -> 0 -->
	<polyline points="200,50 290,140 200,230 110,140 200,50" fill="none" stroke="var(--text-normal)"/>
	<!-- 1 -> 3 -> 5 -> 7 -> 1 -->
	<polyline points="260,80 260,200 140,200 140,80 260,80" fill="none" stroke="var(--text-normal)"/>
	
	<!------------------------ (b) ------------------------>
	<text x="588" y="300" font-size="20" font-family="Arial" fill="var(--text-normal)">(b)</text>
	<circle cx="600" cy="50" r="5" fill="var(--text-normal)"></circle> <!-- 0 -->
	<text x="594" y="38" font-size="20" font-family="Arial" fill="var(--text-normal)">0</text>
	<circle cx="660" cy="80" r="5" fill="var(--text-normal)"></circle> <!-- 1 -->
	<text x="670" y="70" font-size="20" font-family="Arial" fill="var(--text-normal)">1</text>
	<circle cx="690" cy="140" r="5" fill="var(--text-normal)"></circle> <!-- 2 -->
	<text x="705" y="147" font-size="20" font-family="Arial" fill="var(--text-normal)">2</text>
	<circle cx="660" cy="200" r="5" fill="var(--text-normal)"></circle> <!-- 3 -->
	<text x="672" y="225" font-size="20" font-family="Arial" fill="var(--text-normal)">3</text>
	<circle cx="600" cy="230" r="5" fill="var(--text-normal)"></circle> <!-- 4 -->
	<text x="594" y="260" font-size="20" font-family="Arial" fill="var(--text-normal)">4</text>
	<circle cx="540" cy="200" r="5" fill="var(--text-normal)"></circle> <!-- 5 -->
	<text x="515" y="225" font-size="20" font-family="Arial" fill="var(--text-normal)">5</text>
	<circle cx="510" cy="140" r="5" fill="var(--text-normal)"></circle> <!-- 6 -->
	<text x="485" y="147" font-size="20" font-family="Arial" fill="var(--text-normal)">6</text>
	<circle cx="540" cy="80" r="5" fill="var(--text-normal)"></circle> <!-- 7 -->
	<text x="515" y="70" font-size="20" font-family="Arial" fill="var(--text-normal)">7</text>
	
	<!-- 0 -> 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 0 -->
	<polyline points="600,50 660,80 690,140 660,200 600,230 540,200 510,140 540,80 600,50" fill="none" stroke="var(--text-normal)"/>
	<!-- 0 -> 2 -> 4 -> 6 -> 0 -->
	<polyline points="600,50 690,140 600,230 510,140 600,50" fill="none" stroke="var(--text-normal)"/>
	<!-- 1 -> 3 -> 5 -> 7 -> 1 -->
	<polyline points="660,80 660,200 540,200 540,80 660,80" fill="none" stroke="var(--text-normal)"/>
	<!-- dotted -->
	<line x1="600" y1="50" x2="600" y2="230" stroke-dasharray="5,5" stroke="var(--text-normal)"></line>
	<line x1="510" y1="140" x2="690" y2="140" stroke-dasharray="5,5" stroke="var(--text-normal)"></line>
	<line x1="510" y1="140" x2="690" y2="140" stroke-dasharray="5,5" stroke="var(--text-normal)"></line>
	<line x1="660" y1="80" x2="540" y2="200" stroke-dasharray="5,5" stroke="var(--text-normal)"></line>
	<line x1="540" y1="80" x2="660" y2="200" stroke-dasharray="5,5" stroke="var(--text-normal)"></line>
</svg>
