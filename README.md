# EDA de felicidad mundial

Proyecto individual de análisis exploratorio de datos realizado con Python.

## Objetivo

Explorar las diferencias de felicidad entre países y responder:

- ¿Cómo se distribuyen las puntuaciones de felicidad?
- ¿Qué aportes del modelo presentan mayor relación con la puntuación?
- ¿Cómo cambian los resultados al comparar los mismos países entre los cortes 2019 y 2025?

## Fuente y alcance

Utilizo el archivo `WHR26_Data_Figure_2.1.xlsx`, correspondiente al World Happiness Report 2026.

- Histórico: 2.116 registros y 13 columnas originales.
- Etiquetas temporales: 2011–2025, sin 2013.
- Análisis principal: 147 países, periodo 2023–2025.
- Correlaciones: 145 países con los seis factores completos.
- Comparación temporal: 144 países comunes entre los cortes 2019 y 2025.

La etiqueta 2025 identifica el corte del archivo; el informe se publicó en 2026. Las puntuaciones del análisis principal son medias del periodo 2023–2025.

Fuentes:
- [Datos del World Happiness Report](https://www.worldhappiness.report/data-sharing/)
- [Metodología y resultados del informe 2026](https://www.worldhappiness.report/ed/2026/international-evidence-on-happiness-and-social-media/)

### Interpretación de los cortes temporales

La comparación entre los cortes 2019 y 2025 utiliza las puntuaciones
de los períodos 2017–2019 y 2023–2025, publicadas en los informes
2020 y 2026, respectivamente.

El informe publicado en 2019 corresponde a 2016–2018 y no es la
base de esta comparación. Mantengo las etiquetas del Excel en los
nombres de archivos para conservar la trazabilidad.

Fuente del período histórico:
https://www.worldhappiness.report/ed/2020/social-environments-for-world-happiness/

## Estructura

| Ruta | Contenido |
|---|---|
| `notebooks/analisis_felicidad.ipynb` | Código, resultados e interpretaciones |
| `data/raw/` | Excel original |
| `data/processed/` | Datos preparados en CSV |
| `reports/tables/` | Diagnósticos y resultados numéricos |
| `reports/figures/` | Gráficos exportados |
| `requirements.txt` | Versiones de las dependencias |
| `.gitignore` | Exclusión del entorno virtual y archivos temporales |

## Preparación y limpieza

Conservo los datos originales y documento las decisiones:

- Traduzco las columnas y los países al español.
- Añado `pais_original` para conservar la referencia de la fuente.
- Unifico Suazilandia y Esuatini.
- Compruebo tipos de datos, nulos, duplicados y rangos.
- Conservo las 2.116 filas y los 9.879 valores ausentes.
- No imputo valores ni elimino automáticamente los atípicos.
- Detecto atípicos mediante el rango intercuartílico (IQR).
- Mantengo los 147 países para analizar la felicidad.
- Excluyo únicamente del análisis conjunto de factores a dos países con información incompleta: el 1,36 % del corte.
- Exporto CSV con codificación UTF-8 con BOM para facilitar su lectura en Excel.

## Análisis realizado

Incluyo estadísticas descriptivas, histogramas, gráficos de barras, dispersión, correlaciones de Pearson y comparación temporal con países comunes.

También examino:

- La sensibilidad de la correlación del PIB al caso de Venezuela.
- Colombia, España, Argentina, Chile, Marruecos y Ucrania.
- La falta de datos recientes de Cuba en el archivo.
- El índice de Felicidad Nacional Bruta de Bután como contexto metodológico, sin mezclarlo con el ranking mundial.

## Análisis interactivo y estadístico

He incorporado los siguientes apartados al notebook:

- Dos gráficos interactivos con Bokeh: dispersión de apoyo social y
  felicidad, y comparación de las correlaciones de los seis factores.
  Ambos muestran valores al pasar el cursor y se guardan como HTML
  autónomos en `reports/figures/`, consultables sin internet.

- Una simulación del teorema del límite central con 5.000 muestras
  de 30 países, seleccionados con reemplazo. La media de las medias
  simuladas es 5,648, frente a 5,650 en los datos. La desviación de
  las medias simuladas y el error estándar teórico son 0,207,
  redondeados a tres decimales.

- Una prueba bilateral de independencia mediante 10.000 permutaciones
  entre el aporte del apoyo social y la felicidad, con 145 países.
  Obtengo r = 0,8119 y un p-value estimado de 0,000100. Rechazo la
  hipótesis nula bajo los supuestos de la prueba. El resultado es
  exploratorio: seleccioné el factor después de examinar las
  correlaciones y la intercambiabilidad entre países es una limitación.

- Una regresión lineal simple:
  felicidad ajustada = 2,5278 + 2,6032 × aporte del apoyo social.
  Obtengo R² = 0,6591 y RMSE = 0,6652 sobre los datos utilizados
  para ajustar la recta. Reviso visualmente los residuos y documento
  sus patrones y valores extremos. No considero estas métricas
  una validación predictiva en datos nuevos.

Estos ejercicios no demuestran causalidad. La prueba de permutaciones
evalúa la asociación entre apoyo social y felicidad, no la significación
de los cambios temporales entre 2019 y 2025.

## Principales resultados

- La media de los 147 países es 5,650 y la mediana, 5,937.
- Finlandia registra 7,764 puntos y Afganistán, 1,446.
- El aporte del apoyo social presenta la mayor correlación con la felicidad (0,812), seguido del aporte del PIB (0,745).
- Entre los 144 países comunes, el cambio medio entre los cortes 2019 y 2025 es de +0,099 puntos.
- Aumentan su puntuación 90 países y la reducen 54.
- Al excluir Venezuela, la correlación entre aporte del PIB y felicidad pasa de 0,7455 a 0,7992. Mantengo Venezuela en el análisis principal.

## Cómo ejecutar el proyecto

Entorno utilizado: Python 3.13 y Visual Studio Code con las extensiones Python y Jupyter.

1. Descarga o clona el repositorio y abre su carpeta principal en VS Code.
2. Abre una terminal en esa carpeta.
3. Crea el entorno virtual:

```powershell
python -m venv .venv
```

4. Instala las dependencias en Windows:

```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

5. Abre `notebooks/analisis_felicidad.ipynb`.
6. Selecciona `.venv` como kernel.
7. Pulsa **Reiniciar** y después **Ejecutar todo**.

## Organización del trabajo con Git

Desarrollo el proyecto de forma individual y registro los avances
mediante commits descriptivos.

- `analisis-felicidad`: rama donde realicé la limpieza y los análisis.
- `develop`: rama donde completé la documentación y preparé la entrega.
- `main`: contiene la versión integrada del proyecto y el informe ejecutivo.

En macOS o Linux, utiliza `.venv/bin/python` en lugar de `.\.venv\Scripts\python.exe`.

## Limitaciones

- Las medias entre países no están ponderadas por población.
- Las medias nacionales no describen las desigualdades internas.
- Los seis factores son aportes del modelo explicativo, no indicadores brutos.
- Las correlaciones no demuestran causalidad.
- La cobertura cambia entre cortes; la comparación temporal utiliza países comunes.
- No se ha evaluado la significación estadística de los cambios.
- El origen metodológico de los ceros del aporte del PIB de Venezuela sigue pendiente de aclaración.
- Queda pendiente precisar documentalmente algunos periodos históricos, incluido el registro de Cuba etiquetado como 2011.
- El índice de Bután no es directamente comparable con la puntuación del World Happiness Report.

## Estado de la entrega

El repositorio está publicado en GitHub e incluye el notebook explicado, los datos preparados, los gráficos estáticos e interactivos y el [informe ejecutivo en PDF](reports/informe_ejecutivo_felicidad.pdf). Los avances están integrados en `main`; conservo las ramas `develop` y `analisis-felicidad` para documentar el desarrollo. He comprobado en GitHub la visualización del notebook con sus gráficos y las seis páginas del informe ejecutivo.