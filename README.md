## 1. Prerrequisitos y configuración inicial

1. **NeoVim con LuaSnips**
    
    - Asegúrate de tener NeoVim (≥ 0.5) instalado.
    - Instala el plugin [LuaSnips](https://github.com/L3MON4D3/LuaSnips) (puedes usar [packer.nvim](https://github.com/wbthomason/packer.nvim), [vim-plug](https://github.com/junegunn/vim-plug) u otro gestor).
    - En tu `init.vim` o `init.lua`, configura LuaSnips para que cargue snippets en formato Snipmate. Por ejemplo, en `init.lua`:
```lua
require("luasnip.loaders.from_snipmate").lazy_load({ paths = { "~/.config/nvim/snippets" } })
```

   - Esto hará que LuaSnips parseé todos los archivos `*.snippets` que tengas en `~/.config/nvim/snippets`.
        
2. **Estructura de directorios**
- Crea el directorio de snippets, por ejemplo:
```ruby
~/.config/nvim/snippets/
  └── tex.snippets
```
   - El nombre `tex.snippets` hace que LuaSnips (y opcionalmente Snipmate) cargue esos snippets cuando el buffer esté en modo `filetype=tex` o `filetype=latex` (asegúrate de que NeoVim detecte correctamente tu archivo `.tex` como `tex` o `latex`).
        
3. **Otras herramientas recomendadas**
- **vimtex**  
	Instalando y configurando [vimtex](https://github.com/lervag/vimtex) consigues resaltado, compilación y sincronización con el PDF. Por ejemplo:
```vim
Plug 'lervag/vimtex'
let g:vimtex_view_method = 'zathura'
let g:tex_flavor = 'latex'
```

Así, podrás compilar y revisar en tiempo real mientras tomas notas.         

---

## 2. Sintaxis básica de Snipmate

La sintaxis de Snipmate es muy parecida a la de TextMate. Un archivo `*.snippets` para Snipmate (y que LuaSnips posteriormente reconocerá) sigue estas reglas fundamentales:
```text
snippet <trigger> "<descripción opcional>" [opciones]
    <línea 1 del cuerpo ...>
    <línea 2 del cuerpo ...>
    ...
    <lugar de parada final>  ← normalmente se usa ${0}
<un comentario o línea vacía que separa este snippet del siguiente>
```
- **`trigger`**: palabra o patrón que, al escribirlo en modo inserción y presionar `<Tab>`, expande el snippet. 
- **`"<descripción opcional>"`**: texto que describe el snippet; aparece en menús de sugerencia.
- **`[opciones]`**:
    - `b` → solo expandir al inicio de línea (beginning of line).
    - `w` → expandir en los límites de palabra (word boundary).
    - `i` → expandir en cualquier posición (inner).
    - `r` → trigger es una expresión regular. 
    - `A` → autoexpand (expande automáticamente sin necesidad de `<Tab>`).
- **Puntos de tabulación (tab stops)**:
    - `${1}`, `${2}`, … definen lugares donde el cursor saltará sucesivamente con `<Tab>`.
    - `${0}` indica la posición final, donde quedará el cursor al terminar la expansión.
    - `${VISUAL}` sirve para reutilizar texto previamente seleccionado (modo visual) dentro del snippet.
- Cada snippet termina con una línea vacía o un comentario (`#`) para separarlo del siguiente.

---

## 3. Estructura de un archivo `tex.snippets` orientado a notas matemáticas

A continuación se presenta la clasificación propuesta (tomada del _snippets de ejemplo_) y, para cada categoría, se muestra cómo definir snippets Snipmate concretos, junto con comentarios que explican la sintaxis y uso. Las secciones siguen exactamente este orden:

1. **Entornos personalizados (Definiciones, Teoremas, …)**
2. **Comandos personalizados clasificados según su utilidad**
    - 2.1 Alfabeto griego
    - 2.2 Lógica
    - 2.3 Términos conjuntistas
    - 2.4 Funciones
    - 2.5 Operadores
    - 2.6 Espacios y medidas
    - 2.7 Distribuciones estadísticas
    - 2.8 Escritura de ecuaciones y cálculos extensos
    - 2.9 Utilidades extra (formateo de texto, fuentes, entornos, experimentos)

---
### 3.1. Entornos personalizados (Definiciones, Teoremas, etc.)

Estos snippets sirven para insertar entornos (defn, thm, lem,…) que tu plantilla de apuntes reconozca, evitando tener que escribir `\begin{…}` y demás manualmente. Cada uno de ellos usa `${1}`, `${2}`, etc., para que completes título, contenido y, en algunos casos, demostración.
```snippets
# ---------------------------------------------------------------------------
# Entornos personalizados (Definiciones, Teoremas, etc.)
# Nota: funcionan correctamente dentro de la plantilla principal de Apuntes
# ---------------------------------------------------------------------------

snippet defn "Entorno Definición" b
    \\defn{${1:Título}}{${2:Contenido de la definición}}
    ${0}
#

snippet thm "Entorno Teorema (sin demo)" b
    \\thm{${1:Título}}{${2:Enunciado del teorema}}
    ${0}
#
snippet thmp "Entorno Teorema completo" b
    \\thmp{${1:Título}}{${2:Enunciado del teorema}}{${3:Demostración}}
    ${0}
#

snippet lem "Entorno Lema (sin demo)" b
    \\lem{${1:Título}}{${2:Enunciado del lema}}
    ${0}
#
snippet lemp "Entorno Lema completo" b
    \\lemp{${1:Título}}{${2:Enunciado del lema}}{${3:Demostración}}
    ${0}
#

snippet cor "Entorno Corolario (sin demo)" b
    \\cor{${1:Enunciado del corolario}}
    ${0}
#
snippet corp "Entorno Corolario completo" b
    \\corp{${1:Enunciado del corolario}}{${2:Demostración}}
    ${0}
#

snippet prop "Entorno Proposición (sin demo)" b
    \\prop{${1:Enunciado de la proposición}}
    ${0}
#
snippet propp "Entorno Proposición completo" b
    \\propp{${1:Enunciado de la proposición}}{${2:Demostración}}
    ${0}
#

snippet clm "Entorno Claim (sin demo)" b
    \\clm{${1:Enunciado de la claim}}
    ${0}
#
snippet clmp "Entorno Claim completo" b
    \\clmp{${1:Enunciado de la claim}}{${2:Demostración}}
    ${0}
#

snippet pf "Entorno Demostración" b
    \\pf{${1:Contenido de la demostración}}
    ${0}
#

snippet ex "Entorno Ejemplo simple" b
    \\ex{${1:Título}}{${2:Subtítulo}}{${3:Contenido del ejemplo}}
    ${0}
#

snippet vex "Varios ejemplos" b
    \\Vex{${1:Título}}{${2:Subtítulo}}{
        \\begin{itemize}
            \\item ${3:Contenido del ejemplo}
        \\end{itemize}
    }
    ${0}
#

snippet rmk "Entorno Observación (normal)" b
    \\rmk{${1:Contenido de la observación}}
    ${0}
#
snippet rmkb "Entorno Observación (barra)" b
    \\rmkb{${1:Contenido de la observación}}
    ${0}
#

snippet pblm "Entorno Problema" b
    \\pblm{${1:Título del problema}}{${2:Enunciado del problema}}
    ${0}
#
snippet pblms "Entorno Problema con Solución" b
    \\pblmS{${1:Título del problema}}{${2:Enunciado del problema}}{${3:Solución}}
    ${0}
#

# Fin de Entornos personalizados

```

- Todos estos snippets usan el flag `b` para que solo se expandan al comienzo de una línea, evitando conflictos si escribes, p.ej., `subset` en medio de un texto. 
- La sintaxis de la macro (`\defn{Título}{Contenido}`) asume que tu plantilla global de Apuntes define los comandos `\defn`, `\thm`, etc.

---

### 3.2. Comandos personalizados clasificados según su utilidad

#### 3.2.1. Alfabeto griego

Para no tener que escribir `\alpha`, `\beta`, `\Gamma`, etc., cada letra griega se define con un snippet muy corto (`w` = word boundary). Así, al teclear `al<tab>` se expande a `\alpha` al instante.
```snippets
# ---------------------------------------------------------------------------
# 1. Alfabeto griego
# ---------------------------------------------------------------------------

# Letras griegas mayúsculas
snippet Al "Alfa mayúscula" w
    \\Alpha
#
snippet Be "Beta mayúscula" w
    \\Beta
#
snippet Gam "Gamma mayúscula" w
    \\Gamma
#
snippet Del "Delta mayúscula" w
    \\Delta
#
snippet Eps "Epsilon mayúscula" w
    \\Epsilon
#
snippet Zet "Zeta mayúscula" w
    \\Zeta
#
snippet Eta "Eta mayúscula" w
    \\Eta
#
snippet The "Theta mayúscula" w
    \\Theta
#
snippet Iot "Iota mayúscula" w
    \\Iota
#
snippet Kap "Kappa mayúscula" w
    \\Kappa
#
snippet Lamb "Lambda mayúscula" w
    \\Lambda
#
snippet Mu "Mu mayúscula" w
    \\Mu
#
snippet Nu "Nu mayúscula" w
    \\Nu
#
snippet Xi "Xi mayúscula" w
    \\Xi
#
snippet Omi "Omicron mayúscula" w
    \\Omicron
#
snippet Pi "Pi mayúscula" w
    \\Pi
#
snippet Rho "Rho mayúscula" w
    \\Rho
#
snippet Si "Sigma mayúscula" w
    \\Sigma
#
snippet Tau "Tau mayúscula" w
    \\Tau
#
snippet Ups "Upsilon mayúscula" w
    \\Upsilon
#
snippet Phi "Phi mayúscula" w
    \\Phi
#
snippet Chi "Chi mayúscula" w
    \\Chi
#
snippet Psi "Psi mayúscula" w
    \\Psi
#
snippet Om "Omega mayúscula" w
    \\Omega
#

# Letras griegas minúsculas
snippet al "alfa minúscula" w
    \\alpha
#
snippet be "beta minúscula" w
    \\beta
#
snippet gam "gamma minúscula" w
    \\gamma
#
snippet del "delta minúscula" w
    \\delta
#
snippet eps "epsilon minúscula" w
    \\epsilon
#
snippet zet "zeta minúscula" w
    \\zeta
#
snippet eta "eta minúscula" w
    \\eta
#
snippet the "theta minúscula" w
    \\theta
#
snippet iota "iota minúscula" w
    \\iota
#
snippet kap "kappa minúscula" w
    \\kappa
#
snippet lam "lambda minúscula" w
    \\lambda
#
snippet mu "mu minúscula" w
    \\mu
#
snippet nu "nu minúscula" w
    \\nu
#
snippet xi "xi minúscula" w
    \\xi
#
snippet omi "omicron minúscula" w
    \\omicron
#
snippet pi "pi minúscula" w
    \\pi
#
snippet rho "rho minúscula" w
    \\rho
#
snippet si "sigma minúscula" w
    \\sigma
#
snippet tau "tau minúscula" w
    \\tau
#
snippet ups "upsilon minúscula" w
    \\upsilon
#
snippet phi "phi minúscula" w
    \\phi
#
snippet chi "chi minúscula" w
    \\chi
#
snippet psi "psi minúscula" w
    \\psi
#
snippet ome "omega minúscula" w
    \\omega
#

# Fin de Alfabeto griego

```

- Usamos `w` para que solo se expandan tras un espacio o al final de palabra.

---

#### 3.2.2. Lógica

Snippets para demostrar implicaciones de manera inmediata.
```snippets
# ---------------------------------------------------------------------------
# 1. Alfabeto griego
# ---------------------------------------------------------------------------

# Letras griegas mayúsculas
snippet Al "Alfa mayúscula" w
    \\Alpha
#
snippet Be "Beta mayúscula" w
    \\Beta
#
snippet Gam "Gamma mayúscula" w
    \\Gamma
#
snippet Del "Delta mayúscula" w
    \\Delta
#
snippet Eps "Epsilon mayúscula" w
    \\Epsilon
#
snippet Zet "Zeta mayúscula" w
    \\Zeta
#
snippet Eta "Eta mayúscula" w
    \\Eta
#
snippet The "Theta mayúscula" w
    \\Theta
#
snippet Iot "Iota mayúscula" w
    \\Iota
#
snippet Kap "Kappa mayúscula" w
    \\Kappa
#
snippet Lamb "Lambda mayúscula" w
    \\Lambda
#
snippet Mu "Mu mayúscula" w
    \\Mu
#
snippet Nu "Nu mayúscula" w
    \\Nu
#
snippet Xi "Xi mayúscula" w
    \\Xi
#
snippet Omi "Omicron mayúscula" w
    \\Omicron
#
snippet Pi "Pi mayúscula" w
    \\Pi
#
snippet Rho "Rho mayúscula" w
    \\Rho
#
snippet Si "Sigma mayúscula" w
    \\Sigma
#
snippet Tau "Tau mayúscula" w
    \\Tau
#
snippet Ups "Upsilon mayúscula" w
    \\Upsilon
#
snippet Phi "Phi mayúscula" w
    \\Phi
#
snippet Chi "Chi mayúscula" w
    \\Chi
#
snippet Psi "Psi mayúscula" w
    \\Psi
#
snippet Om "Omega mayúscula" w
    \\Omega
#

# Letras griegas minúsculas
snippet al "alfa minúscula" w
    \\alpha
#
snippet be "beta minúscula" w
    \\beta
#
snippet gam "gamma minúscula" w
    \\gamma
#
snippet del "delta minúscula" w
    \\delta
#
snippet eps "epsilon minúscula" w
    \\epsilon
#
snippet zet "zeta minúscula" w
    \\zeta
#
snippet eta "eta minúscula" w
    \\eta
#
snippet the "theta minúscula" w
    \\theta
#
snippet iota "iota minúscula" w
    \\iota
#
snippet kap "kappa minúscula" w
    \\kappa
#
snippet lam "lambda minúscula" w
    \\lambda
#
snippet mu "mu minúscula" w
    \\mu
#
snippet nu "nu minúscula" w
    \\nu
#
snippet xi "xi minúscula" w
    \\xi
#
snippet omi "omicron minúscula" w
    \\omicron
#
snippet pi "pi minúscula" w
    \\pi
#
snippet rho "rho minúscula" w
    \\rho
#
snippet si "sigma minúscula" w
    \\sigma
#
snippet tau "tau minúscula" w
    \\tau
#
snippet ups "upsilon minúscula" w
    \\upsilon
#
snippet phi "phi minúscula" w
    \\phi
#
snippet chi "chi minúscula" w
    \\chi
#
snippet psi "psi minúscula" w
    \\psi
#
snippet ome "omega minúscula" w
    \\omega
#

# Fin de Alfabeto griego

```
- El flag `w` asegura que solo se expandan en el límite de palabra. 

---

#### 3.2.3. Términos conjuntistas

Símbolos básicos de teoría de conjuntos, y un par de entornos para grafos.
```snippets
# ---------------------------------------------------------------------------
# 3. Términos Conjuntistas
# ---------------------------------------------------------------------------

snippet C "Subconjunto ⊂" w
    $\\subset$${0}
#
snippet Cm "Subconjunto (texto crudo)" w
    \\subset
    ${0}
#

snippet n "Intersección ∩" w
    $\\cap$${0}
#
snippet nm "Intersección (texto crudo)" w
    \\cap
    ${0}
#

snippet graf "Entorno Grafo (personalizado)" b
    \\graf{${1:Vértices}}{${2:Aristas}}
    ${0}
#

# Fin de Términos conjuntistas

```

- El snippet `graf` asume que tu plantilla define `\graf{Vértices}{Aristas}`.    

---

#### 3.2.4. Funciones

Estructuras frecuentes para definir funciones en matemáticas (incluye dominio, codominio, restricción, etc.)
```snippets
# ---------------------------------------------------------------------------
# 4. Funciones
# ---------------------------------------------------------------------------

snippet fn "Definición de función f" b
    \\fn{${1:f}}{${2:dominio}}{${3:codominio}}
    ${0}
#
snippet fXtoY "f : X → Y" b
    \\fXtoY{${1:f}}{${2:X}}{${3:Y}}
    ${0}
#
snippet ind "Indicador de conjunto" b
    \\nbb{1}_{${1:Conjunto}}{(${2:Elemento})}
    ${0}
#
snippet restr "Restricción de función f|_A" b
    \\restr{${1:f}}{${2:A}}
    ${0}
#

# Fin de Funciones

```

- Todos usan `b` para expandirse al inicio de línea.

---

#### 3.2.5. Operadores

Sumatorias y productorias con sus límites.
```snippets
# ---------------------------------------------------------------------------
# 5. Operadores
# ---------------------------------------------------------------------------

snippet sum "Sumatoria ∑" b
    \\sum_{${1:índice}}^{${2:tope}}{${3:expresión}}
    ${0}
#
snippet prod "Productoria ∏" b
    \\prod_{${1:índice}}^{${2:tope}}{${3:expresión}}
    ${0}
#

# Fin de Operadores

```

- Para cálculos rápidos, basta con teclear `sum<tab>` y completar.

---

#### 3.2.6. Espacios y medidas

Atajos para ℝ, ℝⁿ, ℕ, ℤ, espacios de probabilidad, medidas y conjuntos Borelianos.
```snippets
# ---------------------------------------------------------------------------
# 6. Espacios y medidas
# ---------------------------------------------------------------------------

# Números reales
snippet R "Espacio ℝ" w
    \\mathbb{R}
#
# ℝ^n
snippet Rn "Espacio ℝ^n" w
    \\mathbb{R}^{${1:n}}
#
# ℝ_{>0}
snippet Rp "ℝ positivo" w
    \\mathbb{R}_{>0}
#
# ℝ_{≥0}
snippet Rnn "ℝ no-negativo" w
    \\mathbb{R}_{\\geq 0}
#

# Números naturales
snippet N "Espacio ℕ" w
    \\mathbb{N}
#
# Números enteros
snippet Z "Espacio ℤ" w
    \\mathbb{Z}
#
snippet Zp "ℤ_{>0}" w
    \\mathbb{Z}_{>0}
#
snippet Znn "ℤ_{≥0}" w
    \\mathbb{Z}_{\\geq 0}
#

# Números racionales
snippet Q "Espacio ℚ" w
    \\mathbb{Q}
#

# Números complejos
snippet Ci "Espacio ℂ" w
    \\mathbb{C}
#

# Números cuaterniones
snippet H "Espacio ℍ" w
    \\mathbb{H}
#

# Números p-ádicos
snippet Qp "Espacio ℚ_p" w
    \\mathbb{Q}_p
#

# Probabilidad de un evento: 𝖯(Evento)
snippet proba "Probabilidad de un evento" b
    \\mathbb{P}\\left(${1:Evento}\\right)
    ${0}
#
snippet probap "Probabilidad con subíndice" b
    \\mathbb{P}_{${1:Parámetro}}\\left(${2:Evento}\\right)
    ${0}
#

# Esperanza
snippet Exv "Esperanza 𝖤(Variable)" b
    \\mathbb{E}\\left(${1:Variable}\\right)
    ${0}
#
snippet Expp "Esperanza con subíndice" b
    \\mathbb{E}_{${1:Parámetro}}\\left(${2:Variable}\\right)
    ${0}
#

# Espacio de probabilidad general
snippet esP "Espacio de probabilidad (Ω, F, P)" w
    (\\Omega,\\mathcal{F},\\mathbb{P})
#
snippet esPi "Espacio de probabilidad particular" b
    (${1:Ω},${2:\\mathcal{F}},${3:\\mathbb{P}})
    ${0}
#

# Espacios Borel:
snippet BorR "Borel en ℝ" w
    \\left(\\mathbb{R},\\mathcal{B}(\\mathbb{R})\\right)
#
snippet BorRn "Borel en ℝⁿ" w
    \\left(\\mathbb{R}^n,\\mathcal{B}(\\mathbb{R}^n)\\right)
#
snippet BorX "Borel en espacio X" w
    \\left(${1:Espacio},\\mathcal{B}\\left(${1:Espacio}\\right)\\right)
#

# Fin de Espacios y medidas

```

- Nuevamente, `w` para la mayoría de ellos para uso en medio de texto.

---

#### 3.2.7. Distribuciones estadísticas

Para anotar rápidamente distribuciones: uniforme, binomial, normal, etc.
```snippets
# ---------------------------------------------------------------------------
# 7. Distribuciones Estadísticas
# ---------------------------------------------------------------------------

snippet unif "X ∼ Uniforme(a, b)" b
    ${1:X} \\sim Uniforme(${2:a}, ${3:b})
    ${0}
#

snippet bern "X ∼ Bernoulli(p)" b
    ${1:X} \\sim Bernoulli(${2:p})
    ${0}
#

snippet bin "X ∼ Binomial(n, p)" b
    ${1:X} \\sim Binomial(${2:n}, ${3:p})
    ${0}
#

snippet geom "X ∼ Geométrica(p)" b
    ${1:X} \\sim Geométrica(${2:p})
    ${0}
#

snippet expo "X ∼ Exponencial(λ)" b
    ${1:X} \\sim Exponencial(${2:\\lambda})
    ${0}
#

snippet pois "X ∼ Poisson(λ)" b
    ${1:X} \\sim Poisson(${2:\\lambda})
    ${0}
#

snippet norm "X ∼ Normal(μ, σ²)" b
    ${1:X} \\sim Normal(${2:\\mu}, ${3:\\sigma^2})
    ${0}
#

snippet tdist "X ∼ t(df)" b
    ${1:X} \\sim t(${2:df})
    ${0}
#

snippet chisq "X ∼ χ²(df)" b
    ${1:X} \\sim \\chi^2(${2:df})
    ${0}
#

snippet fdist "X ∼ F(df1, df2)" b
    ${1:X} \\sim F(${2:df1}, ${3:df2})
    ${0}
#

snippet beta "X ∼ Beta(α, β)" b
    ${1:X} \\sim Beta(${2:\\alpha}, ${3:\\beta})
    ${0}
#

snippet gamma "X ∼ Gamma(shape, rate)" b
    ${1:X} \\sim Gamma(${2:shape}, ${3:rate})
    ${0}
#

snippet lognorm "X ∼ Log-Normal(μ, σ²)" b
    ${1:X} \\sim LogNormal(${2:\\mu}, ${3:\\sigma^2})
    ${0}
#

snippet weib "X ∼ Weibull(shape, scale)" b
    ${1:X} \\sim Weibull(${2:shape}, ${3:scale})
    ${0}
#

snippet multinom "X ∼ Multinomial(n, p)" b
    ${1:X} \\sim Multinomial(${2:n}, ${3:p})
    ${0}
#

snippet dirich "X ∼ Dirichlet(α)" b
    ${1:X} \\sim Dirichlet(${2:\\alpha})
    ${0}
#

snippet pareto "X ∼ Pareto(scale, shape)" b
    ${1:X} \\sim Pareto(${2:scale}, ${3:shape})
    ${0}
#

snippet cauchy "X ∼ Cauchy(x₀, γ)" b
    ${1:X} \\sim Cauchy(${2:x_0}, ${3:\\gamma})
    ${0}
#

snippet negbin "X ∼ NegBin(r, p)" b
    ${1:X} \\sim NegBin(${2:r}, ${3:p})
    ${0}
#

snippet hypergeom "X ∼ Hipergeométrica(N, K, n)" b
    ${1:X} \\sim Hipergeométrica(${2:N}, ${3:K}, ${4:n})
    ${0}
#

# Fin de Distribuciones

```

- Cada snippet usa el flag `b` para expandirse al inicio de línea y que no entre en conflicto con texto cercano.

---

#### 3.2.8. Escritura de ecuaciones y cálculos extensos

En esta categoría se agrupan calculaciones comunes: integrales, derivadas, límites, matrices, sistemas, sumas infinitas, productos infinitos y, sobre todo, atajos para cálculos largos.
```snippets
# ---------------------------------------------------------------------------
# 8. Escritura de ecuaciones y cálculos extensos
# ---------------------------------------------------------------------------

snippet intparts "Integración por partes" b
    \\int ${1:u} \\, ${2:dv} = ${1:u} \\cdot ${3:v} - \\int ${3:v} \\, ${4:du}
    ${0}
#

snippet partial "Derivada parcial ∂f/∂x" w
    \\frac{\\partial ${1:f}}{\\partial ${2:x}}
    ${0}
#

snippet total "Derivada total df/dx" w
    \\frac{d ${1:f}}{d ${2:x}}
    ${0}
#

snippet lim "Límite limₓ→a f(x)" w
    \\lim_{${1:x} \\to ${2:a}} ${3:f(x)}
    ${0}
#

snippet suminf "Suma infinita ∑_{n=0}^∞ aₙ" b
    \\sum_{${1:n}=${2:0}}^{\\infty} ${3:a_n}
    ${0}
#

snippet prodinf "Producto infinito ∏_{n=0}^∞ aₙ" b
    \\prod_{${1:n}=${2:0}}^{\\infty} ${3:a_n}
    ${0}
#

snippet intdef "Integral definida ∫_a^b f(x) dx" b
    \\int_{${1:a}}^{${2:b}} ${3:f(x)} \\, d${4:x}
    ${0}
#

snippet intindef "Integral indefinida ∫ f(x) dx" b
    \\int ${1:f(x)} \\, d${2:x}
    ${0}
#

snippet matrix "Matriz 2×2 (bmatrix)" b
    \\begin{bmatrix}
        ${1:a} & ${2:b} \\\\
        ${3:c} & ${4:d}
    \\end{bmatrix}
    ${0}
#

snippet system "Sistema de ecuaciones (cases)" b
    \\begin{cases}
        ${1:ecuación 1} \\\\
        ${2:ecuación 2}
    \\end{cases}
    ${0}
#

# Fracción simple: "\\frac{a}{b}"
snippet // "Fracción básica" wi
    \\frac{${1}}{${2}}${0}
#
# Fracción avanzada con regex para expresiones simples seguidas de "/" 
snippet '((\d+)|(\d*)(\\)?([A-Za-z]+)((\^|_)(\{\d+\}|\d))*)/' "Fracción regex" wr
    \\frac{`!p snip.rv = match.group(1)`}{${1}}${0}
#

# Fracción con selección visual:
snippet / "Fracción con selección visual" wi
    \\frac{${VISUAL}}{${1}}${0}
#

snippet sqrt "Raíz cuadrada √{…}" w
    \\sqrt{${1:Expresión}}
    ${0}
#

snippet nroot "Raíz n-ésima ∛, ∜, etc." w
    \\sqrt[${1:n}]{${2:Expresión}}
    ${0}
#

# Fin de Escritura de ecuaciones

```

- El trigger `//` (flag `wi` = word boundary + interior) genera `\frac{}{}` y coloca cursor en el numerador.    
- El regex en el segundo snippet reconoce expresiones simples (`3/`, `x^2/`, etc.) para convertirlas en `\frac{…}{…}` automáticamente.
- El fragmento visual (`/` en modo visual) usa `${VISUAL}` para envolver la selección en el numerador de la fracción.
- Para integrales y derivadas, basta con teclear `intdef<tab>`, `partial<tab>`, etc.

---

#### 3.2.9. Utilidades extra

Incluye snippets para formato de texto (negrita, cursiva, subrayado), fuentes matemáticas (`\mathcal`, `\mathfrak`, etc.), entornos extra (figuras, align, enumerate), y “experimentos” como modo matemático inline/display, subíndices y superíndices automáticos.
```snippets
# ---------------------------------------------------------------------------
# 9. Utilidades extra
# ---------------------------------------------------------------------------

# 9.1. Formateo de texto
snippet bf "Texto en negrita \\textbf{…}" wi
    \\textbf{${1:Texto}}${0}
#
snippet it "Texto en cursiva \\textit{…}" wi
    \\textit{${1:Texto}}${0}
#
snippet ul "Texto subrayado \\underline{…}" w
    \\underline{${1:Texto}}${0}
#
snippet tt "Texto teletipo \\texttt{…}" wi
    \\texttt{${1:Texto}}${0}
#
snippet sc "Small Caps \\textsc{…}" w
    \\textsc{${1:Texto}}${0}
#
snippet tsub "Texto subíndice \\textsubscript{…}" i
    \\textsubscript{${1:Texto}}${0}
#
snippet tsup "Texto superíndice \\textsuperscript{…}" i
    \\textsuperscript{${1:Texto}}${0}
#

# 9.2. Fuentes matemáticas
snippet mf "Mathfrak \\mathfrak{…}" w
    \\mathfrak{${1:Letra}}${0}
#
snippet mc "Mathcal \\mathcal{…}" w
    \\mathcal{${1:Letra}}${0}
#
snippet ms "Mathscr \\mathscr{…}" w
    \\mathscr{${1:Letra}}${0}
#

snippet aref "Referencia en negrita \\aref{…}" w
    \\textbf{\\aref{${1:Etiqueta}}}${0}
#
snippet bref "Referencia en negrita \\ref{…}" w
    \\textbf{\\ref{${1:Etiqueta}}}${0}
#
snippet eref "Referencia doble \\eref{…}{…}" w
    \\eref{${1:Etiqueta}}{${2:Texto}}${0}
#

# 9.3. Entornos extra
snippet fig "Entorno Figura" b
    \\begin{figure}
      \\begin{center}
        \\includegraphics[scale=${1:0.5}]{${2:Ruta/imagen}}
      \\end{center}
      \\caption{${3:Texto de la figura}}
      \\label{fig:${4:etiqueta}}
    \\end{figure}
    ${0}
#

snippet bsal "Entorno align (alineado sin numeración)" b
    \\begin{align*}
      ${1:Ecuaciones}
    \\end{align*}
    ${0}
#

snippet enum "Entorno enumerate" b
    \\begin{enumerate}
      \\item ${1:Ítem}
    \\end{enumerate}
    ${0}
#
snippet i "Ítem (dentro de enumerate o itemize)" i
    \\item ${0}
#

snippet case "Entorno cases" b
    ${1:Expresión}=
    \\begin{cases}
      ${2:Valor}, &\\text{ si } ${3:Caso}\\\\
      ${4:Valor}, &\\text{ en otro caso}
    \\end{cases}
    ${0}
#

# 9.4. Experimentos (modo matemático, sub/suo)
snippet mk "Inline Math $…$" wA
    $${1}$`!p
      if t[2] and t[2][0] not in [',', '.', '?', '-', ' ']:
        snip.rv = ' '
      else:
        snip.rv = ''
    `$2
    ${0}
#

snippet dm "Display Math \\[ … \\]" wA
    \\[
      ${1:Ecuación}
    .\\]${0}
#

# Subíndices y superíndices automáticos con regex
snippet '([A-Za-z])(\d)' "Auto subíndice" wrA
    `!p snip.rv = match.group(1)`_`!p snip.rv = match.group(2)`
    ${0}
#
snippet '([A-Za-z])_(\d\d)' "Subíndice con 2 dígitos" wrA
    `!p snip.rv = match.group(1)`_{`!p snip.rv = match.group(2)`}
    ${0}
#

snippet sr "^2" iA
    ^2
    ${0}
#
snippet cb "^3" iA
    ^3
    ${0}
#
snippet compl "Complemento ^{c}" iA
    ^{c}
    ${0}
#
snippet td "Superíndice general ^{…}" iA
    ^{${1:Texto}}${0}
#

# Fin de Utilidades extra

```

- Observe que muchos triggers llevan `A` (autoexpand). Por ejemplo, al teclear `dm` al inicio de palabra, se expande sin necesidad de presionar `<Tab>`.

---

## 4. Buenas prácticas y recomendaciones

1. **Orden y clasificación**
    - Mantén la clasificación tal como se muestra: primero entornos, luego símbolos clasificados por utilidad. Esto facilita la búsqueda de snippets y su mantenimiento a lo largo del tiempo.
        
2. **Descripciones claras**
    - Incluye siempre una descripción breve entre comillas en la línea `snippet`. Sirve para reconocerte rápidamente qué hace cada snippet cuando LuaSnips muestra sugerencias o cuando uses plugins de autocompletado.
        
3. **Uso de flags adecuados**
    - `b` (beginning): si quieres snippet solo al inicio de línea (muy útil para entornos).
    - `w` (word boundary): para símbolos o expresiones que pueden estar en medio de texto.
    - `i` (inner): para triggers en cualquier posición.
    - `r` (regex): cuando el trigger es una expresión regular (p.ej. subíndices automáticos).
    - `A` (autoexpand): para snippets que quieras expandir inmediatamente sin presionar `<Tab>`.  
        Ajusta estos flags según la frecuencia de uso y posición habitual donde escribirás el trigger.
        
4. **Prueba incremental**
    - Una vez guardes `tex.snippets`, abre un archivo `.tex` en NeoVim y teclea el trigger seguido de `<Tab>` para comprobar que el snippet se expande correctamente. Si no lo hace, revisa:
        - Que `filetype=tex` o `tex.latex`.
        - Que LuaSnips cargue la ruta correcta (`:LuaSnipListSnippets` te muestra los snippets cargados).
        - Flags y sintaxis (fíjate que no falte el espacio o tab justo después de `snippet`).
            
5. **Selección visual**
    - Para fracciones con selección (`/${VISUAL}`), primero entra en modo visual (`v`), selecciona la expresión, luego teclea `/` y se expandirá la fracción con la selección en el numerador. Es ideal para casos donde quieres envolver bloques enteros en una fracción sin teclear mucho.
        
6. **Refactorización constante**
    - A medida que avances en tus apuntes, revisa periódicamente tus snippets:
        - Elimina triggers que no uses.
        - Ajusta placeholders para acelerar la entrada de parámetros comunes (p. ej. usa siempre `${1:variable}` para indicar nombres de variables, y así recuerdas rápidamente qué colocar).
        - Si detectas patrones repetitivos nuevos, crea snippets ad hoc para ellos.
            

---

## 5. Ejemplo de uso en flujo de trabajo (basado en Castel)

1. **Preparar la plantilla principal**
    - Define un archivo `plantilla_apuntes.tex` donde importes paquetes esenciales (`amsmath`, `amsthm`, `mathtools`, etc.) y declares macros como `\defn`, `\thm`, `\lem`, etc., junto con el estilo de sección, encabezados y pie de página.
        
2. **Crear un nuevo archivo de apuntes**
```tex
\documentclass[12pt]{article}
\input{plantilla_apuntes.tex}

\begin{document}
\section*{Apuntes de Matemáticas}
\subsection*{Clase del 10 de junio de 2025}

% Aquí empiezas a tomar notas...
```
- Abre en NeoVim: `nvim apuntes_clase_2025-06-10.tex` y posiciona tu cursor dentro del entorno `document`.

3. **Tomar apuntes al dictado**
	- Para escribir “Definición” rápidamente:
		- Tecleas `defn` + `<Tab>` →
	```tex
	\defn{<cursor en Título>}{<cursor en Contenido>}
	```            
		-  Pulsas `<Tab>` para completar el contenido.
	
	- Para insertar la letra griega “α” en línea:      
	    - Escribes `al` + `<Tab>` → `\alpha`.
	            
	- Para escribir una integral por partes:        
		- Tecleas `intparts` + `<Tab>` →
	```tex
	\int u \, dv = u \cdot v - \int v \, du
	```         
		y el cursor queda en “u” listo para sustituirlo por la función que corresponda.
	            
	 - Si el profesor dice “tomemos el espacio ℝⁿ”, simplemente marcas `Rn` + `<Tab>` → `\mathbb{R}^n`.
	- Para anotar un teorema con demostración:
		- Escribes `thmp` + `<Tab>` →
	```tex
	\thmp{<Título>}{<Enunciado>}{<Demostración>}
	```
		llenas los tres campos con `<Tab>`.
					
	- Si deseas una fracción rápida en medio de una ecuación:
	    - Escribas `(1+2+3)/` + `<Tab>` → `\frac{1+2+3}{}`, con cursor en el denominador.
            
4. **Compilar en tiempo real**
    - Con vimtex abierto sincronizas con Zathura (u otro visor PDF) para ver inmediatamente cómo queda la nota. De este modo, no interrumpes el flujo: cada vez que guardas (`:w`), el PDF se actualiza.
        

---

## 6. Enlaces de interés y recursos

- **Repositorio oficial de snippets “vim-snippets”** (Snipmate y otros formatos):  
    [https://github.com/honza/vim-snippets](https://github.com/honza/vim-snippets)
- **Documentación de Snipmate (formato *.snippets)**:  
    [https://github.com/msanders/snipmate.vim/blob/master/doc/SnipMate.txt](https://github.com/msanders/snipmate.vim/blob/master/doc/SnipMate.txt)
- **Blog “How I'm able to take notes in mathematics lectures using LaTeX and Vim”** (Gilles Castel):  
    [https://castel.dev/post/lecture-notes-1/](https://castel.dev/post/lecture-notes-1/)
- **LuaSnips: cargar snippets Snipmate en NeoVim**:  
    [https://github.com/L3MON4D3/LuaSnips](https://github.com/L3MON4D3/LuaSnips) (ver sección “Loading SnipMate Snippets”)
    

---

## 7. Conclusión

Con esta guía detallada tienes todo lo necesario para:

1. **Configurar NeoVim + LuaSnips** de modo que reconozca archivos Snipmate.
2. **Estructurar tu `tex.snippets`** siguiendo la clasificación de entornos, símbolos y utilidades.
3. **Diseñar snippets eficientes** para tomar apuntes de matemáticas al ritmo del profesor, sin perder tiempo estructurando manualmente definiciones, fórmulas o diagramas.
4. **Integrar buenas prácticas** extraídas del blog de Castel, asegurando que tus snippets autoexpandan correctamente y mantengan el flujo de trabajo ágil (por ej. uso de `${VISUAL}`, flags `b`,`w`,`i`,`r`,`A`).
5. **Extender y refactorizar** progresivamente tus snippets en función de nuevas necesidades (diagramas, teoremas muy especializados, dibujo rápido de figuras con `TikZ`, etc.).

Siguiendo esta plantilla y ejemplo, podrás tomar apuntes de manera fluida, con una fuerte presencia de cálculos detallados y extensos, sin detenerte a teclear símbolos repetitivos. ¡Éxito tomando notas!

---

## 8. Guía detallada para implementación de contexto (Snipmate vs. UltiSnips)

A continuación encontrarás una guía paso a paso para “rebastecer” tus snippets en formato Snipmate con funciones globales y contexto, tomando como base el archivo de UltiSnips de Gilles Castel ([https://github.com/gillescastel/latex-snippets/blob/master/tex.snippets](https://github.com/gillescastel/latex-snippets/blob/master/tex.snippets)). La idea es:

1. **Definir funciones globales en Vimscript** que establezcan “banderas de contexto” (por ejemplo: si estamos dentro de un entorno matemático, en un entorno `theorem`, en un `section`, etc.).
2. **Crear autocomandos** que actualicen dichas variables de contexto cada vez que el cursor entre o salga de los entornos relevantes.
3. **Reestructurar los snippets de UltiSnips** para Snipmate, insertando triggers y/o reglas (regex) que consulten esas variables de contexto antes de expandir.
4. **Modificar los snippets originales** (del repositorio de Castel) para que sólo se expandan cuando la función global de contexto correspondiente devuelva verdadero.

De esta forma, podrás tener un único `tex.snippets` (o bien varios archivos separados, según contexto) en donde cada “familia” de snippets se active únicamente cuando el cursor se encuentre, por ejemplo, en un ambiente `align`, en modo texto “normal”, dentro de un `theorem`, etc.

---

### 8.1. ¿Por qué necesitamos funciones globales y contexto en Snipmate?

Por defecto, Snipmate (y la mayoría de los motores que cargan archivos `*.snippets` en formato Snipmate) no distinguen “¿estoy dentro de un entorno matemático?”, ni “¿estoy dentro de un bloque `theorem`?”. Simplemente comprueban el trigger (palabra clave o regex) y el flag (`b`, `w`, etc.).

Sin embargo, cuando revisamos el repositorio de **UltiSnips** de Gilles Castel, vemos que muchos de sus snippets suponen condicionales: por ejemplo, ciertos atajos de “theorem”, “proof” o “align” sólo tienen sentido si el cursor está dentro del entorno correspondiente. En UltiSnips, es posible usar Python (`!p …`) para consultar la sintaxis del buffer, determinar si estás dentro de `\begin{theorem}...\end{theorem}`, etc. En Snipmate puro no existe esa capacidad, por lo que debemos:

1. Definir en Vimscript funciones que (al más puro estilo “small API”) comprueben el contexto (mirando el árbol de sintaxis o simplemente buscando delimitadores `\begin{…}` que te encierren).
2. Definir autocomandos para actualizarlas variables de buffer (`b:…`) cada vez que entres o salgas de entornos comunes.
3. Hacer que nuestros snippets (con triggers en Snipmate) incluyan _pon guardias_ basadas en dichas variables. Pero, dado que Snipmate NO permite condicionales dentro de la definición de un snippet, la única forma de “activar” o “desactivar” un snippet es organizando la carga de archivos o prefijando el trigger con una porción que implique “estoy en X contexto”.

En resumen:

- **Objetivo**: recrear en Snipmate la riqueza de contexto que UltiSnips ofrece con Python, usando en su lugar Vimscript + “arquitectura de archivos” + triggers regex.
- **Beneficio**: podrás tener, por ejemplo, un snippet `thm` que sólo “responda” cuando estés dentro del entorno `\begin{theorem}`; o un snippet de “fracción rápida” que sólo aparezca en modo matemático.

---

### 8.2. Estructura general y carga de snippets

Partiremos de la configuración mínima que ya tienes para Snipmate vía LuaSnips (cargando tus `.snippets` con `lazy_load`), y añadiremos una capa de Vimscript para detectar contexto y organizar nuestros archivos.

1. **Directorio de snippets**
```text
~/.config/nvim/snippets/
  ├── tex-context.vim       " Funciones y autocomandos de contexto
  ├── tex.math.snippets     " Snippets que sólo aplican en modo matemáticas
  ├── tex.text.snippets     " Snippets que aplican en modo texto “normal”
  ├── tex.theorem.snippets  " Snippets exclusivos dentro de entornos theorem/lemma/etc.
  └── tex.snippets          " Snippets “genéricos”, sin contexto especial
```

> De este modo, en `init.lua` o `init.vim` sólo cargas a LuaSnips todo el directorio, pero mantenemos separados los fragmentos según el contexto.
    
2. **Configuración en `init.lua`**
```lua
-- (Ejemplo usando packer.nvim)
use {
  "L3MON4D3/LuaSnips",
  config = function()
    -- Cargamos todos los archivos .snippets en ~/.config/nvim/snippets
    require("luasnip.loaders.from_snipmate").lazy_load({ paths = "~/.config/nvim/snippets" })
  end
}
```   
   > Asegúrate de que tu `filetype=tex` o `latex` esté correctamente detectado para que LuaSnips lea `tex.*.snippets`.
    
3. **`tex-context.vim`: funciones globales + autocomandos**  
    En este archivo definiremos todas las “banderas de contexto” (variables de buffer o globales) y autocomandos para actualizarlas. Por ejemplo, vamos a detectar si el cursor está dentro de un entorno matemático (inline o display), o dentro de `\begin{theorem}`…`\end{theorem}`.
```vim
" ------------------------------------------------------------
" Archivo: ~/.config/nvim/snippets/tex-context.vim
" ------------------------------------------------------------

" 1) Función para saber si estamos en modo matemático (inline o display).
"    Esto se basa en la sintaxis de Vim (la información de synID).
function! TexInMathMode() abort
  " Obtenemos el grupo de sintaxis en la posición actual del cursor
  let l:group = synIDattr(synID(line("."), col("."), 1), "name")
  " Muchos ftplugins para TeX ponen “texMathZone” o “texMathZoneX” como grupo
  return l:group =~? 'texMathZone'
endfunction

" 2) Función para detectar si estamos dentro de un entorno LaTeX dado.
"    Por ejemplo, entorno = 'theorem' o 'lemma'.
function! TexInEnv(env) abort
  " Buscamos hacia atrás un \begin{env}, y hacia adelante un \end{env}.
  " Si existe un \begin{env} más cercano por encima que un \end{env}, entonces
  " estamos “dentro” de ese entorno.
  let l:ln = line(".")
  let l:before = search('\\v\\c\\begin\{' . a:env . '\}', 'bnW')
  let l:after  = search('\\v\\c\\end\{'   . a:env . '\}', 'nW')
  " Si no hay \begin{} ni \end{}, search() devuelve 0.
  if l:before == 0
    return 0
  endif
  " Si no hay \end{env}, o el \begin{} está más abajo que el \end{}, entonces
  " no estamos dentro.
  if l:after > 0 && l:after < l:before
    return 0
  endif
  return 1
endfunction

" 3) Variables de buffer que almacenan estado de contexto
"    (0 = fuera, 1 = dentro)
augroup TexContextUpdater
  autocmd!
  " Cada vez que movemos el cursor en un buffer ‘tex’, actualizamos banderas
  autocmd CursorMoved,CursorMovedI *.tex call s:UpdateTexContext()
augroup END

function! s:UpdateTexContext() abort
  " Sólo aplica si buffer es tipo tex/latex
  if &filetype !~? 'tex'
    return
  endif
  " Actualizamos variables de buffer
  let b:in_math_mode    = TexInMathMode()
  let b:in_theorem_env = TexInEnv('theorem') || TexInEnv('lemma') \
                          || TexInEnv('proposition') || TexInEnv('corollary')
  let b:in_proof_env   = TexInEnv('proof')
  " (Puedes ampliar con entornos ‘align’, ‘equation’, etc., si lo deseas)
  let b:in_align_env   = TexInEnv('align') || TexInEnv('align*') \
                          || TexInEnv('equation') || TexInEnv('equation*')
endfunction

```

   **¿Qué logramos con esto?**    
- **`TexInMathMode()`**: Devuelve `1` si el cursor está en zonas marcadas como `texMathZone…` (grupo de sintaxis TeX), es decir, dentro de `$…$`, `\[…\]`, `$$…$$`, etc.
- **`TexInEnv(env)`**: Busca el par más cercano `\begin{env} … \end{env}`, y devuelve `1` si el cursor está “entre” ellos.
- **Autocomando `CursorMoved`**: Cada vez que cambias de posición, las variables de buffer `b:in_math_mode`, `b:in_theorem_env`, `b:in_proof_env` y `b:in_align_env` se actualizan.
   Estas variables son accesibles en Vimscript (e incluso podríamos leerlas dentro de un snippet Snipmate _siempre que usemos condicionales en Vimscript al momento de cargar/snippetizar_), o bien las utilizaremos para **organizar la carga de archivos** y **prefijar triggers** que dependan de ellas.
    

---

### 8.3. Cómo “activar” o “desactivar” snippets según contexto

Dado que Snipmate **NO** permite poner un condicional (“if b:in_math_mode == 1 then este snippet; else nada”), la estrategia consiste en:

1. **Separar los snippets en archivos distintos** (por ejemplo, `tex.math.snippets` sólo para aquello que deba existir en modo matemático; `tex.theorem.snippets` sólo para entornos theorem/proposition; etc.).
2. **Renombrar los triggers** de modo que incluyan un prefijo o símbolo que indique “sólo si estoy en matemáticas” o “sólo si estoy en un theorem”. Ejemplo:
    - En `tex.math.snippets`, todos los triggers podrían ir precedidos de `m_` (→ `m_sum`, `m_prod`, etc.), y en la guía de uso indicarás que para expandir esas macros hay que hacerlo **dentro** de `$…$` o `\[...\]`.
    - En `tex.theorem.snippets`, los triggers van como `thm_…` o simplemente `thm` (y su flag `b` (beginning) fuerza a que sólo se expandan si estamos al principio de línea).
    
3. **Autocomando que recargue fragmentos** cada vez que entres/salgas de un contexto, de modo que Snipmate “olvide” o “recuerde” ciertos archivos. Con LuaSnips es muy sencillo:
```vim
" Cada vez que b:in_math_mode cambie, recargamos sólo los snippets .math
autocmd CursorMoved,CursorMovedI *.tex call s:ReloadTexSnippets()

function! s:ReloadTexSnippets() abort
  if b:in_math_mode
    " Cargamos tex.math.snippets en primer lugar
    let g:luasnip_loaded_math = 1
    " (podrías llamar a :LuaSnipReloadSnippets(), pero para fines de Snipmate
    " basta que en tu init.lua hayas hecho lazy_load de todo el directorio,
    " y luego controlas que el buffer muestre o “vea” los snippets;*/
    " en la práctica LuaSnips recargará todo, pero te aseguras de que
    " el usuario sólo intente triggers que tengan sentido en matemáticas.
  else
    let g:luasnip_loaded_math = 0
  endif
  " De modo similar para teoremas:
  if b:in_theorem_env
    let g:luasnip_loaded_theorem = 1
  else
    let g:luasnip_loaded_theorem = 0
  endif
endfunction
```    
> Este fragmento muestra la idea; en la práctica, LuaSnips no “carga”/“descarga” condicionalmente, sino que tú debes _organizar tus triggers_ para que no choquen cuando no correspondan.
    
4. **Prefijos de trigger + convención de flags**    
    - En cada archivo `.snippets` (por ejemplo, `tex.math.snippets`) definiremos triggers “más largos” como `m_sum` o `m_frac`, en lugar de `sum` o `/`. De esa forma, aunque los snippets estén cargados, el usuario sólo los escribirá cuando sepa que está en modo matemático (por ejemplo: escribes `$ … m_sum … $` → `<Tab>` → `\sum_{…}^{…}`).
    - En `tex.theorem.snippets`, usaremos triggers como `thm`, `lem`, `cor`, `proof`. Y su flag `b` (beginning) garantiza que sólo se expandan al inicio de línea.
    

---

## 9. Traducción y adaptación de los snippets de UltiSnips

A continuación se muestra cómo “traducir” algunos fragmentos del repositorio de Gilles Castel (UltiSnips) a Snipmate, adaptándolos para que respeten las variables de contexto.

> **Nota**: A efectos de ejemplificar, no incluiremos todos los snippets originales sino únicamente las familias más representativas. Para cada sección, primero comentaremos brevemente cuál era el mecanismo en UltiSnips (sin profundizar en Python), y luego mostraremos la versión en Snipmate, con los prefijos de trigger adecuados y, cuando sea posible, un comentario que haga referencia a las variables `b:in_math_mode`, `b:in_theorem_env`, etc.

---

### 9.1. Snippets “genéricos” (modo texto normal)

Estos son atajos que **SIEMPRE** deben estar disponibles, sin importar si estamos en matemáticas o no (por ejemplo, comandos de sección, fecha, autoreference, etc.). Originalmente en UltiSnips aparecen así:
```vim 
snippet sec "Sección: \section{…}"
    \section{${1:Section Title}}
    ${0}
endsnippet

snippet sub "Subsección: \subsection{…}"
    \subsection{${1:Subsection Title}}
    ${0}
endsnippet

snippet lb "label: \label{…}"
    \label{${1:label}}${0}
endsnippet

snippet ref "ref: \ref{…}"
    \ref{${1:label}}${0}
endsnippet
```
> Estos no requieren contexto, así que en Snipmate van a `tex.text.snippets` (o `tex.snippets`) **sin prefijo**.
```snippets
# -------------------------------------------------------------------------
# 9.1 Snippets genéricos (modo texto “normal”)
# Archivo: ~/.config/nvim/snippets/tex.text.snippets
# -------------------------------------------------------------------------

snippet sec "Sección: \section{…}" b
    \\section{${1:Section Title}}
    ${0}
#

snippet sub "Subsección: \subsection{…}" b
    \\subsection{${1:Subsection Title}}
    ${0}
#

snippet lb "label: \label{…}" w
    \\label{${1:label}}${0}
#

snippet ref "ref: \ref{…}" w
    \\ref{${1:label}}${0}
#

# (Más snippets “textuales” pueden ir aquí, p.ej. date, today, etc.)
```
- **Flags**:
    - `b` (beginning) para secciones, porque normalmente escribes `\section` al inicio de una línea.
    - `w` (word boundary) para `\label` y `\ref`, que pueden ir tras texto incluido.
        

---

### 9.2. Snippets de entornos “Theorem / Lemma / Proof”

En UltiSnips, Castel define (entre otros) este bloque:
```vim
snippet thm "Theorem environment" b !p
if int(vim.eval("b:in_theorem_env")) == 1:
    snip.rv = "\\begin{theorem}[${1:Optional Name}]\n  ${2:Theorem statement.}\n\\end{theorem}$0"
else:
    snip.rv = ""
endif
endsnippet

snippet proof "Proof environment" b !p
if int(vim.eval("b:in_proof_env")) == 1:
    snip.rv = "\\begin{proof}\n  ${1:Proof goes here.}\n\\end{proof}$0"
else:
    snip.rv = ""
endif
endsnippet
```
> Lo importante es que en UltiSnips esos snippets podrían requerir `context “tex”` y/o condicionales Python para evitar expandirse dentro de entornos distintos. En Snipmate, **no podemos poner Python dentro del snippet**, así que:
> 
> 1. **Los separamos** en `tex.theorem.snippets`.
> 2. **Renombramos el trigger** (opcional) a `thm` o `thm_` (y confiamos en la convención: “sólo usarlo dentro de entornos theorem”).
> 3. **Mantenemos el flag** `b` para que sólo funcione al inicio de línea (que coincide con la sintaxis natural de un bloque theorem).
> 4. **En el manual de uso**, explicamos: “Para usar `thm`, antes asegúrate de que `b:in_theorem_env == 1`: normalmente abres un archivo nuevo, escribes `\begin{theorem}` y luego al final `\end{theorem}`. Dentro, `thm` funciona.”
>     

**Snipmate** (archivo: `~/.config/nvim/snippets/tex.theorem.snippets`)
```snippets
# -------------------------------------------------------------------------
# 9.2 Snippets: Theorem / Lemma / Proof  (sólo “contexto teorema”)
# Archivo: ~/.config/nvim/snippets/tex.theorem.snippets
# -------------------------------------------------------------------------

snippet thm "Entorno Theorem" b
    \\begin{theorem}[${1:Optional Name}]
      ${2:Theorem statement.}
    \\end{theorem}
    ${0}
#

snippet lem "Entorno Lemma" b
    \\begin{lemma}[${1:Optional Name}]
      ${2:Lemma statement.}
    \\end{lemma}
    ${0}
#

snippet pro "Entorno Proposition" b
    \\begin{proposition}[${1:Optional Name}]
      ${2:Proposition statement.}
    \\end{proposition}
    ${0}
#

snippet cor "Entorno Corollary" b
    \\begin{corollary}[${1:Optional Name}]
      ${2:Corollary statement.}
    \\end{corollary}
    ${0}
#

snippet proof "Entorno Proof" b
    \\begin{proof}
      ${1:Proof goes here.}
    \\end{proof}
    ${0}
#

# (Puedes agregar más, p.ej. remark, definition, example, etc.)
```
- **Observación**:
    - Al estar en `tex.theorem.snippets`, asumimos que el usuario sólo invoca estos triggers cuando `b:in_theorem_env == 1`.
    - Si intentas escribir `thm<tab>` fuera de un entorno theorem, Snipmate aún intentará expandirlo. Para mitigar confusiones, basta con que tu guía de usuario indique:
        > “Antes de teclear `thm<tab>`, escribe primero `\begin{theorem}` (que por efecto del highlight te deja en el entorno). Al final, cierra con `\end{theorem}`. Dentro, `thm` funciona.”

---

### 9.3. Snippets en modo matemático “inline/display”

En el repositorio de Castel, UltiSnips define cosas como:
```vim
snippet frac "Fracción \frac{…}{…}" "Expand in math mode" w !p
if snip.expandable() and vim.eval("b:in_math_zone") == "1":
    snip.rv = "\\frac{" + snip.mkfname("numerator") + "}{" + snip.mkfname("denominator") + "}$0"
else:
    snip.rv = ""
endif
endsnippet
```
> Nota: UltiSnips comprueba `b:in_math_zone` antes de devolver el contenido. En Snipmate no tenemos esa capacidad, pero implementaremos la misma lógica en dos niveles:
> 1. Separar en `tex.math.snippets`.
> 2. Renombrar el trigger para `m_frac` (o `/`), de modo que “siempre” se use dentro de `$…$` o `\[...\]`.

**Snipmate** (`~/.config/nvim/snippets/tex.math.snippets`)
```snippets
# -------------------------------------------------------------------------
# 9.3 Snippets Matemáticos (inline/display)
# Archivo: ~/.config/nvim/snippets/tex.math.snippets
# -------------------------------------------------------------------------

snippet m_frac "Fracción \frac{…}{…} (sólo maths)" wi
    \\frac{${1:numerator}}{${2:denominator}}${0}
#

snippet m_sqrt "Raíz cuadrada \sqrt{…}" wi
    \\sqrt{${1:expression}}${0}
#

snippet m_sum "Sumatoria \sum_{}^{}" b
    \\sum_{${1:i}}^{${2:n}} ${3:expression}${0}
#

snippet m_int "Integral ∫_{}^{}" b
    \\int_{${1:a}}^{${2:b}} ${3:f(x)} \\, d${4:x}${0}
#

snippet m_lim "Límite \lim_{x→a}" w
    \\lim_{${1:x} \\to ${2:a}} ${3:f(x)}${0}
#

snippet m_binom "Coeficiente binomial \binom{n}{k}" w
    \\binom{${1:n}}{${2:k}}${0}
#

# (Puedes seguir trayendo más desde tex.snippets de UltiSnips, siempre
#  precedidos por “m_” o bien anclas más cortas, según convención.)

# Ejemplo de atajo regex: subtituto de “/”  
snippet '([A-Za-z0-9\)\]])\/' "Expand slash → \\frac{}{} en maths" wr
    \\frac{`!p snip.rv = match.group(1)`}{${1:...}}${0}
#

# Ejemplo: autoexpand de ^ para superíndices (solo en math)
snippet '([A-Za-z0-9])\^' "Auto superíndice en math mode" wrA
    `!p snip.rv = match.group(1)`^{${1}}${0}
#

# Fin de tex.math.snippets
```
- **Triggers**:
    - Usamos `wi` (word boundary + “interior”) para `m_frac`, `m_sqrt`: significa que sólo se expanden en límites de palabra (asumiendo que el cursor ya está entre marcadores de math).
    - Para `m_sum`, `m_int` y otros, `b` (beginning) garantiza que suelen ir al principio de un bloque (por ejemplo, en un `align`).
    - El fragmento regex `([A-Za-z0-9\)\]])\/` detecta, en modo matemáticas, cuando terminas de escribir algo como `x/` y automáticamente lo reemplaza por `\frac{x}{}`. Aunque no hagamos chequeo explícito de `b:in_math_mode`, la convención de renombrar el trigger con “m_” y el flags `w`/`b` reduce mucho el riesgo de disparar la expansión en modo texto.

---

### 9.4. Snippets de entornos “align / equation” (sólo dentro de math)

En el UltiSnips original verás algo como:
```vim
snippet aline "alinear entorno align" b !p
if int(vim.eval("b:in_align_env")) == 1:
    snip.rv = "\\begin{align*}\n  " + snip.mkfname("equations") + "\n\\end{align*}$0"
else:
    snip.rv = ""
endif
endsnippet
```
> En Snipmate volvemos a separar en `tex.math.snippets` y renombramos el trigger, por ejemplo, a `m_align`.
```snippets
# -------------------------------------------------------------------------
# 9.4 Snippets Align / Equation (math display)
# Archivo: ~/.config/nvim/snippets/tex.math.snippets
# -------------------------------------------------------------------------

snippet m_align "Entorno align* (sólo math)" b
    \\begin{align*}
      ${1:equations}
    \\end{align*}
    ${0}
#

snippet m_eq "Entorno equation*" b
    \\begin{equation*}
      ${1:equation}
    \\end{equation*}
    ${0}
#

snippet m_case "Entorno cases (sólo math)" b
    ${1:expression} =
    \\begin{cases}
      ${2:value_1}, & \\text{si } ${3:case_1} \\\\
      ${4:value_2}, & \\text{en otro caso}
    \\end{cases}
    ${0}
#

# (Más entornos “math” pueden agregarse aquí, con prefijo “m_”)
```

---

### 9.5. Snippets específicos de entornos adicionales (Ej.: “tikzpicture”)

En el repositorio de Castel también encontrarás, en UltiSnips, atajos para `tikzpicture`. Por ejemplo:
```vim
snippet tikz "Entorno tikzpicture" b
\begin{tikzpicture}[scale=${1:1}]
  \node at (0,0) {${2:Content}};
\end{tikzpicture}
$0
endsnippet
```
> Para usarlo en Snipmate, podríamos crear un archivo `tex.tikz.snippets`, o bien incluirlo en `tex.snippets` (ya que **no** requiere estar en modo matemático), con trigger `tikz`.
```snippets
# -------------------------------------------------------------------------
# 9.5 Snippets: tikzpicture (modo texto)
# Archivo: ~/.config/nvim/snippets/tex.tikz.snippets
# -------------------------------------------------------------------------

snippet tikz "Entorno tikzpicture (tex.tikz.snippets)" b
    \\begin{tikzpicture}[scale=${1:1}]
      \\node at (0,0) {${2:Content}};
    \\end{tikzpicture}
    ${0}
#

# (Si en tu flujo de trabajo siempre dibujas en modo math,
#  podrías mover esto a tex.math.snippets y renombrar el trigger a m_tikz)
```

---

## 10. Resumen de la nueva regla de contexto

Con lo anterior, tu conjunto de archivos `.snippets` deberá verse así:
```text
~/.config/nvim/snippets/
├── tex-context.vim        "Funciones y autocomandos para b:in_math_mode, b:in_theorem_env, etc."
├── tex.text.snippets      "Snippets sin contexto especial (texto, secciones, referencias, etc.)"
├── tex.math.snippets      "Snippets con prefijo m_… que sólo tienen sentido en modo matemático"
├── tex.theorem.snippets   "Snippets con prefijo (o triggers propios) para entornos theorem/lemma/proof"
├── tex.tikz.snippets      "Snippets de tikzpicture, figure, etc."
└── tex.snippets           "Opcional: snippets genéricos que no queden claramente en ninguna categoría"
```
1. **`tex-context.vim`** se encarga de calcular las variables de buffer:
    - `b:in_math_mode`
    - `b:in_theorem_env`
    - `b:in_proof_env`
    - `b:in_align_env`  
        Estas variables **no** se leen _dentro_ de Snipmate (pues no hay condicionales), pero sirven de guía para organizar los triggers, para que el usuario se “autodirija” al subdirectorio correcto.
        
2. **`tex.text.snippets`**: aquí van los atajos de texto normal (secciones, referencias, tablas, listas, entornos no matemáticos).
    - Triggers sencillos: `sec`, `sub`, `lb`, `ref`, `fig`, etc.
    - Flags: combinaciones de `b`, `w` según corresponda.
        
3. **`tex.math.snippets`**:
    - **Prefijo obligatorio** por convención: `m_`. De esta forma, aunque el snippet esté cargado en cualquier parte, tú sólo lo invocarás cuando estés en modo matemático (por ejemplo: escribes `$ … m_sum … $` → `<Tab>` → `\sum_{…}^{…}`).
    - Snippets de fracciones (`m_frac`), raíces (`m_sqrt`), sumatorias (`m_sum`), entornos `align` (`m_align`), etc.
    - **Opcional**: podemos incluir regex triggers que detecten patrones como `(x)/` para convertir automáticamente en `\frac{x}{}`, siempre y cuando el usuario se asegure de que el cursor está dentro de `$…$` o `\[...\]`.
        
4. **`tex.theorem.snippets`**:
    - Triggers sin prefijo (o con uno mínimo, p.ej. `thm`, `lem`, `proof`), pues ya se asume que el usuario está dentro de un bloque `\begin{…}`.
    - Flags: `b` para que sólo se expandan al principio de línea.
        
5. **`tex.tikz.snippets`**:
    - Triggers como `tikz`, `pgf`, `tikzfig`, etc.
    - Flags: `b` por conveniencia.
        
6. **`tex.snippets`** (opcional):
    - Aquí puedes reunir fragmentos que no encajen estrictamente en ninguna otra categoría, o aquellos que uses muy poco.
        

---

## 11. Ejemplo concreto de “contexto reforzado” en uso

A modo de ilustración, supongamos que tienes abierto un archivo `clase2025-06-10.tex` y ya cargaste:
```text
\documentclass{article}
\usepackage{amsmath,amsthm,amssymb,graphicx}
\begin{document}

Texto normal aquí. 
% ------------------ Snippets en modo texto ------------------
% Escribamos una sección:
sec<tab>
→ \section{…}

“Ahora una ecuación en modo matemático:” 
\[
  % Al entrar en math, b:in_math_mode = 1
  % Si tecleo `m_sum<tab>` obtengo:
  m_sum <tab>
  → \sum_{i}^{n} expression
  % Con placeholders sobre “i”, “n” y “expression”.
\]

% Dentro de \begin{theorem} … \end{theorem} (al escribir \begin{theorem}, Vim resalta).
% Ahora b:in_theorem_env = 1
\begin{theorem}[Fundamental]
  % Dentro de un entorno theorem
  % Teclo `thm<tab>`
  thm <tab>
  → 
  \begin{theorem}[Optional Name]
    Theorem statement.
  \end{theorem}
 
  % Si inserto un proof:
  proof <tab>
  →
  \begin{proof}
    Proof goes here.
  \end{proof}
\end{theorem}

\end{document}
```
- **¿Por qué funciona?**
    1. Cuando el cursor está dentro de `\begin{theorem} … \end{theorem}`, la variable `b:in_theorem_env` vale `1`. Tú sabes (por convención) que ahí puedes aplicar los triggers sin prefijo (`thm`, `lem`, `proof`).
    2. Cuando el cursor está entre `\[…\]` (o `$…$`), la variable `b:in_math_mode` vale `1` y, por convención, sólo ahí usarás los triggers con prefijo `m_…`.
    3. Fuera de esos entornos interna o matemática, sólo usa los triggers de `tex.text.snippets`.
        

La clave radica en:
- **Funciones globales (Vimscript) que asignan variables**.
- **Convención de triggers-prefijos** para aislar “familias” de snippets.
- **Separación física de archivos `.snippets`** para facilitar mantenimiento.
    

---

## 12. Migración completa de los snippets de Castel

A continuación se muestra, a modo de listado resumido, cómo reescribir “en bloque” los **snippets principales** que encontrarás en [https://github.com/gillescastel/latex-snippets/blob/master/tex.snippets](https://github.com/gillescastel/latex-snippets/blob/master/tex.snippets). Cada bloque original de UltiSnips va acompañado de su equivalente en Snipmate, respetando la nueva regla de “prefijo‐contexto”.

> **IMPORTANTE**: A efectos de brevedad no incluimos _todos_ los snippets, pero el patrón se repite igualmente si quieres migrar el repositorio completo.

---
### 12.1. Entornos “Theorem / Lemma / Proposition / Corollary / Proof”
### Snipmate adaptado (archivo: tex.theorem.snippets)
```snippets
# Theorem (trigger: thm)
snippet thm "Entorno Theorem" b
    \\begin{theorem}[${1:Optional Name}]
      ${2:Theorem statement.}
    \\end{theorem}
    ${0}
#

# Lemma (trigger: lem)
snippet lem "Entorno Lemma" b
    \\begin{lemma}[${1:Optional Name}]
      ${2:Lemma statement.}
    \\end{lemma}
    ${0}
#

# Proposition (trigger: pro)
snippet pro "Entorno Proposition" b
    \\begin{proposition}[${1:Optional Name}]
      ${2:Proposition statement.}
    \\end{proposition}
    ${0}
#

# Corollary (trigger: cor)
snippet cor "Entorno Corollary" b
    \\begin{corollary}[${1:Optional Name}]
      ${2:Corollary statement.}
    \\end{corollary}
    ${0}
#

# Proof (trigger: proof)
snippet proof "Entorno Proof" b
    \\begin{proof}
      ${1:Proof goes here.}
    \\end{proof}
    ${0}
#

# Remark (trigger: rmk)
snippet rmk "Entorno Remark" b
    \\begin{remark}
      ${1:Remark text.}
    \\end{remark}
    ${0}
#

# Definition (trigger: defn)
snippet defn "Entorno Definition" b
    \\begin{definition}[${1:Optional Name}]
      ${2:Definition text.}
    \\end{definition}
    ${0}
#

# Example (trigger: exmp)
snippet exmp "Entorno Example" b
    \\begin{example}[${1:Optional Name}]
      ${2:Example text.}
    \\end{example}
    ${0}
#

# End snippet file: tex.theorem.snippets
```

---

### 12.2. Snippets matemáticos “básicos”: fracciones, sumatorias, integrales, etc.
### Snipmate adaptado (archivo: tex.math.snippets)
```snippets
# Fracción (trigger: m_frac) – modo math
snippet m_frac "Fracción \\frac{…}{…}" wi
    \\frac{${1:numerator}}{${2:denominator}}${0}
#

# Raíz cuadrada (trigger: m_sqrt)
snippet m_sqrt "Raíz cuadrada \\sqrt{…}" wi
    \\sqrt{${1:expression}}${0}
#

# Sumatoria (trigger: m_sum)
snippet m_sum "Sum \\sum_{…}^{…}" b
    \\sum_{${1:i}}^{${2:n}} ${3:expression}${0}
#

# Integrales (trigger: m_int)
snippet m_int "Integral \\int_{a}^{b} f(x) dx" b
    \\int_{${1:a}}^{${2:b}} ${3:f(x)}\\,d${4:x}${0}
#

# Límite (trigger: m_lim)
snippet m_lim "Límite \\lim_{x→a} f(x)" w
    \\lim_{${1:x} \\to ${2:a}} ${3:f(x)}${0}
#

# Binomio (trigger: m_binom)
snippet m_binom "Coef. binomial \\binom{n}{k}" w
    \\binom{${1:n}}{${2:k}}${0}
#

# Raíz n-ésima (trigger: m_nroot)
snippet m_nroot "Raíz n-ésima \\sqrt[​n]{​…}" wi
    \\sqrt[${1:n}]{${2:expression}}${0}
#

# Fin de tex.math.snippets

```

---

### 12.3. Snippets de entornos “align / equation / cases”
### Snipmate adaptado (archivo: tex.math.snippets)
```snippets
# Entorno align* (trigger: m_align)
snippet m_align "Entorno align*" b
    \\begin{align*}
      ${1:equations}
    \\end{align*}
    ${0}
#

# Entorno equation* (trigger: m_eq)
snippet m_eq "Entorno equation*" b
    \\begin{equation*}
      ${1:equation}
    \\end{equation*}
    ${0}
#

# Entorno cases (trigger: m_case)
snippet m_case "Entorno cases" b
    ${1:expression} = 
    \\begin{cases}
      ${2:value_1}, & \\text{si } ${3:case_1} \\\\
      ${4:value_2}, & \\text{en otro caso}
    \\end{cases}
    ${0}
#

# Fin de tex.math.snippets
```
---

### 12.4. Snippets “Verbales” y de formateo (texto normal)
### Snipmate adaptado (archivo: tex.text.snippets)
```snippets
# Texto en negrita (trigger: bf)
snippet bf "Texto en negrita \\textbf{…}" w
    \\textbf{${1:Texto}}${0}
#

# Texto en cursiva (trigger: it)
snippet it "Texto en cursiva \\textit{…}" w
    \\textit{${1:Texto}}${0}
#

# Subrayado (trigger: ul)
snippet ul "Texto subrayado \\underline{…}" w
    \\underline{${1:Texto}}${0}
#

# Fuente teletipo (trigger: tt)
snippet tt "Texto teletipo \\texttt{…}" w
    \\texttt{${1:Texto}}${0}
#

# Fin de tex.text.snippets
```
---

### 12.5. Snippets “Avanzados” o “Extras”

En el repositorio de Castel hay también snippets para “tabla rápida”, “enumerate”, “itemize”, etc. Por ejemplo:
```vim
snippet tab "Tabla (@tabular@)" b
\begin{tabular}{${1:c|c|c}}
  ${2:Cell1} & ${3:Cell2} & ${4:Cell3} \\\\
\end{tabular}
$0
endsnippet
```

> En Snipmate, iría en `tex.text.snippets`:

```snippets
# Tabla sencilla (trigger: tab)
snippet tab "Entorno tabular" b
    \\begin{tabular}{${1:c|c|c}}
      ${2:Cell1} & ${3:Cell2} & ${4:Cell3} \\\\
    \\end{tabular}
    ${0}
#

# Enumerate (trigger: enum)
snippet enum "Entorno enumerate" b
    \\begin{enumerate}
      \\item ${1:Ítem}
    \\end{enumerate}
    ${0}
#

# Item (trigger: it *dentro* de enumerate)
snippet it "\\item" i
    \\item ${0}
#

# Fin de tex.text.snippets
```

---

## 13. Buenas prácticas para ampliar y mantener

1. **Actualiza `tex-context.vim`** cuando agregues nuevos entornos cuyo contexto quieras controlar (por ejemplo, `align`, `equation`, `tikzpicture`, etc.).
    
2. **Cada archivo `*.snippets` debe comenzar** con un pequeño encabezado indicando su propósito y contexto. Ejemplo:
```snippets
# ----------------------------------------------------------------------------
# Archivo: tex.math.snippets
# Descripción: Snippets para uso exclusivo en modo matemático (b:in_math_mode)
# Prefijo de triggers: m_  
# ----------------------------------------------------------------------------
```
    
3. **Siempre que migres un nuevo fragmento** de UltiSnips, reescríbelo en Snipmate “separando” el contexto y renombrando el trigger con la convención que hayas elegido.
4. **Evita conflictos de triggers**: si tienes un snippet `sum` en modo texto (por ejemplo “resumen”), no uses exactamente `sum` en modo matemático: usa `m_sum`.
5. **Documenta en tu README personal** (o en el encabezado de `tex-context.vim`) la lista de variables de buffer (p.ej., `b:in_math_mode`, `b:in_theorem_env`) y explica brevemente qué hace cada una, para futuros colaboradores.

---

## 14. Conclusión

Con esta guía:

- **Definiste funciones globales** en `tex-context.vim` que determinan el contexto (matemático, entornos específicos).
- **Separaste y renombraste** los snippets de UltiSnips en archivos Snipmate (`tex.math.snippets`, `tex.theorem.snippets`, etc.), aplicando los prefijos de trigger (`m_`, `thm`, `lem`, etc.).
- **Mostraste cómo estructurar** físicamente los archivos para que la carga sea clara y escalable.
- **Proporcionaste ejemplos concretos** de la migración, adaptando triggers y flags.

De esta forma, tus snippets en NeoVim con LuaSnips + Snipmate no sólo funcionarán —sino que estarán “contextualizados”—: el usuario sabrá exactamente cuál atajo usar en cada bloque de su documento LaTeX, sin que un snippet se dispare donde no corresponda.

---

**Referencias principales**

1. Gilles Castel, “latex-snippets/tex.snippets” (UltiSnips)
2. Documentación oficial Snipmate (syntaxis de `.snippets`)
3. Configuración de LuaSnips para cargar Snipmate: `require("luasnip.loaders.from_snipmate").lazy_load()`
