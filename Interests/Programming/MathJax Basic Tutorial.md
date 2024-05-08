---
Tags: 
Created: 2022-11-15 03:46:15
---
(Links:: [[LaTeX]])
# Quick reference
1. **For inline formulas, enclose the formula in `$...$` . For displayed formulas, use `$$...$$` .**
   These render differently. For example, type
   `$\sum_{i=0}^n i^2 = \frac{(n^2+n)(2n+1)}{6}$`
   to show $\sum_{i=0}^n i^2 = \frac{(n^2+n)(2n+1)}{6}$ (which is inline mode) or type
   `$$\sum_{i=0}^n i^2 = \frac{(n^2+n)(2n+1)}{6}$$`
   to show
   $$\sum_{i=0}^n i^2 = \frac{(n^2+n)(2n+1)}{6}$$
   (which is display mode). 
2. Fro **Greek letters**, use `\alpha`,`\beta`,...,`\omega`:$\alpha,\beta,...\omega$. For uppercase, use `\Gamma`,`\Delta`,...,`\Omega`:$\Gamma,\Delta,...,\Omega$. Some Greek letters have variant forms: `\epsilon \varepsilon`$\epsilon$,$\varepsilon$,`\phi \varphi`$\phi$,$\varphi$, and others.
3. For **superscripts and subscripts**, use `^`and `_`. For example, `x_i^2`:$x_i^2$,`\log_2 x`:$\log_2 x$.
4. **Groups**. Superscripts, subscripts, and other operations apply only to the next "group". A "group" is either a single symbol, or any formula surrounded by curly braces `{`...`}`. If you do `10^10`, you will get a surprise: $10^10$. But `10^{10}`gibes what you probably wanted: $10^{10}$. Use curly braces to delimit a formula to which a superscript or subscript applies: `x^5^6`is an error;`{x^y}^z` is ${x^y}^z$, and `x^{y^z}` is $x^{y^z}$. Observe the difference between `x_i^2`$x_i^2$ and `x_{i^2}`$x_{i^2}$.
5. **Parentheses** Ordinary symbols `()[]`make parentheses and brackets $(2+3)[4+4]$. Use `\{` and `\}` for curly braces $\{\}$.
   These do *not* scale with the formula in between, so if you write `(\frac{\sqrtx}{y^3})` the parentheses will be too small: $(\frac{\sqrt x}{y^3})$. Using `\left(`...`\right)` will make the sizes adjust automatically to the formula they enclose: `\left(\frac{\sqrt x}{y^3}\right)` is $\left(\frac{\sqrt x}{y^3}\right)$.
   `\left` and `\right` apply to all the following sorts of parentheses: `(` and `)` $(x)$, `[` and `]` $[x]$, `\{` and `\}` $\{x\}$, `|` $|x|$, `\vert` $\vert x\vert$, `\Vert` $\Vert x \Vert$, `\langle` and `\rangle` $\langle x \rangle$, `\lceil` and `\rceil` $\lceil x\rceil$, `\lfloor` and `\floor` $\lfloor x\rfloor$. `\middle` can be used to add additional dividers. There are also invisible parentheses, denoted by `.`:
   `\left.\frac12\right\rbrace` is $\left.\frac12\right\rbrace$.
   If manual size adjustments are required:
   `\Biggl(\biggl(\Bigl(\bigl((x)\bigr)\Bigr)\biggr)\Biggr)` gives $\Biggl(\biggl(\Bigl(\bigl((x)\bigr)\Bigr)\biggr)\Biggr)$. ^a09317
