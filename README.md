# Kíndynos VaR

Aplicación web académica para calcular el **valor en riesgo (VaR) paramétrico** de una posición en un activo: estime la volatilidad a partir de una serie de precios, obtenga el VaR y el Expected Shortfall con el nivel de confianza y el horizonte que elija, y valide el modelo con backtesting.

**Abrir la aplicación:** https://mgomezr1.github.io/kindynos-var/

No requiere instalación ni registro. Funciona en cualquier navegador moderno, en computador, tableta o teléfono.

## Qué permite hacer

- Pegar una serie de precios o cargar un CSV, con revisión de calidad en vivo; o ingresar directamente la media y la volatilidad diarias.
- Elegir rendimientos logarítmicos o simples, volatilidad muestral o EWMA (RiskMetrics), y VaR con media o con media cero.
- Calcular el VaR y el Expected Shortfall para varios niveles de confianza y cualquier horizonte con la regla de la raíz del tiempo.
- Revisar el supuesto de normalidad con asimetría, curtosis y la prueba de Jarque Bera.
- Validar el modelo con backtesting, la prueba de Kupiec y el semáforo del Comité de Basilea.
- Saber qué posición, volatilidad, nivel de confianza u horizonte llevarían el VaR a un límite de riesgo.
- Descargar los resultados en Excel, con fórmulas reales, y un informe ejecutivo en PDF.

## Cómo usarla

1. Lea la sección «El método» y mueva el deslizador de la portada para ver cómo cambia la cola de pérdida.
2. En **Configuración**, escriba el activo, la posición, el horizonte y los niveles de confianza, elija las variantes y confirme.
3. En **Datos**, pegue la serie de precios o cargue un archivo CSV, o escriba la media y la volatilidad. Pulse **Calcular el VaR**.
4. Revise los **Resultados** y la **Robustez**: normalidad, backtesting y límite.
5. Descargue el **Excel** o el **informe PDF**.

El **modo de prueba** carga cinco escenarios con datos ficticios para practicar; esos datos no describen un activo real y los archivos exportados llevan el prefijo PRUEBA_.

## Privacidad

Todos los cálculos se hacen en su navegador. Los datos no se envían a ningún servidor ni se guardan: al cerrar o recargar la página se pierden, así que descargue el Excel o el PDF antes de salir.

## Requisitos

- Navegador actualizado (Chrome, Edge, Firefox o Safari) con JavaScript activo.
- Conexión a internet para exportar a Excel, porque la aplicación descarga la biblioteca SheetJS. El informe PDF funciona sin conexión.

## Documentación

La guía completa (método, fórmulas, funciones, exportaciones, verificación y limitaciones) está en [docs/Guia_Kindynos_VaR.md](docs/Guia_Kindynos_VaR.md).

## Cómo citar

Gómez Rueda, M. S. (2026). *Kíndynos VaR* (Versión 1.0) [Software]. https://mgomezr1.github.io/kindynos-var/

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

La exportación a Excel usa [SheetJS Community Edition](https://sheetjs.com), distribuida bajo la licencia Apache 2.0 y cargada desde cdn.sheetjs.com.
