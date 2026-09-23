# Kíndynos VaR

**Riesgo de mercado. Mida el riesgo. Evalúe el modelo. Explore sus límites.**

Aplicación web académica para la estimación y el diagnóstico del riesgo de mercado mediante VaR paramétrico, Expected Shortfall, backtesting y análisis de robustez. El VaR estima un umbral de pérdida que solo debería excederse con una probabilidad determinada durante el horizonte elegido.

**Abrir la aplicación:** https://mgomezr1.github.io/kindynos-VaR/

No requiere instalación ni registro. Funciona en cualquier navegador moderno, en computador, tableta o teléfono.

## Qué permite hacer

- Cargar una matriz de precios con el estándar de datos Kíndynos (fecha u orden y una columna por activo, de 1 a n activos) desde CSV, Excel o pegado, con plantillas descargables, validación detallada y vista previa; o ingresar una media y una volatilidad conocidas.
- Elegir el activo a analizar sin volver a cargar los datos.
- Partir de configuraciones con respaldo metodológico (RiskMetrics o Basilea) o definir la propia: rendimientos logarítmicos o simples, volatilidad muestral o EWMA, rendimiento esperado incluido o μ = 0.
- Obtener el resumen de riesgo (exposición, VaR, Expected Shortfall y uso del límite) con una lectura narrativa del resultado.
- Diagnosticar el modelo: normalidad con Jarque Bera y excepciones dentro de la muestra.
- Validar el modelo con backtesting, la prueba de Kupiec y el semáforo del Comité de Basilea.
- Explorar la frontera de riesgo: qué posición, volatilidad, confianza u horizonte llevarían el VaR al límite, y el margen disponible.
- Descargar un modelo auditable en Excel, con fórmulas reales, y un informe ejecutivo en PDF.

## Cómo usarla

1. **Método:** revise qué calcula Kíndynos y sus supuestos.
2. **Modelo:** defina posición, horizonte, confianza y límite; ajuste la configuración avanzada si lo necesita, y confirme.
3. **Datos:** descargue la plantilla, llénela y cárguela (o use el ejemplo); revise la validación y elija el activo. Pulse **Calcular el VaR**.
4. **Riesgo** y **Robustez:** lea el resumen, el diagnóstico, el backtesting y la frontera de riesgo.
5. **Informe:** descargue el Excel o el PDF.

«Usar un ejemplo» y los escenarios de prueba de «Acerca de Kíndynos» cargan datos ficticios para practicar; los archivos exportados con esos datos llevan el prefijo PRUEBA_.

## Privacidad

Todos los cálculos se hacen en su navegador. Los datos no se envían a ningún servidor ni se guardan: al cerrar o recargar la página se pierden, así que descargue el Excel o el PDF antes de salir.

## Requisitos

- Navegador actualizado (Chrome, Edge, Firefox o Safari) con JavaScript activo.
- Conexión a internet para el Excel de resultados, la plantilla Excel y la lectura de archivos Excel, porque la aplicación descarga la biblioteca SheetJS. El PDF, la plantilla CSV y la lectura de CSV funcionan sin conexión.

## Documentación

La guía técnica (estándar de datos, fórmulas, funciones, exportaciones, verificación y limitaciones) está en [docs/Guia_Kindynos_VaR.md](docs/Guia_Kindynos_VaR.md).

## Validación

La aplicación incluye 37 pruebas automáticas («Acerca de Kíndynos», «Ejecutar verificación»). La versión 2.0 reproduce exactamente las cifras de la versión 1.0, coincide con un cálculo independiente en Python (SciPy) y sus libros de Excel se recalculan sin errores en LibreOffice.

## Cómo citar

Gómez Rueda, M. S. (2026). *Kíndynos VaR* (Versión 2.0) [Software]. https://mgomezr1.github.io/kindynos-VaR/

## Autoría y uso

Este aplicativo fue desarrollado por Mario Sergio Gómez Rueda. Su uso es de carácter académico y cualquier otro uso se regirá por el derecho de la propiedad intelectual. Consulte los términos en [LICENSE.md](LICENSE.md). Sugerencias o dudas: mgomezr1@gmail.com

## Fundamento metodológico

- Basel Committee on Banking Supervision. (1996). *Supervisory framework for the use of "backtesting" in conjunction with the internal models approach to market risk capital requirements*. Bank for International Settlements.
- Hull, J. C. (2018). *Risk management and financial institutions* (5th ed.). Wiley.
- Jarque, C. M., & Bera, A. K. (1987). A test for normality of observations and regression residuals. *International Statistical Review, 55*(2), 163–172. https://doi.org/10.2307/1403192
- Jorion, P. (2007). *Value at risk: The new benchmark for managing financial risk* (3rd ed.). McGraw-Hill.
- J.P. Morgan & Reuters. (1996). *RiskMetrics: Technical document* (4th ed.). Morgan Guaranty Trust Company.
- Kupiec, P. H. (1995). Techniques for verifying the accuracy of risk measurement models. *The Journal of Derivatives, 3*(2), 73–84. https://doi.org/10.3905/jod.1995.407942

## Componentes de terceros

La exportación a Excel y la lectura de archivos Excel usan [SheetJS Community Edition](https://sheetjs.com), distribuida bajo la licencia Apache 2.0 y cargada desde cdn.sheetjs.com.
