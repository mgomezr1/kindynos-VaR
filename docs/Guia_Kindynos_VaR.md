# Kíndynos VaR

**Guía técnica del aplicativo · Versión 2.0**

Autor: Mario Sergio Gómez Rueda · mgomezr1@gmail.com
Uso académico. Cualquier otro uso se regirá por el derecho de la propiedad intelectual.
Aplicación: https://mgomezr1.github.io/kindynos-VaR/ · Código: https://github.com/mgomezr1/kindynos-VaR

## 1. Descripción general

Kíndynos VaR es una aplicación web académica, contenida en un solo archivo `index.html`, para la **estimación y el diagnóstico del riesgo de mercado** de una posición en un activo. El nombre viene del griego κίνδυνος, riesgo o peligro, y sigue la línea del portafolio de aplicativos del autor (Kairós AHP, Áristos ELECTRE y Éngista TOPSIS).

Su lema resume el enfoque: **mida el riesgo, evalúe el modelo y explore sus límites.** Kíndynos no se limita a calcular cuánto riesgo existe: ayuda a entender de dónde viene, a evaluar si el modelo es confiable y a determinar cuándo ese riesgo deja de ser aceptable. Medir el riesgo no equivale a tomar una decisión; por eso el análisis sigue la ruta datos, modelo, riesgo, validación, límite y decisión.

El valor en riesgo (VaR) estima un **umbral de pérdida** que solo debería excederse con una probabilidad determinada durante el horizonte elegido. No es una pérdida máxima.

La aplicación:

* lee una matriz de precios con uno o varios activos (estándar de datos Kíndynos) o recibe una media y una volatilidad conocidas;
* estima la media y la volatilidad diarias del activo elegido, con desviación estándar muestral o con EWMA de RiskMetrics;
* calcula el VaR y el Expected Shortfall (ES) para uno o varios niveles de confianza y cualquier horizonte;
* diagnostica el supuesto de normalidad con asimetría, curtosis y Jarque Bera;
* valida el modelo con backtesting, la prueba de Kupiec y, por separado, el semáforo del Comité de Basilea;
* calcula la frontera de riesgo: qué posición, volatilidad, nivel de confianza u horizonte llevarían el VaR exactamente al límite, y el margen que queda;
* exporta un modelo auditable en Excel con fórmulas reales y un informe ejecutivo en PDF.

Todo se calcula en el navegador. Ningún dato sale del equipo del usuario y no se usan servicios de terceros ni estadísticas de visitas.

### Novedades de la versión 2.0

La versión 2.0 no cambia el motor cuantitativo: los cinco escenarios de la versión 1 reproducen exactamente las mismas cifras. Los cambios son de datos, arquitectura de información y comunicación del riesgo:

* estándar de datos matricial de 1 a n activos, con plantillas Excel y CSV, arrastrar y soltar, lectura de archivos Excel, validación resumida, vista previa, mensajes de error por columna y fila, y selector del activo a analizar;
* navegación en seis pasos (Método, Modelo, Datos, Riesgo, Robustez, Informe) y una sección Acerca de Kíndynos;
* configuración básica y avanzada, puntos de partida RiskMetrics y Basilea, y etiquetas nuevas para el rendimiento esperado;
* resumen de riesgo, lectura narrativa, marcas de μ y ES en la distribución y bloque de diagnóstico del modelo;
* Kupiec y semáforo de Basilea separados, frontera de riesgo con protagonismo y margen frente al límite;
* hoja Matriz, margen y punto de partida en el Excel; PDF reorganizado.

## 2. Cómo guardar el archivo

Guarde `index.html` en una carpeta propia. No necesita otros archivos. Si quiere usar fuentes tipográficas propias, cree una carpeta `fuentes` junto al archivo y siga las instrucciones comentadas al inicio del bloque de estilos.

## 3. Cómo ejecutarlo