6. **Sums and integrals** `\sum` and `\int`; the subscript is the lower limit and the superscript is the upper limit, so for example `\sum_1^n`  $\sum_1^n$. Don't forget `{`...`}` if the limits are more than a single symbol. For example, `\sum_{i=0}^\infty i^2` is $\sum_{i=0}^\infty i^2$. Similarly, `\prod`$\prod$, `\int`$\int$,`\bigcup`$\bigcup$,`\bigcap`$\bigcap$,`\iint`$\iint$,`\iiint`$\iiint$,`\idotsint`$\idotsint$.
7. **Fractions** There are [three ways to make these](https://math.meta.stackexchange.com/questions/12978/should-dfrac-be-edited-in). `\frac ab`applies to the next two groups, and produces $\frac ab$; for more complicated numerators and denominatros use `{`...`}`:
   ` \frac{a+1}{b+1}` is $\frac{a+1}{b+1}$. If the numerator and denominator are complicated, you may prefer `\over`, which splits up the group that is is in: `{a+1\over b+1}` is ${a+1\over b+1}$. Using `\cfrac{a}{b}`command is useful for continued fractions $\cfrac{a}{b}$.
8. **Fonts**
	1. Use `\mathbb` or `\Bbb` for "blackboard bold": $\Bbb {CHNQRZ}$
	2. Use `\mathbf` for boldface:$\mathbf{ABCDEFGHIJKLMNOPQRSTUVWXYZ abcdefghijklmnopqrstuvwxyz}$
		1. For expression based characters, use `\boldsymbol` instead: $\boldsymbol \alpha$
	3. Use `\mathit` for italics:$\mathit{ABCDEFGHIJKLMNOPQRSTUVWXYZ abcdefghijklmnopqrstuvwxyz}$
	4. Use `pmb`for boldfaced italics:$\pmb{ABCDEFGHIJKLMNOPQRSTUVWXYZ abcdefghijklmnopqrstuvwxyz}$
	5. Use `\mathtt` for "typewriter" font:$\mathtt{ABCDEFGHIJKLMNOPQRSTUVWXYZ abcdefghijklmnopqrstuvwxyz}$
	6. Use `\mathrm` for roman font:$\mathrm{ABCDEFGHIJKLMNOPQRSTUVWXYZ abcdefghijklmnopqrstuvwxyz}$
	7. Use `\mathsf` for sans-serif font:$\mathsf{ABCDEFGHIJKLMNOPQRSTUVWXYZ abcdefghijklmnopqrstuvwxyz}$
	8. Use `\mathcal` for "calligraphic" font:$\mathcal{ABCDEFGHIJKLMNOPQRSTUVWXYZ abcdefghijklmnopqrstuvwxyz}$
	9. Use `\mathscr` for script font:$\mathscr{ABCDEFGHIJKLMNOPQRSTUVWXYZ abcdefghijklmnopqrstuvwxyz}$
	10. Use `\mathfrak` for "Fraktur" font:$\mathfrak{ABCDEFGHIJKLMNOPQRSTUVWXYZ abcdefghijklmnopqrstuvwxyz}$
9. **Radical signs / roots** Use `sqrt`, which adjusts to the size of its argument: `\sqrt{x^3}` $\sqrt{x^3}$; `\sqrt[3]{\frac xy}`$\sqrt[3]{\frac xy}$ . For complicated expressions, consider using `{...}^{1/2}` instead.
10. Some **special functions** such as "lim","sin","max","ln", and so on are normally set in roman font instead of italic font. Use `\lim`,`\sin`,etc, to make these: `\sin x`$\sin x$, not `sin x`$sin x$. Use subscripts to attach a notation to `\lim`:`\lim_{x\to 0}`$$\lim_{x\to 0}$$
    Nonstandard function names can be set with `\operatorname{foo}(x)` $\operatorname{foo}(x)$.
11. There are a very large number of **special symbols and notations**, too many to list here; see this [shorter listing](https://pic.plover.com/MISC/symbols.pdf), or this [exhaustive listing](https://www.ctan.org/tex-archive/info/symbols/comprehensive/symbols-a4.pdf). Some of the most common include:
	1. `\lt \gt \le \leq \leqq \leqslant \ge \geq \geqq \geqslant \neq` $\lt \gt \le \leq \leqq \leqslant \ge \geq \geqq \geqslant \neq$ You can use `\not` to put a slash through almost anything: `\not\lt`$\not\lt$  but it often looks bad.
	2. `\times \div \pm \mp`$\times, \div, \pm, \mp$.`\cdot`is centered dot: $x\cdot y$
	3. `\cup \cap \setminus \subset \subseteq \subsetneq \supset \in \notin \emptyset \varnothing`$\cup, \cap, \setminus, \subset, \subseteq, \subsetneq, \supset, \in, \notin, \emptyset, \varnothing$
	4. `{n+1 \choose 2k}` or `\binom{n+1}{2k}`$\binom{n+1}{2k}$
	5. `\to \rightarrow \leftarrow \Rightarrow \Leftarrow \mapsto`$\to, \rightarrow, \leftarrow, \Rightarrow, \Leftarrow, \mapsto$
	6. `\land \lor \lnot \forall \exists \top \bot \vdash \vDash`$\land, \lor, \lnot, \forall, \exists, \top, \bot, \vdash, \vDash$
	7. `\star \ast \oplus \circ \bullet`$\star, \ast, \oplus, \circ, \bullet$
	8. `\approx \sim \simeq \cong \equiv \prec \lhd \therefore`$\approx, \sim, \simeq, \cong, \equiv, \prec, \lhd, \therefore$
	9. `\infty \aleph_0`$\infty \; \aleph_0$ `\nabla \partial` $\nabla, \partial$ `\Im \Re`$\Im,\Re$
	10. For modular equivalence, use `\pmod` like this: `a\equiv b\pmod` $a\equiv b\pmod n$.
	11. For the binary mod operator, use `\bmod` like this: `a\bmod 17`$a\bmod 17$.
	12. `\ldots` is the dots in $a_1,a_2,\ldots,a_n$ `\cdots` is the dots in $a_1+a_2+\cdots+a_n$
	13. Script lowercase l is `\ell` $\ell$.
12. **Spaces** MathJax usually decides for itself how to space faormulas, using a complex set of rules. Putting extra literal spaces into formulas will not change the amount of space MathJax puts in: 
 `a␣b` and `a␣␣␣␣b` are both $a b$. To add more space, use `\,` for a thin space $a\,b$; `\;` for a wider space $a\;b$. `\quad` and `\qquad` are large spaces: $a\quad b$,$a\qquad b$.
To set plain text, use `\text{...}`: $\{x \in s \mid x \text{ is extra large} \}$. You can nest `$...$` inside of `\text{...}`, for example to access spaces.
1. **Accents and diacritical marks** Use `\hat` for a single $\hat x$, `\widehat` for a larger formula $\widehat {xy}$. If you make it too wide, it will look silly. Similarly, there are `\bar` $\bar x$ and `\overline` $\overline {xyz}$, and `\vec` $\vec x$ and `\overrightarrow`$\overrightarrow {xy}$ and `\overleftrightarrow`$\overleftrightarrow {xy}$. For dots, as in $\frac{d}{dn}x\dot x= \dot x^2+x\ddot x$, use `\dot` and `\ddot`.
2. Special characters used for MathJax interpreting can be escaped using the `\` character: `\\\$` $\\\$$, `\{`$\{$,`\_`$\_$, etc. If you want `\` itself, you should use `\backslash` (symbol) or `\setminus` (binary operation) for $\backslash$, because `\\` is for a new line.
# Matrices
1. Use `$$\begin{matrix}...\end{matrix}$$` In between the `\begin` and `\end` , put the matrix elements. End each matrix row with `\\` , and separate matrix elements with `&` . For example,
   $$
	   \begin{matrix}
	   1 & x & x^2 \\
	   1 & y & y^2 \\
	   1 & z & z^2 \\
	   \end{matrix}
   $$
   MathJax will adjust the sizes of the rows and columns so that everything fits.
2. To add brackets, either use `\left...\right` as in [[MathJax Basic Tutorial#^a09317|section 4]] of the tutorial, or replace `matrix` with `pmatrix` $\begin{pmatrix}1 & 2 \\ 3 & 4 \\\end{pmatrix}$, `bmatrix`$\begin{bmatrix}1 & 2 \\ 3 & 4 \\\end{bmatrix}$, `Bmatrix`$\begin{Bmatrix}1 & 2 \\ 3 & 4 \\\end{Bmatrix}$,`vmatrix`$\begin{vmatrix}1 & 2 \\ 3 & 4 \\\end{vmatrix}$,`Vmatrix`$\begin{Vmatrix}1 & 2 \\ 3 & 4 \\\end{Vmatrix}$.
3. use `\cdots`$\cdots$`\ddots`$\ddots$`\vdots`$\vdots$ when you want to omit some of the entries:
    $$
	   \begin{pmatrix}
	   1 & a_1 & a_1^2 & \cdots & a_1^n \\
	   1 & a_2 & a_2^2 & \cdots & a_2^n \\
	   \vdots & \vdots & \vdots & \ddots & \vdots \\
	   1 & a_m & a_m^2 & \cdots & a_m^n \\
	   \end{pmatrix}
   $$
4. For horizontally "augmented" matrices, put parentheses or brackets around a suitably- formatted table; see [[MathJax Basic Tutorial#Arrays|arrays]] below for details. Here is an example:
   $$\left[
 \begin{array}{cc|c}
	 1&2&3\\
	 4&5&6
 \end{array}
 \right] 
   $$
   The `cc|c` is the crucial part here; it says that there are three centered columns with a vertical bar between the secong and third.
5. For vertically "augmented" matrices, use `\hline`. For example
   $$
   \begin{pmatrix}
	   a & b\\
	   c & d\\ 
   \hline
	   1 & 0\\
	   0&1 
   \end{pmatrix}
 $$
6. For small inline matrices use `\bigl(\begin{smallmatrix} ... \end{smallmatrix}\bigr)`, e.g. $\bigl( \begin{smallmatrix} a & b \\ c & d \end{smallmatrix} \bigr)$ is produced by: `$\bigl( \begin{smallmatrix} a & b \\ c & d \end{smallmatrix} \bigr)$`

# Aligned equations
Often people want a series of equations where the equals signs are aligned. To get this, use `\begin{align}...\end{align}` . Each line should end with `\\` , and should contain an ampersand at the point to align at, typically immediately before the equals sign.

For example,
$$
\begin{align}
 \sqrt{37} & = \sqrt{\frac{73^2-1}{12^2}} \\
  & = \sqrt{\frac{73^2}{12^2}\cdot\frac{73^2-1}{73^2}} \\
  & = \sqrt{\frac{73^2}{12^2}}\sqrt{\frac{73^2-1}{73^2}} \\
  & = \frac{73}{12}\sqrt{1 - \frac{1}{73^2}} \\
  & \approx \frac{73}{12}\left(1 - \frac{1}{2\cdot73^2}\right)
\end{align}
$$
is produced by 
```
\begin{align}
 \sqrt{37} & = \sqrt{\frac{73^2-1}{12^2}} \\
  & = \sqrt{\frac{73^2}{12^2}\cdot\frac{73^2-1}{73^2}} \\
  & = \sqrt{\frac{73^2}{12^2}}\sqrt{\frac{73^2-1}{73^2}} \\
  & = \frac{73}{12}\sqrt{1 - \frac{1}{73^2}} \\
  & \approx \frac{73}{12}\left(1 - \frac{1}{2\cdot73^2}\right)
\end{align}
```
# Arrays
It is often easier to read tables formatted in MathJax rather than plain text or a fixed width font. Arrays and tables are created with the `array` environment. Just after `\begin{array}` the format of each column should be listed, use `c` for a center aligned column, `r` for right aligned, `l` for left aligned and a `|` for a vertical line. Just as with matrices, cells are separated with `&` and rows are broken using `\\`. A horizontal line spanning the array can be placed before the current line with
`\hline`. 

For example,
$$
\begin{array}{c|lcr}
n & \text{Left} & \text{Center} & \text{Right} \\
\hline
1 & 0.24 & 1 & 125 \\
2 & -1 & 189 & -8 \\
3 & -20 & 2000 & 1+10i
\end{array}
$$
```
\begin{array}{c|lcr}
n & \text{Left} & \text{Center} & \text{Right} \\
\hline
1 & 0.24 & 1 & 125 \\
2 & -1 & 189 & -8 \\
3 & -20 & 2000 & 1+10i
\end{array}
```
Arrays can be nested to make an array of tables.

For example,
$$
% outer vertical array of arrays
\begin{array}{c}
% inner horizontal array of arrays
\begin{array}{cc}
% inner array of minimum values
\begin{array}{c|cccc}
\text{min} & 0 & 1 & 2 & 3\\
\hline
0 & 0 & 0 & 0 & 0\\
1 & 0 & 1 & 1 & 1\\
2 & 0 & 1 & 2 & 2\\
3 & 0 & 1 & 2 & 3
\end{array}
&
% inner array of maximum values
\begin{array}{c|cccc}
\text{max}&0&1&2&3\\
\hline
0 & 0 & 1 & 2 & 3\\
1 & 1 & 1 & 2 & 3\\
2 & 2 & 2 & 2 & 3\\
3 & 3 & 3 & 3 & 3
\end{array}
\end{array}
\\
% inner array of delta values
\begin{array}{c|cccc}
\Delta&0&1&2&3\\
\hline
0 & 0 & 1 & 2 & 3\\
1 & 1 & 0 & 1 & 2\\
2 & 2 & 1 & 0 & 1\\
3 & 3 & 2 & 1 & 0
\end{array}
\end{array}
$$

# Definitions by cases (piecewise funtions)
Use `\begin{cases}...\end{cases}`. End each case with a `\\` , and use `&` before parts that should be aligned.

For example, you get this:
$$
f(n) =
\begin{cases}
n/2,  & \text{if $n$ is even} \\
3n+1, & \text{if $n$ is odd}
\end{cases}
$$
by writing this: 
```
f(n) =
\begin{cases}
n/2,  & \text{if $n$ is even} \\
3n+1, & \text{if $n$ is odd}
\end{cases}
```
The brace can be moved to the right:
$$
\left.
\begin{array}{l}
\text{if $n$ is even:}&n/2\\
\text{if $n$ is odd:}&3n+1
\end{array}
\right\}
=f(n)
$$
by writing this:
```
\left.
\begin{array}{l}
\text{if $n$ is even:}&n/2\\
\text{if $n$ is odd:}&3n+1
\end{array}
\right\}
=f(n)
```
To get a larger vertical space between cases we can use `\\[2ex]` instead of `\\` . For example, you get this:
$$ 
f(n) =
\begin{cases}
\frac{n}{2},  & \text{if $n$ is even} \\[2ex]
3n+1, & \text{if $n$ is odd}
\end{cases}
$$
by writing this:
```
f(n) =
\begin{cases}
\frac{n}{2},  & \text{if $n$ is even} \\[2ex]
3n+1, & \text{if $n$ is odd}
\end{cases}
```
(An 'ex' is a length equal to the height of the letter `x` ; `2ex` here means the space should be two exes high.)
# Additional decorations
# System of equations
# Commutative diagrams
$$
\require{AMScd}
\begin{CD}
A @>a>> B\\
@V b V V= @VV c V\\
C @>>d> D
\end{CD}
$$

---
References: https://math.meta.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference