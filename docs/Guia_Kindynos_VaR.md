# Kíndynos VaR

**Guía técnica del aplicativo · Versión 1.0**

Autor: Mario Sergio Gómez Rueda · mgomezr1@gmail.com
Uso académico. Cualquier otro uso se regirá por el derecho de la propiedad intelectual.

## 1. Descripción general

Kíndynos VaR es una aplicación web académica, contenida en un solo archivo `index.html`, que calcula el **valor en riesgo (VaR) paramétrico** de una posición en un activo. El nombre viene del griego κίνδυνος, riesgo o peligro, y sigue la línea del portafolio de aplicativos del autor (Kairós AHP, Áristos ELECTRE y Éngista TOPSIS).

Responde una pregunta concreta: con un nivel de confianza dado, ¿cuál es la pérdida que no debería superarse en un horizonte de tiempo? A partir de una serie de precios, o de una media y una volatilidad conocidas, la aplicación:

* estima la media y la volatilidad diarias, con desviación estándar muestral o con EWMA de RiskMetrics;
* calcula el VaR y el Expected Shortfall (ES) para uno o varios niveles de confianza y cualquier horizonte;
* revisa el supuesto de normalidad con asimetría, curtosis y Jarque Bera;
* valida el modelo con backtesting, la prueba de Kupiec y el semáforo del Comité de Basilea;
* calcula qué posición, volatilidad, nivel de confianza u horizonte llevarían el VaR a un límite de riesgo;
* exporta un libro de Excel con fórmulas reales y un informe ejecutivo en PDF.

Todo se calcula en el navegador. Ningún dato sale del equipo del usuario y no se usan servicios de terceros ni estadísticas de visitas.

## 2. Cómo guardar el archivo

Guarde `index.html` en una carpeta propia. No necesita otros archivos. Si quiere usar fuentes tipográficas propias, cree una carpeta `fuentes` junto al archivo y siga las instrucciones comentadas al inicio del bloque de estilos.

## 3. Cómo ejecutarlo

* **Local:** doble clic sobre `index.html`. Se abre en el navegador predeterminado.
* **En línea:** publique la carpeta del repositorio en GitHub Pages (ver `README.md`).
* **Requisitos:** navegador actualizado (Chrome, Edge, Firefox o Safari) con JavaScript activo. El Excel requiere conexión para descargar SheetJS desde cdn.sheetjs.com; los cálculos y el PDF funcionan sin conexión.

## 4. Estructura de la interfaz

Un riel lateral organiza el análisis en pasos. Marca la sección visible y el estado de cada paso con un punto verde (listo) o coral (requiere atención). En tabletas y teléfonos se convierte en una barra superior.

| Paso | Contenido |
|---|---|
| Portada | Curva normal interactiva: un deslizador del nivel de confianza mueve la cola de pérdida y explica el valor z. Se maneja con ratón y con teclado. |
| 00 El método | Cinco pasos con diagrama, fórmulas, supuestos y referencias. |
| 01 Configuración | Activo, valor de la posición, moneda, horizonte, niveles de confianza, límite opcional, fuente de los parámetros y variantes. Cada campo se valida al salir de él y la configuración se confirma con un botón. |
| 02 Datos | Serie de precios pegada o cargada desde CSV, con revisión de calidad en vivo y gráfico de la serie; o media y volatilidad conocidas. |
| 03 Resultados | VaR, ES y volatilidad destacados, frase de conclusión, tablas 1 y 2, gráfico 1 de la distribución con la cola de pérdida y tabla 3 de normalidad. |
| 04 Robustez | Tabla 4 y gráfico 2 del backtesting; tabla 5 con los valores que igualan el límite; gráfico 3 del VaR según la confianza o el horizonte. |
| 05 Exportación | Botones de Excel y PDF. |
| 06 Pruebas | Cinco escenarios de datos ficticios y 19 pruebas automáticas con su tabla de resultados. |

### Lectura de la serie

* Una fila por día: solo el precio, o fecha y precio separados por punto y coma, tabulación o coma.
* Fechas en formato aaaa‑mm‑dd o dd/mm/aaaa. Si hay fechas, el orden se detecta y corrige solo; si no, el usuario indica si la primera fila es la más antigua o la más reciente.
* Separador decimal: se detecta para toda la serie. Si algún precio usa coma decimal, la coma se toma como decimal y el punto como separador de miles.
* Una primera fila no numérica se toma como encabezado.
* Se rechazan precios no positivos, filas con más de dos columnas, fechas repetidas o mezcla de filas con y sin fecha. El mensaje indica las filas a corregir.
* Se exigen al menos 11 precios (10 rendimientos) y se advierte con menos de 30.

### Mensajes

Los mensajes dicen qué pasó y cómo corregirlo. Las advertencias metodológicas aparecen junto a los resultados: normalidad rechazada, escalado por raíz del tiempo, media no significativa, VaR negativo, volatilidad del horizonte superior al 25 % o pocos días de backtesting.

### Identidad visual

* Fondo índigo profundo y saturado; superficies separadas por tono y sombra, sin bordes.
* Par de colores con significado: **calma** (verde glaciar, `#7FE0C4`) para la zona de confianza y **brasa** (coral, `#FF6F5B`) para la cola de pérdida. Tiñen la curva de la portada, los gráficos, las cifras principales y los estados.
* Paleta y medidas definidas como variables en `:root`. Pila de fuentes del sistema, con bloque `@font-face` comentado para fuentes locales.
* Transiciones suaves que respetan `prefers-reduced-motion`. Foco visible, etiquetas asociadas a cada campo y textos alternativos en los gráficos.
* Sin desplazamiento horizontal a 400 px; las tablas anchas se desplazan dentro de su contenedor.

## 5. Exportación a Excel

El libro se llama `Kindynos_VaR_<activo>_<fecha>.xlsx` y lleva el prefijo `PRUEBA_` si los datos son ficticios. Las fórmulas usan funciones compatibles con todas las versiones de Excel y con LibreOffice (`NORMSINV`, `NORMSDIST`, `STDEV`, `CHIDIST`, `BINOMDIST`).

| Hoja | Contenido |
|---|---|
| Menu | Advertencia de datos de prueba si aplica, activo, origen, fecha, VaR principal, índice con enlaces a cada hoja y autoría. |
| Datos | Fecha, precio, rendimiento (`=LN(B7/B6)` o `=B7/B6-1`) y varianza EWMA de cada día (`=λ*D8+(1-λ)*C8^2`). Solo con serie histórica. |
| Parametros | Posición, horizonte, inclusión de la media, λ, número de rendimientos (`COUNT`), media (`AVERAGE`), volatilidad muestral (`STDEV`), volatilidad EWMA pronosticada, volatilidad usada y el diagnóstico de normalidad con momentos centrales (`SUMPRODUCT`), asimetría, curtosis, Jarque Bera y su valor p (`CHIDIST`). |
| VaR | Por nivel: c, `z = NORMSINV(c)`, σ·√h, μ·h, VaR %, VaR en dinero, φ(z), ES % y ES en dinero. |
| Backtesting_xx | Una hoja por nivel con backtesting disponible: resumen (p, W, z, T, x, esperadas, LR de Kupiec, valor p, probabilidad binomial acumulada, zona y decisión) y tabla diaria con media y volatilidad de la ventana, VaR de un día y marca de excepción. |
| Robustez | Con límite: uso del límite, posición máxima, volatilidad máxima, confianza máxima y horizonte máximo por nivel, todos con fórmula. |
| Resultado | Estructura del modelo, VaR y ES principales enlazados a la hoja VaR, conclusión y advertencias. |

## 6. Informe ejecutivo en PDF

Generado con código propio, sin librerías, en tamaño carta. Estructura:

1. **Conclusión:** caja con el VaR y el ES del nivel principal, hechos clave (frase de conclusión, backtesting y normalidad) y origen de los datos, con aviso destacado si son de prueba.
2. **VaR y ES por nivel de confianza:** tabla con barras y curva normal con la cola de pérdida.
3. **Estructura del modelo:** método, fuente, estimador, media, volatilidad y escalado.
4. **Calidad del supuesto de normalidad:** asimetría, curtosis, Jarque Bera y valor p, más las advertencias.
5. **Robustez:** tabla de Kupiec y semáforo por nivel; con límite, tabla de los valores que lo igualan.
6. **Nota metodológica y referencias**, y recuadro de autoría y uso.

Todas las páginas llevan marca de agua diagonal translúcida («USO ACADÉMICO» y el nombre del aplicativo con el autor) y pie con nombre, versión, autor, mención de uso académico, correo, nota de propiedad intelectual y numeración.

## 7. Fundamento de cálculo

Notación: P(t) precio del día t, r(t) rendimiento, n número de rendimientos, c nivel de confianza, h horizonte en días, V valor de la posición, Φ y φ la distribución y la densidad normal estándar.

**Rendimientos.** Logarítmico r(t) = ln[P(t) / P(t−1)]; simple r(t) = P(t) / P(t−1) − 1.

**Media y volatilidad muestrales.**

μ = (1/n) Σ r(t)  σ = √[ Σ (r(t) − μ)² / (n − 1) ]

**Volatilidad EWMA (RiskMetrics, 1996).** Con λ entre 0,5 y 1 (0,94 publicado para datos diarios):

σ²(2) = r(1)²  σ²(t) = λ·σ²(t−1) + (1 − λ)·r(t−1)²

El pronóstico para el día siguiente es σ²(n+1) = λ·σ²(n) + (1 − λ)·r(n)².

**Media usada.** m = μ si se incluye la media (VaR absoluto); m = 0 en la variante de media cero (VaR relativo).

**VaR y ES paramétricos** (Jorion, 2007; Hull, 2018):

z = Φ⁻¹(c)
VaR% = z·σ·√h − m·h  VaR = V·VaR%
ES% = σ·√h·φ(z) / (1 − c) − m·h  ES = V·ES%

El paso a dinero es la aproximación lineal del método delta normal. El escalado por √h es la regla de la raíz del tiempo.

**Normalidad (Jarque y Bera, 1987).** Con momentos centrales m(k) = (1/n) Σ (r(t) − μ)^k:

S = m3 / m2^1,5  K = m4 / m2² − 3  JB = (n/6)·(S² + K²/4)

Valor p = P(χ²₂ > JB) = e^(−JB/2). Se rechaza la normalidad si es menor que 0,05.

**Backtesting (Kupiec, 1995).** Con ventana W, para cada día t desde W+1 hasta n se estima con los W días previos: m(t), σ(t) (muestral de la ventana, o EWMA σ(t)) y VaR(t) = z·σ(t) − m(t). Hay excepción si r(t) < −VaR(t). Con T = n − W días, x excepciones y p = 1 − c:

LR = −2·ln[(1 − p)^(T−x)·p^x] + 2·ln[(1 − x/T)^(T−x)·(x/T)^x]

con la convención 0·ln 0 = 0. Valor p = P(χ²₁ > LR) = 2·[1 − Φ(√LR)].

**Semáforo del Comité de Basilea (1996).** Con la probabilidad binomial acumulada F(x; T, p): zona verde si F < 0,95; amarilla si 0,95 ≤ F < 0,9999; roja si F ≥ 0,9999. Con T = 250 y c = 99 % reproduce las zonas publicadas: verde de 0 a 4 excepciones, amarilla de 5 a 9 y roja desde 10.

**Valores que igualan el límite L.** Con l = L/V y una sola variable libre:

Posición máxima = L / VaR%
Volatilidad máxima = (l + m·h) / (z·√h)
Confianza máxima = Φ[(l + m·h) / (σ·√h)]
Horizonte máximo: con s = √h, raíz positiva de m·s² − z·σ·s + l = 0; si m = 0, h = [l / (z·σ)]². Si el discriminante es negativo el límite nunca se alcanza.

**Funciones numéricas.** Φ con el algoritmo de Hart en la versión de West (2005), de precisión cercana a la doble. Φ⁻¹ con la aproximación racional de Acklam seguida de dos pasos de refinamiento de Halley; el error de Φ(Φ⁻¹(p)) − p es del orden de 1e‑16.

## 8. Explicación de las funciones

El código está ordenado en 18 secciones numeradas, con nombres y comentarios en español.

### Datos del aplicativo y utilidades (secciones 1 y 2)

* `APLICATIVO`: constante única con nombre, versión, autor, correo y nota de uso; desde ella se escriben la interfaz, el Excel y el PDF.
* `REFERENCIAS`, `estado`: referencias APA 7 y estado único de la aplicación.
* `leerNumero(texto, convencion)`: interpreta números con coma o punto decimal y separador de miles.
* `leerFecha`, `fechaISO`, `formatearNumero`, `formatearPorcentaje`, `formatearDinero`, `formatearNivel`, `formatearP`: lectura y formato colombiano.

### Estadística (sección 3)

* `densidadNormal`, `normalAcumulada`, `normalInversa`: φ, Φ y Φ⁻¹.
* `valorPChi2(x, gl)`: cola derecha de chi cuadrado con 1 o 2 grados de libertad.
* `binomialAcumulada(x, T, p)`, `media`, `desviacionMuestral`.

### Serie, modelo, backtesting y límites (secciones 4 a 7)

* `leerSerie(texto, orden)`: separadores, encabezado, convención decimal, fechas, orden y validaciones.
* `calcularRendimientos`, `varianzasEWMA`: rendimientos y recursión EWMA.
* `varParametrico({c, mu, sigma, h})`: z, σ·√h, μ·h, VaR % y ES %.
* `pruebaNormalidad(r)`: asimetría, curtosis, Jarque Bera y valor p.
* `calcularModelo(cfg, serie)`: cálculo completo y advertencias (`construirAdvertencias`).
* `estadisticoKupiec`, `zonaSemaforo`, `backtestKupiec`: ventana móvil, excepciones, LR, valor p y zona.
* `analisisLimite`: posición, volatilidad, confianza y horizonte que igualan el límite.

### Interfaz y gráficos (secciones 8 a 13)

* `mostrarMensaje`, `actualizarEstadoPasos`, `observarSecciones`: mensajes y riel.
* `leerConfiguracion`, `confirmarConfiguracion`, `actualizarPaneles`: validación del paso 01.
* `revisarSerie`, `leerParametrosDirectos`, `calcular`: paso 02 y disparo del cálculo.
* `mostrarResultados`, `fraseConclusion`, `mostrarRobustez`, `mostrarBacktesting`, `mostrarLimites`.
* `dibujarPortada`, `dibujarPrecios`, `dibujarDensidad`, `dibujarBacktesting`, `dibujarSensibilidad`: gráficos SVG hechos a mano, con descripciones accesibles y tooltips.

### Exportación (secciones 14 y 15)

* `crearHojaExcel`, `construirLibroExcel`, `exportarExcel`: libro con fórmulas y enlaces.
* `DocumentoPDF`: generador propio de PDF (texto, rectángulos, tablas, polígonos y líneas, marca de agua y pie).
* `dibujarCurvaPDF`, `generarInformePDF`, `exportarPDF`.

### Prueba e inicio (secciones 16 a 18)

* `generadorSemilla`, `normalAleatoria`, `generarSerie`, `ESCENARIOS`, `cargarEscenario`: datos ficticios reproducibles.
* `ejecutarPruebas`, `pintarPruebas`: 19 pruebas; guardan y restauran lo que el usuario tenía en pantalla.
* `iniciar`: conecta eventos y escribe la autoría.

## 9. Ejemplo de uso con resultados verificados

**Ejemplo de libro** (escenario «Parámetros de libro»): V = 1 000 000 USD, μ = 0, σ diaria = 2 %, horizonte 10 días, límite 150 000 USD.

| Nivel | z | σ·√h | VaR (USD) | ES (USD) |
|---|---|---|---|---|
| 95 % | 1,6449 | 6,325 % | 104 029,68 | 130 457,41 |
| 99 % | 2,3263 | 6,325 % | 147 131,16 | 168 562,95 |

Verificación a mano al 99 %: 2,326348 × 0,02 × √10 × 1 000 000 = 147 131,16. ES al 95 %: 0,063246 × φ(1,644854) / 0,05 × 1 000 000 = 0,063246 × 0,103136 / 0,05 × 1 000 000 = 130 457,41.

Con el límite de 150 000 USD al 99 % el VaR usa el 98,09 % del límite. Lo igualarían una posición de 1 019 498,53 USD, una volatilidad diaria de 2,039 %, un nivel de confianza de 99,11 % o un horizonte de 10,39 días.

**Serie simulada** (escenario «Mediano: 500 precios»): 499 rendimientos logarítmicos, media incluida, volatilidad muestral, horizonte 10 días, posición 100 000 000 COP, límite 12 000 000 COP.

* μ diaria = 0,0381 %, σ diaria = 1,8372 % (EWMA de referencia 1,8362 %).
* VaR al 95 % = 9 175 396,76 COP y ES = 11 603 085,60 COP; VaR al 99 % = 13 134 759,28 COP y ES = 15 103 513,56 COP.
* Normalidad: asimetría −0,0658, curtosis en exceso 0,1621, Jarque Bera 0,906, valor p 0,6357 (no se rechaza).
* Backtesting con ventana de 250 días: al 95 %, 11 excepciones en 249 días (12,45 esperadas), LR 0,1847, valor p 0,6674, zona verde; al 99 %, 3 excepciones (2,49 esperadas), LR 0,0990, valor p 0,7530, zona verde.
* El VaR al 99 % supera el límite (109,46 % de uso). Lo cumplirían una posición de 91 360 639 COP, una volatilidad diaria de 1,683 %, un nivel de confianza de 98,35 % o un horizonte de 8,3 días.

Todas estas cifras coinciden con un cálculo independiente en Python con NumPy y SciPy, con error relativo máximo de 1,4e‑13.

## 10. Modo de prueba

| Escenario | Qué muestra |
|---|---|
| Pequeño: 60 precios | Serie corta con ventana de 20 días; aviso de poca potencia del backtesting. |
| Mediano: 500 precios | Caso típico con horizonte de 10 días y un límite que se supera al 99 %. |
| Grande con colas gruesas: 2 000 precios | Rendimientos t de Student con 4 grados de libertad. Jarque Bera rechaza la normalidad; Kupiec acepta al 95 % pero rechaza al 99 % y 99,5 %: el VaR paramétrico falla justo en la cola. |
| Volatilidad cambiante: 800 precios | La volatilidad se triplica en el último 30 %. Con estimador muestral el backtesting al 99 % cae en zona roja; sirve para comparar con EWMA. |
| Parámetros de libro | μ = 0 y σ = 2 %, verificable a mano. |

Los datos se generan con semilla fija (Mulberry32 y Box Muller), quedan marcados como datos de prueba en pantalla, en el Excel y en el PDF, y los archivos llevan el prefijo `PRUEBA_`. Si el usuario modifica un escenario, el origen pasa a «datos del usuario partiendo de un escenario de prueba modificado».

### Pruebas automáticas

1. Φ⁻¹ en 95 %, 97,5 % y 99 % contra valores publicados (tolerancia 1e‑12).
2. Φ(Φ⁻¹(p)) = p en 999 puntos.
3. VaR analítico con V = 1 000 000, σ = 2 %, 99 %, 1 día: 46 526,96.
4. Raíz del tiempo: VaR a 10 días = √10 veces el de 1 día (147 131,16).
5. ES analítico al 99 %: 53 304,28.
6. VaR con media μ = 0,1 %, σ = 2 %, 95 %.
7. Lectura de números en formatos colombiano e internacional.
8. Rendimientos logarítmicos y simples.
9. Desviación estándar muestral con n − 1.
10. Recursión EWMA calculada a mano.
11. Kupiec con T = 250, x = 5, p = 1 % (LR 1,9568) y con x = 0 (LR 5,0252).
12. Valores críticos de chi cuadrado con 1 y 2 grados de libertad.
13. Zonas de Basilea con 250 días al 99 %: 4 verde, 5 y 9 amarilla, 10 roja.
14. Límite inverso: cada valor calculado devuelve exactamente el límite.
15. Casos límite: precio negativo, serie corta y fechas en orden inverso.
16. Tamaños dinámicos: 59, 499, 799 y 1 999 rendimientos.
17. Jarque Bera rechaza colas gruesas y no rechaza una normal simulada.
18. Estructura del Excel: hojas, fórmula `NORMSINV` y enlaces del menú.
19. Estructura del PDF: encabezado, cierre, pie y marca de agua en cada página.