* **Local:** doble clic sobre `index.html`. Se abre en el navegador predeterminado.
* **En línea:** https://mgomezr1.github.io/kindynos-VaR/, publicada con GitHub Pages (ver `README.md`).
* **Requisitos:** navegador actualizado (Chrome, Edge, Firefox o Safari) con JavaScript activo. El Excel de resultados, la plantilla Excel y la lectura de archivos Excel requieren conexión para descargar SheetJS desde cdn.sheetjs.com. Los cálculos, el PDF, la plantilla CSV y la lectura de CSV funcionan sin conexión.

## 4. Estructura de la interfaz

Un riel lateral organiza el análisis. Marca la sección visible y el estado de cada paso con un símbolo además del color: ✓ listo y ! requiere atención. En tabletas y teléfonos se convierte en una barra superior.

| Paso | Pregunta que responde | Contenido |
|---|---|---|
| Portada | ¿Qué es Kíndynos? | Lema, definición del VaR como umbral, curva normal interactiva y tres entradas: Iniciar análisis, Usar un ejemplo y Entender el método. |
| 00 Método | ¿Qué está calculando Kíndynos? | Cadena precios, retornos, volatilidad, distribución y riesgo; ruta de la decisión; formulación matemática y supuestos en desplegables. |
| 01 Modelo | ¿Bajo qué supuestos y parámetros? | Punto de partida, configuración básica y configuración avanzada contraída. |
| 02 Datos | ¿Con qué información? | Matriz de precios o parámetros conocidos, plantillas, carga, validación, vista previa y selector de activo. |
| 03 Riesgo | ¿Cuánto riesgo existe? | Resumen de riesgo, lectura narrativa, distribución, tablas y diagnóstico del modelo. |
| 04 Robustez | ¿El modelo se comporta razonablemente y qué tan cerca estoy del límite? | Backtesting con Kupiec y semáforo de Basilea; frontera de riesgo. |
| 05 Informe | ¿Cómo documento y comunico el análisis? | Modelo auditable en Excel e informe ejecutivo en PDF. |
| Acerca de | ¿Qué respalda a Kíndynos? | Nombre, versión, autoría, validación con pruebas técnicas y escenarios, referencias, citación, código fuente y licencia. |

El orden de los pasos presenta el modelo antes que los datos porque el horizonte y la ventana condicionan la revisión de la carga; la lógica conceptual de la decisión es la de la ruta del Método.

### Método

La explicación usa divulgación progresiva: primero la cadena del método y la ruta de la decisión; luego, en desplegables, la formulación matemática completa y la tabla de supuestos. Cada supuesto indica qué ocurre si no se cumple y dónde lo revisa Kíndynos:

| Supuesto | Dónde se revisa |
|---|---|
| Normalidad | Diagnóstico del modelo, con Jarque Bera (paso 03). |
| Independencia | Advertencia cuando h > 1. Esta versión no la prueba estadísticamente. |
| Estabilidad de σ | Comparación entre volatilidad muestral y EWMA (Tabla 1) y backtesting (paso 04). |
| Linealidad | Advertencia cuando la volatilidad del horizonte supera el 25 %. |

### Modelo

* **Configuración básica:** valor de la posición, moneda, horizonte (1 a 250 días), niveles de confianza (uno a seis, entre 80 y 99,99) y límite de riesgo opcional.
* **Configuración avanzada:** tipo de rendimiento, estimador de volatilidad, λ de EWMA, rendimiento esperado y ventana del backtesting. Contraída, muestra una línea de resumen.
* **Rendimiento esperado:** «Incluir rendimiento esperado (μ)» produce el VaR absoluto; «Asumir μ = 0» produce el VaR relativo a la media, la convención de RiskMetrics.
* **Puntos de partida.** Cada uno fija solo los parámetros que define su fuente:

| Punto de partida | Parámetros que fija | Fuente |
|---|---|---|
| Personalizada | Ninguno | Parámetros del usuario. |
| RiskMetrics | EWMA con λ = 0,94, μ = 0, horizonte 1 día, confianza 95 % | J.P. Morgan y Reuters (1996). |
| Basilea | Confianza 99 %, horizonte 10 días, ventana de backtesting 250 días | BCBS (1996). |

