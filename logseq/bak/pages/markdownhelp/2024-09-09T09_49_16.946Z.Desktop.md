- https://katex.org/docs/supported.html#environments
- # General
  collapsed:: true
	- <div>
	  div .... /div - игнорировать markdown в области (нужно писать в скобочках <>)
	  ``` ..... ```  - для кода
	  
	  </div>
- # Math
	- **!!!! далее могут дублироваться символы, очевидно в каких местах, но все таки**
	- <div>
	  Всякую математику нужно заключать в $.......$ (line) или если математики много можно писать блоком, который заключен в $$.......$$.
	  
	  Также можно использовать 
	  ```math
	  
	  ```
	  </div>
	- ## General
	  collapsed:: true
		- <div>\LARGE{smth}
		  \Large{}
		  \Huge{}
		  \huge{}
		  \\ - break line
		  \text{<some_test>} - just write some text
		  
		  </div>
	- ## Matrix
	  collapsed:: true
		- `\begin{matrix}`
		     `a & b \\`
		     `c & d`
		  `\end{matrix}`
		  
		  \begin{matrix}
		     a & b \\
		     c & d
		  \end{matrix}
		- `\begin{pmatrix}`
		     `a & b \\`
		     `c & d`
		  `\end{pmatrix}`
		  
		  \begin{pmatrix}
		     a & b \\
		     c & d
		  \end{pmatrix}
		- `\begin{vmatrix}`
		     `a & b \\`
		     `c & d`
		  `\end{vmatrix}`
		  
		  \begin{vmatrix}
		     a & b \\
		     c & d
		  \end{vmatrix}
	- ## Greek letters
	  collapsed:: true
		- | A `\Alpha` | BB `\Beta` | ΓΓ `\Gamma` | ΔΔ `\Delta` |
		  | EE `\Epsilon` | ZZ `\Zeta` | HH `\Eta` | ΘΘ `\Theta` |
		  | II `\Iota` | KK `\Kappa` | ΛΛ `\Lambda` | MM `\Mu` |
		  | NN `\Nu` | ΞΞ `\Xi` | OO `\Omicron` | ΠΠ `\Pi` |
		  | PP `\Rho` | ΣΣ `\Sigma` | TT `\Tau` | ΥΥ `\Upsilon` |
		  | ΦΦ `\Phi` | XX `\Chi` | ΨΨ `\Psi` | ΩΩ `\Omega` |
		  | 𝛤*Γ* `\varGamma` | 𝛥*Δ* `\varDelta` | 𝛩*Θ* `\varTheta` | 𝛬*Λ* `\varLambda` |
		  | 𝛯*Ξ* `\varXi` | 𝛱*Π* `\varPi` | 𝛴*Σ* `\varSigma` | 𝛶*Υ* `\varUpsilon` |
		  | 𝛷*Φ* `\varPhi` | 𝛹*Ψ* `\varPsi` | 𝛺*Ω* `\varOmega` |  |
		  | 𝛼*α* `\alpha` | 𝛽*β* `\beta` | 𝛾*γ* `\gamma` | 𝛿*δ* `\delta` |
		  | 𝜖*ϵ* `\epsilon` | 𝜁*ζ* `\zeta` | 𝜂*η* `\eta` | 𝜃*θ* `\theta` |
		  | 𝜄*ι* `\iota` | 𝜅*κ* `\kappa` | 𝜆*λ* `\lambda` | 𝜇*μ* `\mu` |
		  | 𝜈*ν* `\nu` | 𝜉*ξ* `\xi` | 𝜊*ο* `\omicron` | 𝜋*π* `\pi` |
		  | 𝜌*ρ* `\rho` | 𝜎*σ* `\sigma` | 𝜏*τ* `\tau` | 𝜐*υ* `\upsilon` |
		  | 𝜙*ϕ* `\phi` | 𝜒*χ* `\chi` | 𝜓*ψ* `\psi` | 𝜔*ω* `\omega` |
		  | 𝜀*ε* `\varepsilon` | ϰϰ `\varkappa` | 𝜗*ϑ* `\vartheta` | 𝜗*ϑ* `\thetasym` |
		  | 𝜛*ϖ* `\varpi` | 𝜚*ϱ* `\varrho` | 𝜍*ς* `\varsigma` | 𝜑*φ* `\varphi` |
		  | ϝϝ `\digamma` |
	- ## Logic and Sets
	  collapsed:: true
		- | ∀ `\forall` | ∁∁ `\complement` | ∴∴ `\therefore` | ∅∅ `\emptyset` |
		  | ∃ `\exists` | ⊂⊂ `\subset` | ∵∵ `\because` | ∅∅ `\empty` |
		  | ∃∃ `\exist` | ⊃⊃ `\supset` | ↦↦ `\mapsto` | ∅∅ `\varnothing` |
		  | ∄∄ `\nexists` | ∣∣ `\mid` | →→ `\to` |   ⟹  ⟹ `\implies` |
		  | ∈∈ `\in` | ∧∧ `\land` | ←← `\gets` |   ⟸  ⟸ `\impliedby` |
		  | ∈∈ `\isin` | ∨∨ `\lor` | ↔↔ `\leftrightarrow` |   ⟺  ⟺ `\iff` |
		  | ∉∈/ `\notin` | ∋∋ `\ni` | ∌∋ `\notni` | ¬¬ `\neg` or `\lnot` |
	- ## Big Operators
	  collapsed:: true
		- | ∑ `\sum` | ∏∏ `\prod` | ⨂⨂ `\bigotimes` | ⋁⋁ `\bigvee` |
		  | ∫∫ `\int` | ∐∐ `\coprod` | ⨁⨁ `\bigoplus` | ⋀⋀ `\bigwedge` |
		  | ∬∬ `\iint` | ∫∫ `\intop` | ⨀⨀ `\bigodot` | ⋂⋂ `\bigcap` |
		  | ∭∭ `\iiint` | ∫∫ `\smallint` | ⨄⨄ `\biguplus` | ⋃⋃ `\bigcup` |
		  | ∮∮ `\oint` | ∯∬​ `\oiint` | ∰∭​ `\oiiint` | ⨆⨆ `\bigsqcup` |
	- ## Fractions
	  collapsed:: true
		- | *a*​ `\frac{a}{b}` | 𝑎𝑏*b**a*​ `\tfrac{a}{b}` | (𝑎𝑎+1](*a*+1*a*​] `\genfrac ( ] {2pt}{1}a{a+1}` |
		  | 𝑎𝑏*b**a*​ `{a \over b}` | 𝑎𝑏*b**a*​ `\dfrac{a}{b}` | 𝑎𝑏+1*b*+1*a*​ `{a \above{2pt} b+1}` |
		  | 𝑎/𝑏*a*/*b* `a/b` |  | 𝑎1+1𝑏1+*b*1​*a*​ `\cfrac{a}{1 + \cfrac{1}{b}}` |
		  
		  ![image.png](../assets/image_1725804348868_0.png)
	- ## Funcs and math operators
	  collapsed:: true
		- |  |
		  | arcsin `\arcsin` | cosec⁡cosec `\cosec` | deg⁡deg `\deg` | sec⁡sec `\sec` |
		  | arccos⁡arccos `\arccos` | cosh⁡cosh `\cosh` | dim⁡dim `\dim` | sin⁡sin `\sin` |
		  | arctan⁡arctan `\arctan` | cot⁡cot `\cot` | exp⁡exp `\exp` | sinh⁡sinh `\sinh` |
		  | arctg⁡arctg `\arctg` | cotg⁡cotg `\cotg` | hom⁡hom `\hom` | sh⁡sh `\sh` |
		  | arcctg⁡arcctg `\arcctg` | coth⁡coth `\coth` | ker⁡ker `\ker` | tan⁡tan `\tan` |
		  | arg⁡arg `\arg` | csc⁡csc `\csc` | lg⁡lg `\lg` | tanh⁡tanh `\tanh` |
		  | ch⁡ch `\ch` | ctg⁡ctg `\ctg` | ln⁡ln `\ln` | tg⁡tg `\tg` |
		  | cos⁡cos `\cos` | cth⁡cth `\cth` | log⁡log `\log` | th⁡th `\th` |
		  | f⁡f `\operatorname{f}` |  |  |  |
		  | arg max⁡argmax `\argmax` | inj lim⁡injlim `\injlim` | min⁡min `\min` | lim→⁡lim​ `\varinjlim` |
		  | arg min⁡argmin `\argmin` | lim⁡lim `\lim` | plim⁡plim `\plim` | lim‾⁡lim​ `\varliminf` |
		  | det⁡det `\det` | lim inf⁡liminf `\liminf` | Pr⁡Pr `\Pr` | lim‾⁡lim `\varlimsup` |
		  | gcd⁡gcd `\gcd` | lim sup⁡limsup `\limsup` | proj lim⁡projlim `\projlim` | lim←⁡lim​ `\varprojlim` |
		  | inf⁡inf `\inf` | max⁡max `\max` | sup⁡sup `\sup` |  |
		  | f⁡f `\operatorname*{f}` | f⁡f `\operatornamewithlimits{f}` |
	- ## EXMP
	  collapsed:: true
		- `\sum_{`
		  `\begin{subarray}{l}`
		     `i\in\Lambda\\`
		     `0<j<n`
		  `\end{subarray}}`
		  
		  $$
		  \LARGE{\sum_{
		  \begin{subarray}{l}
		     i\in\Lambda\\
		     0<j<n
		  \end{subarray}}}
		  $$
		- `x = \begin{cases}`
		     `a &\text{if } b \\`
		     `c &\text{if } d`
		  `\end{cases}`
		  
		  $$
		  \LARGE{x} = \LARGE{\begin{cases}
		     a &\text{if } b \\
		     c &\text{if } d
		  \end{cases}}
		  $$
	-