## 11. Verificación realizada

* La aplicación se ejecutó en Chromium, en escritorio (1366 px) y en teléfono (400 px). Las 19 pruebas pasaron y la consola no registró errores ni advertencias.
* A 400 px el ancho del documento es exactamente 400 px: no hay desplazamiento horizontal.
* Se exportaron el Excel y el PDF de los cinco escenarios. Los resultados del VaR, el ES, la volatilidad, Jarque Bera, las excepciones, el LR, el valor p y la zona coincidieron con un cálculo independiente en Python (NumPy y SciPy); error relativo máximo 1,4e‑13.
* Los cinco libros se recalcularon en LibreOffice: 38 865 fórmulas, cero errores, y todos los valores recalculados coinciden con los guardados (diferencia relativa máxima 1e‑12). Esta revisión detectó y permitió corregir una fórmula de φ(z) en la que el signo menos se aplicaba antes que la potencia.
* Los PDF abren sin errores; cada página tiene marca de agua y pie de página.

## 12. Limitaciones

* **Normalidad.** El método supone rendimientos normales. Con colas gruesas subestima el riesgo extremo, como muestra el escenario de colas gruesas. La aplicación lo advierte, pero no corrige la distribución (no incluye Cornish Fisher, simulación histórica ni Monte Carlo en esta versión).
* **Raíz del tiempo.** Supone rendimientos independientes e idénticamente distribuidos. Con autocorrelación o volatilidad cambiante es solo una aproximación.
* **Un solo activo.** No agrega posiciones ni considera correlaciones.
* **Aproximación lineal.** El VaR en dinero es V·VaR%. Con rendimientos logarítmicos la pérdida exacta sería V·(1 − e^(−VaR%)), algo menor.
* **EWMA.** Se inicializa con σ²(2) = r(1)². Otras inicializaciones cambian los primeros valores; su efecto se diluye con series largas. RiskMetrics supone media cero; si se combina EWMA con media incluida, la media es la muestral.
* **Backtesting.** Solo evalúa el VaR de un día. La prueba de Kupiec revisa la frecuencia de excepciones, no si llegan agrupadas (para eso existe la prueba de independencia de Christoffersen, no incluida). Con menos de 250 días tiene poca potencia.
* **Semáforo.** Las zonas se generalizan con la probabilidad binomial acumulada, que es el criterio con el que Basilea las definió; los multiplicadores de capital de cada zona no se aplican.
* **Persistencia.** Los datos no se guardan: al recargar la página se pierden.

## 13. Referencias

Basel Committee on Banking Supervision. (1996). *Supervisory framework for the use of "backtesting" in conjunction with the internal models approach to market risk capital requirements*. Bank for International Settlements.

Basel Committee on Banking Supervision. (2019). *Minimum capital requirements for market risk*. Bank for International Settlements.

Hull, J. C. (2018). *Risk management and financial institutions* (5th ed.). Wiley.

Jarque, C. M., & Bera, A. K. (1987). A test for normality of observations and regression residuals. *International Statistical Review, 55*(2), 163–172. https://doi.org/10.2307/1403192

Jorion, P. (2007). *Value at risk: The new benchmark for managing financial risk* (3rd ed.). McGraw‑Hill.

J.P. Morgan & Reuters. (1996). *RiskMetrics: Technical document* (4th ed.). Morgan Guaranty Trust Company.

Kupiec, P. H. (1995). Techniques for verifying the accuracy of risk measurement models. *The Journal of Derivatives, 3*(2), 73–84. https://doi.org/10.3905/jod.1995.407942

West, G. (2005). Better approximations to cumulative normal functions. *Wilmott Magazine*, 70–76.