Si el usuario edita un parámetro fijado por el punto de partida, este vuelve a «Personalizada». Cada campo se valida al salir de él y el modelo se confirma con un botón; si se calcula sin confirmar, Kíndynos lo confirma en silencio o indica qué corregir.

### Datos: estándar de datos Kíndynos

El estándar es matricial, único y escalable de 1 a n activos:

| Fecha | ECOPETROL | ISA | PFBCOLOM | BTC |
|---|---|---|---|---|
| 02/01/2026 | 1865 | 18240 | 46300 | 94320 |
| 05/01/2026 | 1890 | 18310 | 46700 | 95180 |
| 06/01/2026 | 1875 | 18180 | 46250 | 93870 |

* **Primera columna:** índice de observación, Fecha (aaaa‑mm‑dd o dd/mm/aaaa) u Orden (1, 2, 3…).
* **Columnas siguientes:** un activo por columna. Los encabezados son libres y se conservan; si se repiten, el segundo recibe un sufijo, por ejemplo «ISA (2)».
* **Cada fila** es una observación común a todos los activos. Un solo activo usa el mismo estándar: Fecha y una columna.
* **Separadores:** punto y coma, tabulación (al copiar desde Excel) o coma. Con coma como separador se exige encabezado y punto decimal.
* **Decimales:** se detecta para toda la matriz si la coma o el punto es el decimal.
* **Faltantes:** una celda vacía (o NA, N/A, NaN, null, #N/A) es un valor faltante. Se excluye solo para su activo y los rendimientos se calculan entre precios válidos consecutivos. Esto permite mezclar activos con calendarios distintos.
* **Orden:** con fechas u orden, las filas se ordenan de la más antigua a la más reciente y se informa si se invirtieron o reordenaron. Sin índice (una sola columna de precios) se pregunta el orden.
* **Mínimos:** 11 precios válidos por activo (10 rendimientos); con menos de 30 rendimientos hay advertencia; para el backtesting se necesitan ventana más 30 rendimientos.

**Compatibilidad con la versión 1.** Siguen siendo válidos los archivos de una sola columna de precios, los de fecha y precio separados por punto y coma o tabulación, los de «fecha,precio» con coma decimal (se corta solo en la primera coma) y los que tienen encabezado «Fecha;Precio». La prueba automática 31 lo verifica.

**Plantillas.** «Descargar plantilla Excel» entrega un libro con la hoja Datos (matriz de ejemplo con 12 observaciones ilustrativas de tres activos genéricos) y la hoja Instrucciones. «Descargar plantilla CSV» entrega la misma matriz con punto y coma y coma decimal, como la abre Excel en español. «Cargar ejemplo» es otra acción: pone en Datos una matriz ficticia de tres activos y 500 fechas, marcada como datos de prueba.

**Carga.** Arrastrar y soltar, seleccionar un archivo (CSV, texto o Excel; se lee la hoja Datos o la primera) o pegar la matriz en «O pegue los datos».

**Validación.** La tarjeta «Datos reconocidos» muestra observaciones, rango del índice, activos identificados, valores faltantes, duplicados, inválidos, separador y decimal, una tabla por activo (válidas, vacías, mínimo, máximo y disponibilidad) y el estado «Matriz lista para análisis». Los mensajes de error nombran la columna y la fila, por ejemplo:

* «La columna ISA contiene un valor no numérico en la fila 37 («abc»).»
* «Se encontraron 2 observaciones con fecha 15/03/2026 (filas 2, 3).»
* «La fila 6 tiene 2 columnas y se esperaban 3. Si falta un valor, deje la celda vacía.»
* «ISA tiene 3 observaciones vacías; se excluyeron y los rendimientos se calcularon entre precios válidos consecutivos.»

**Vista previa y selector.** Se muestran las tres primeras y las tres últimas observaciones ya ordenadas e interpretadas. Con varios activos aparece «Activo a analizar»: el motor VaR trabaja sobre esa columna y se puede cambiar de activo sin recargar la matriz. El nombre del activo en los informes se toma del encabezado y se puede editar.

**Esta versión no calcula VaR de portafolio.** Cargar varios activos prepara la arquitectura de datos; el análisis es de un activo a la vez.

### Riesgo

* **Resumen de riesgo:** exposición, VaR y ES del nivel de confianza más alto, y uso del límite con el margen disponible o el exceso. Las cifras grandes se muestran compactas (por ejemplo, 13,13 M) con el valor completo debajo.
* **Lectura narrativa:** tres frases condicionales basadas en los resultados; por ejemplo, «Para un horizonte de 10 días y un nivel de confianza del 99 %, Kíndynos estima un VaR de COP 13.134.759, equivalente al 13,13 % de la posición…».
* **Gráfico 1:** distribución normal del rendimiento a h días con la zona de confianza, la cola sombreada, el VaR como umbral, el ES como pérdida promedio más allá del umbral y la media escalada. Con h = 1 se superpone el histograma de los rendimientos observados.
* **Tabla 2** (VaR y ES por nivel) y **Tabla 1** (parámetros, en desplegable).
* **Diagnóstico del modelo:** Tabla 3 de normalidad, Tabla 4 de excepciones dentro de la muestra (descriptiva) y advertencias metodológicas. No se resume en puntajes: se muestran los estadísticos y cómo leerlos. El bloque está preparado para incorporar más adelante autocorrelación, agrupamiento de volatilidad u otros diagnósticos.

### Robustez

* **¿Funcionó el modelo en el pasado?** Resumen de días evaluados, excepciones esperadas y observadas. Dos bloques separados: la prueba de Kupiec (Tabla 5, contraste estadístico) y el semáforo de Basilea (Tabla 6, criterio supervisor), cada uno con su propia lectura, porque pueden no coincidir. Gráfico 2 con el rendimiento diario, el −VaR estimado el día anterior y las excepciones.
* **¿Qué tendría que cambiar para alcanzar el límite?** Frontera de riesgo (Risk Boundary): barra del VaR frente al límite, margen y, cambiando una variable a la vez, posición límite, volatilidad límite, confianza equivalente y horizonte límite (Tabla 7). Gráfico 3 del VaR según la confianza o el horizonte, con la línea del límite.

### Identidad visual y accesibilidad

* Fondo índigo profundo; superficies separadas por tono y sombra, sin bordes.
* Par de colores con significado: **calma** (`#7FE0C4`) para la zona de confianza y **brasa** (`#FF6F5B`) para la cola de pérdida. Ningún estado depende solo del color: se acompaña de ✓, ! o texto.
* Pila de fuentes del sistema; transiciones que respetan `prefers-reduced-motion`; foco visible, etiquetas asociadas y textos alternativos en los gráficos.
* Sin desplazamiento horizontal a 400 px; las tablas, la matriz y la vista previa se desplazan dentro de su contenedor.

## 5. Modelo auditable en Excel

El libro se llama `Kindynos_VaR_<activo>_<fecha>.xlsx` y lleva el prefijo `PRUEBA_` si los datos son ficticios. Las fórmulas usan funciones compatibles con todas las versiones de Excel y con LibreOffice (`NORMSINV`, `NORMSDIST`, `STDEV`, `CHIDIST`, `BINOMDIST`).

| Hoja | Contenido |
|---|---|
| Menu | Advertencia de datos de prueba si aplica, activo, origen, punto de partida, matriz cargada, fecha, VaR principal, índice con enlaces y autoría. |
| Datos | Fecha u orden, precio, rendimiento (`=LN(B7/B6)` o `=B7/B6-1`) y varianza EWMA (`=λ*D8+(1-λ)*C8^2`) del activo analizado. |
| Matriz | Matriz completa cargada, con todos los activos y los faltantes como celdas vacías. |
| Parametros | Posición, horizonte, inclusión de μ, λ, número de rendimientos (`COUNT`), media (`AVERAGE`), volatilidad muestral (`STDEV`), volatilidad EWMA pronosticada, volatilidad usada y diagnóstico de normalidad con momentos centrales (`SUMPRODUCT`), asimetría, curtosis, Jarque Bera y valor p (`CHIDIST`). |
| VaR | Por nivel: c, `z = NORMSINV(c)`, σ·√h, μ·h, VaR %, VaR en dinero, φ(z), ES % y ES en dinero. |
| Backtesting_xx | Por nivel: resumen (p, W, z, T, x, esperadas, LR de Kupiec, valor p, probabilidad binomial acumulada, zona y decisión) y tabla diaria con media y volatilidad de la ventana, VaR de un día y excepción. |
| Robustez | Frontera de riesgo: uso del límite, posición límite, volatilidad límite, confianza equivalente, horizonte límite y margen en dinero y en porcentaje, todos con fórmula. |
| Resultado | Estructura del modelo, punto de partida, activo analizado, VaR y ES enlazados, límite, uso, margen, conclusión y advertencias. |

## 6. Informe ejecutivo en PDF

Generado con código propio, sin librerías, en tamaño carta:

1. **Conclusión:** caja con el VaR y el ES del nivel principal, resumen de riesgo (exposición, VaR, ES y uso del límite), margen o exceso, lectura narrativa, backtesting y normalidad, y origen de los datos con aviso si son de prueba.
2. **VaR y ES por nivel de confianza:** tabla con barras y curva normal con la cola de pérdida, el VaR y el ES.
3. **Estructura del modelo:** método, fuente, estimador, rendimiento esperado, punto de partida, activo analizado (si hay varios), media, volatilidad y escalado.
4. **Diagnóstico del modelo:** asimetría, curtosis, Jarque Bera, valor p y advertencias.
5. **Robustez: ¿funcionó el modelo en el pasado?** Tabla de Kupiec con la decisión y tabla separada del semáforo de Basilea.
6. **Frontera de riesgo:** límite, VaR, uso, margen y valores que igualan el límite.
7. **Nota metodológica y referencias**, y recuadro de autoría y uso.

Todas las páginas llevan marca de agua diagonal translúcida («USO ACADÉMICO» y el nombre del aplicativo con el autor) y pie con nombre, versión, autor, mención de uso académico, correo, nota de propiedad intelectual y numeración.

## 7. Fundamento de cálculo

Notación: P(t) precio del día t, r(t) rendimiento, n número de rendimientos, c nivel de confianza, h horizonte en días, V valor de la posición, L límite, Φ y φ la distribución y la densidad normal estándar.

**Rendimientos.** Logarítmico r(t) = ln[P(t) / P(t−1)]; simple r(t) = P(t) / P(t−1) − 1. Con celdas vacías, P(t−1) es el precio válido anterior del mismo activo.

**Media y volatilidad muestrales.**

μ = (1/n) Σ r(t)  σ = √[ Σ (r(t) − μ)² / (n − 1) ]

**Volatilidad EWMA (RiskMetrics, 1996).** Con λ entre 0,5 y 1:

σ²(2) = r(1)²  σ²(t) = λ·σ²(t−1) + (1 − λ)·r(t−1)²

El pronóstico para el día siguiente es σ²(n+1) = λ·σ²(n) + (1 − λ)·r(n)².

**Media usada.** m = μ si se incluye el rendimiento esperado (VaR absoluto); m = 0 si se asume μ = 0 (VaR relativo).

**VaR y ES paramétricos** (Jorion, 2007; Hull, 2018):

z = Φ⁻¹(c)
VaR% = z·σ·√h − m·h  VaR = V·VaR%
ES% = σ·√h·φ(z) / (1 − c) − m·h  ES = V·ES%

**Normalidad (Jarque y Bera, 1987).** Con momentos centrales m(k) = (1/n) Σ (r(t) − μ)^k:

S = m3 / m2^1,5  K = m4 / m2² − 3  JB = (n/6)·(S² + K²/4)  valor p = e^(−JB/2)

**Backtesting (Kupiec, 1995).** Con ventana W, para cada día t desde W+1 hasta n se estima con los W días previos m(t), σ(t) y VaR(t) = z·σ(t) − m(t). Hay excepción si r(t) < −VaR(t). Con T = n − W días, x excepciones y p = 1 − c:

LR = −2·ln[(1 − p)^(T−x)·p^x] + 2·ln[(1 − x/T)^(T−x)·(x/T)^x], con 0·ln 0 = 0, y valor p = 2·[1 − Φ(√LR)].

**Semáforo del Comité de Basilea (1996).** Con F(x; T, p) binomial acumulada: verde si F < 0,95; amarilla si 0,95 ≤ F < 0,9999; roja si F ≥ 0,9999. Con T = 250 y c = 99 % reproduce las zonas publicadas (0 a 4, 5 a 9, 10 o más).

**Frontera de riesgo.** Con l = L/V y una sola variable libre:

Posición límite = L / VaR%
Volatilidad límite = (l + m·h) / (z·√h)
Confianza equivalente = Φ[(l + m·h) / (σ·√h)]
Horizonte límite: con s = √h, raíz positiva de m·s² − z·σ·s + l = 0; si m = 0, h = [l / (z·σ)]²; sin solución real, el límite no se alcanza.

**Margen frente al límite.** Margen = L − VaR y Margen % = (L − VaR) / L = 1 − uso. Positivo es margen disponible; cero, VaR exactamente en el límite; negativo, exceso sobre el límite. Solo se calcula si hay límite (el límite debe ser positivo).

**Funciones numéricas.** Φ con el algoritmo de Hart en la versión de West (2005); Φ⁻¹ con la aproximación de Acklam y dos pasos de Halley. El error de Φ(Φ⁻¹(p)) − p es del orden de 1e‑16.

## 8. Explicación de las funciones

El código está ordenado en 18 secciones numeradas, con nombres y comentarios en español.

* **Datos del aplicativo y utilidades (1 y 2):** `APLICATIVO` (nombre, versión, autor, correo, nota, sitio y repositorio), `REFERENCIAS`, `estado`, `leerNumero`, `leerFecha`, formatos (`formatearNumero`, `formatearPorcentaje`, `formatearDinero`, `formatearCompacto`, `formatearNivel`, `formatearP`).
* **Estadística (3):** `densidadNormal`, `normalAcumulada`, `normalInversa`, `valorPChi2`, `binomialAcumulada`, `media`, `desviacionMuestral`.
* **Estándar de datos (4):** `partirLineas` (separadores), `detectarConvencion` (decimal), `leerMatriz` (encabezados, índice, valores, faltantes, duplicados, orden y mensajes), `serieDeActivo` (columna elegida en el formato del motor), `leerSerie` (compatibilidad con la versión 1), `textoDesdeLibro` (lectura de Excel).
* **Modelo, backtesting y frontera (5 a 7):** `calcularRendimientos`, `varianzasEWMA`, `varParametrico`, `pruebaNormalidad`, `calcularModelo`, `construirAdvertencias`, `estadisticoKupiec`, `zonaSemaforo`, `backtestKupiec`, `analisisLimite` (incluye el margen).
* **Interfaz (8 a 12):** mensajes y estados del riel; `PRESETS`, `aplicarPreset`, `presetCoincide`, `revisarPreset`, `leerConfiguracion`, `confirmarConfiguracion`; `revisarSerie`, `aplicarSeleccion`, `seleccionarActivo`, `pintarFichaDatos`, `pintarVistaPrevia`, `leerArchivo`, plantillas (`textoPlantillaCSV`, `construirPlantillaExcel`), `calcular`; `lecturaResultado`, `mostrarResultados`, `mostrarDiagnostico`; `mostrarBacktesting`, `mostrarLimites`.
* **Gráficos (13):** `dibujarPortada`, `dibujarPrecios`, `dibujarDensidad`, `dibujarBacktesting`, `dibujarSensibilidad`.
* **Exportación (14 y 15):** `construirLibroExcel`, `exportarExcel`, `DocumentoPDF`, `dibujarCurvaPDF`, `generarInformePDF`, `exportarPDF`.
* **Prueba e inicio (16 a 18):** `generarSerie`, `ESCENARIOS`, `textoMatrizEjemplo`, `cargarEjemploDatos`, `cargarEscenario`, `ejecutarPruebas` (guarda y restaura lo que el usuario tenía en pantalla), `pintarPruebas`, `iniciar`.

## 9. Ejemplos con resultados verificados

**Ejemplo de libro** (escenario «Parámetros de libro»): V = 1 000 000 USD, μ = 0, σ diaria = 2 %, horizonte 10 días, límite 150 000 USD.

| Nivel | z | σ·√h | VaR (USD) | ES (USD) |
|---|---|---|---|---|
| 95 % | 1,6449 | 6,325 % | 104 029,68 | 130 457,41 |
| 99 % | 2,3263 | 6,325 % | 147 131,16 | 168 562,95 |

Verificación a mano al 99 %: 2,326348 × 0,02 × √10 × 1 000 000 = 147 131,16. Con el límite de 150 000 USD el VaR usa el 98,09 % y deja un margen de 2 868,84 USD (1,9 %).

**Serie simulada** (escenario «Mediano: 500 precios»): 499 rendimientos logarítmicos, μ incluida, volatilidad muestral, horizonte 10 días, posición 100 000 000 COP, límite 12 000 000 COP.

* μ diaria = 0,0381 %, σ diaria = 1,8372 %.
* VaR al 95 % = 9 175 396,76 COP y ES = 11 603 085,60 COP; VaR al 99 % = 13 134 759,28 COP y ES = 15 103 513,56 COP.
* Normalidad: Jarque Bera 0,906, valor p 0,6357 (no se rechaza).
* Backtesting con ventana de 250 días: al 95 %, 11 excepciones en 249 días (12,45 esperadas), valor p 0,6674; al 99 %, 3 excepciones (2,49 esperadas), valor p 0,7530; zona verde en ambos.
* Frontera de riesgo al 99 %: uso del 109,5 % y exceso de 1 134 759 COP (margen −9,5 %). Se alcanzaría el límite con una posición de 91 360 639 COP, una volatilidad de 1,683 %, una confianza de 98,35 % o un horizonte de 8,3 días.

**Matriz de ejemplo** (escenario «Ejemplo multiactivo: 3 activos», activo «Energía ficticia»): horizonte 1 día, posición 100 000 000 COP, límite 4 000 000 COP. VaR al 99 % = 3 728 690,61 COP, ES = 4 313 076,56 COP; uso del límite 93,2 % y margen disponible de 271 309 COP (6,8 %). Backtesting al 99 %: 1 excepción en 249 días, zona verde. Al seleccionar otro activo el cálculo usa su propia columna; la prueba automática 33 lo compara con el cálculo del mismo activo cargado solo.

Todas estas cifras coinciden con un cálculo independiente en Python con NumPy y SciPy (error relativo máximo 1,4e‑13).

## 10. Modo de prueba

Los escenarios están en Acerca de Kíndynos, «Escenarios de prueba»; «Usar un ejemplo» (portada) carga el multiactivo y calcula, y «Cargar ejemplo» (Datos) solo pone su matriz.

| Escenario | Qué muestra |
|---|---|
| Pequeño: 60 precios | Serie corta con ventana de 20 días y aviso de poca potencia. |
| Mediano: 500 precios | Horizonte de 10 días y un límite que se supera al 99 %. |
| Grande con colas gruesas: 2 000 precios | Jarque Bera rechaza la normalidad; Kupiec acepta al 95 % y rechaza al 99 % y 99,5 %. |
| Volatilidad cambiante: 800 precios | Con estimador muestral el backtesting al 99 % cae en zona roja; sirve para comparar con EWMA. |
| Ejemplo multiactivo: 3 activos | Estándar matricial, selector de activo y margen disponible. |
| Parámetros de libro | μ = 0 y σ = 2 %, verificable a mano. |

Los datos se generan con semilla fija, quedan marcados como datos de prueba en pantalla, en el Excel y en el PDF, y los archivos llevan el prefijo `PRUEBA_`.

### Pruebas automáticas

«Ejecutar verificación» corre 37 pruebas y muestra «Motor cuantitativo verificado» con el resultado real; el detalle está en «Ver pruebas técnicas».

1. Φ⁻¹ en 95 %, 97,5 % y 99 %.
2. Φ(Φ⁻¹(p)) = p en 999 puntos.
3. VaR analítico: 46 526,96.
4. Raíz del tiempo: 147 131,16.
5. ES analítico al 99 %: 53 304,28.
6. VaR con media.
7. Lectura de números colombianos e internacionales.
8. Rendimientos logarítmicos y simples.
9. Desviación estándar muestral.
10. Recursión EWMA.
11. Kupiec con x = 5 y x = 0.
12. Valores críticos de chi cuadrado.
13. Zonas de Basilea.
14. Límite inverso.
15. Casos límite de la serie.
16. Tamaños dinámicos.
17. Jarque Bera.
18. Estructura del Excel.
19. Estructura del PDF.
20. Fecha y 1 activo.
21. Fecha y 2 activos.
22. Fecha y 5 activos, con punto y coma y con coma.
23. Orden y 1 activo.
24. Orden y varios activos.
25. Fecha duplicada con mensaje preciso.
26. Faltantes excluidos solo en su activo.
27. Precio negativo con columna y fila.
28. Valor no numérico con columna y fila.
29. Columnas incompletas.
30. Encabezados personalizados y repetidos.
31. Compatibilidad con los archivos de la versión 1.
32. Cambio de activo sin recargar la matriz.
33. Cada selección usa la columna correspondiente.
34. Resultados de la versión 1 reproducidos exactamente.
35. Plantillas CSV y Excel con el mismo estándar.
36. Puntos de partida RiskMetrics y Basilea.
37. Margen dentro, exacto, excedido y sin límite.

## 11. Verificación realizada

* Se congeló como línea base la versión 1.0 publicada. Los 3 575 valores de sus cinco escenarios (VaR, ES, parámetros, normalidad, backtesting y frontera) se reproducen en la versión 2.0 con diferencia cero.
* La aplicación se ejecutó en Chromium en escritorio (1366 px) y teléfono (400 px): 37 de 37 pruebas superadas, consola sin errores ni advertencias y ancho de 400 px sin desplazamiento horizontal.
* Los resultados de los seis escenarios coinciden con NumPy y SciPy (error relativo máximo 1,4e‑13).
* Los seis libros de Excel se recalcularon en LibreOffice: 42 453 fórmulas, cero errores y diferencia relativa máxima de 1e‑12.
* Las plantillas Excel y CSV descargadas se volvieron a cargar por el selector de archivos y se leyeron correctamente.
* Los PDF abren sin errores; cada página tiene marca de agua y pie.

## 12. Limitaciones

* **Normalidad.** El método supone rendimientos normales y subestima el riesgo con colas gruesas. No incluye Cornish Fisher, simulación histórica ni Monte Carlo en esta versión.
* **Raíz del tiempo.** Supone rendimientos independientes e idénticamente distribuidos; la independencia no se prueba estadísticamente.
* **Un activo a la vez.** La matriz admite varios activos, pero no se calcula VaR de portafolio, correlaciones ni contribuciones (VaR marginal, por componente o incremental).
* **Aproximación lineal.** El VaR en dinero es V·VaR%; con rendimientos logarítmicos la pérdida exacta sería V·(1 − e^(−VaR%)).
* **EWMA.** Se inicializa con σ²(2) = r(1)². RiskMetrics supone μ = 0; si se combina EWMA con μ incluida, la media es la muestral.
* **Backtesting.** Solo evalúa el VaR de un día; Kupiec revisa la frecuencia de excepciones, no su agrupamiento (prueba de Christoffersen no incluida). Con menos de 250 días tiene poca potencia.
* **Semáforo.** Se generaliza con la probabilidad binomial acumulada; no se aplican los multiplicadores de capital.
* **Faltantes.** Los rendimientos entre precios válidos no consecutivos abarcan más de un día; si hay muchos faltantes en un activo, su volatilidad diaria puede sobrestimarse.
* **Lectura de Excel.** Requiere conexión para SheetJS; sin conexión se debe usar CSV.
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
