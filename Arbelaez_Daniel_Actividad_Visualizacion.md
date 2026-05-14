div align"center"

---

br

#  Visualización de Evolución en el Tiempo

### Pipeline ETL con OpenRefine  Dashboard en Power BI

br

---

  
:---:---
 **Estudiante**  Daniel Arbeláez Álvarez 
 **Asignatura / Módulo**  [Nombre de la asignatura] 
 **Fecha de entrega**  [Fecha de entrega] 
 **Herramientas**  OpenRefine 3.7 · Power BI Desktop · DAX · GREL 
 **Dataset**  `datos_ventas.csv` — 18,000 filas · 15 columnas 
 **Transformaciones ETL**  100 operaciones registradas (Undo/Redo 100/100) 
 **Visualizaciones**  5 gráficos · 3 medidas DAX · Tabla Calendario 

br

 *Proyecto de análisis de datos aplicado: limpieza, normalización y visualización temporal  
 de un dataset de ventas comerciales usando herramientas de Business Intelligence.*

br

---

/div

---

## Índice de Contenidos
1. [Descripción del Dataset Utilizado](#1-descripción-del-dataset-utilizado)
2. [Fase 1: Limpieza y Transformación de Datos en OpenRefine](#2-fase-1-limpieza-y-transformación-de-datos-en-openrefine)
3. [Fase 2: Visualización de Datos en Power BI](#3-fase-2-visualización-de-datos-en-power-bi)
4. [Análisis e Interpretación de Resultados](#4-análisis-e-interpretación-de-resultados)

---

## 1. Descripción del Dataset Utilizado
* **Origen de los datos:** Archivo `datos_ventas.csv`
* **Dimensiones:** 18,000 filas.
* **Contenido principal:** Registros de ventas que incluyen variables categóricas (Categoría, Ciudad, Método de Pago, Estado) y variables cuantitativas (Precio Unitario, Cantidad).
* **Objetivo:** Preparar estos datos eliminando inconsistencias (como caracteres especiales `*`, `?`, `_`) para analizar su evolución temporal en un dashboard.

p align"center"img src"assets/screenshots/001%20Acción%20previa%20Crear%20el%20Proyecto.png" alt""/p

---

## 2. Fase 1: Limpieza y Transformación de Datos en OpenRefine

### 2.1 Identificación de Problemas (Exploración y Diagnóstico)
El dataset cargado en OpenRefine (18,000 filas) mostró caracteres sucios visibles desde la primera pantalla: `wait*`, `*democratic`, `door_` en `Producto`; `Jodichester?` en `Ciudad`; `*whom`, `_whom`, `*leader`, `budget?` en `Categoria`; fechas con `*` y `?` en `Fecha_Venta`; emails con `*` al inicio. Se aplicaron Facets para medir el alcance real antes de transformar.

####  Columnas Categóricas — Text Facets

*   **Paso 1.1 - Categoria:** El `Text facet` devolvió **227 opciones únicas** en una columna que debería tener 30 valores. Cada variante sucia (`*whom`, `_whom`, `budget?`, `individual_`, `*job`, `*leader`) contaba como un valor independiente. Se usó `Cluster and edit` con Key collision y función Fingerprint para agrupar y unificar.

    p align"center"img src"assets/screenshots/002%20Text%20Facet%20en%20columna%20Categoria.png" alt""/p

    p align"center"img src"assets/screenshots/003%20Seleccion%20de%20cluster.png" alt""/p

    El diálogo de clustering identificó 28 grupos de variantes. Se confirmaron los merges.

    p align"center"img src"assets/screenshots/004%20Se%20refina%20los%20datos%20de%20la%20columna.png" alt""/p

    `Categoria` quedó en **30 valores canónicos** (`at`, `budget`, `individual`, `job`, `leader`, `TV`, `whom`).

    p align"center"img src"assets/screenshots/005%20Resultado%20categorias%20unificadas.png" alt""/p

*   **Paso 1.2 - Ciudad:** El `Text facet` post-limpieza confirma 30 ciudades sin signos de interrogación. Los valores sucios (`Jodichester?`, `East Brittany?`) visibles en la captura inicial fueron eliminados con las transformaciones GREL.

    p align"center"img src"assets/screenshots/006%20Text%20Facet%20en%20columna%20Ciudad.png" alt""/p

*   **Paso 1.3 - Metodo_Pago:** Resultado post-limpieza: 2 valores canónicos, `Efectivo` (8,383 registros) y `Tarjeta` (9,617). Sin variantes sucias residuales.

    p align"center"img src"assets/screenshots/007%20Text%20Facet%20en%20columna%20Metodo_Pago.png" alt""/p

*   **Paso 1.4 - Estado:** Resultado post-limpieza: `En camino` (6,593) y `Entregado` (11,407). Los valores corruptos `En?cami` y `En cam*` fueron corregidos con GREL.

    p align"center"img src"assets/screenshots/008%20Text%20Facet%20en%20columna%20Estado.png" alt""/p

####  Columnas Numéricas — Numeric Facets

*   **Paso 1.5 - Precio_Unitario:** El `Numeric facet` no permitía transformaciones directas en esta columna. Se usó `Text facet`, que confirmó 18 valores numéricos únicos (de 200 a 3000) sin texto mezclado.

    p align"center"img src"assets/screenshots/009%20Text%20Facet%20en%20columna%20Precio_Unitario.png" alt""/p

*   **Paso 1.6 - Cantidad:** Mismo caso que `Precio_Unitario`. Se aplicó primero `value.replace(/\s/,"")` para eliminar espacios embebidos, luego `Text facet` para validar. Resultado: 23 enteros limpios (del 2 al 30).

    p align"center"img src"assets/screenshots/010%20Text%20Facet%20en%20columna%20cantidad%20-%20function.png" alt""/p

    p align"center"img src"assets/screenshots/011%20Text%20Facet%20en%20columna%20cantidad.png" alt""/p

####  Columna Email — Clustering por n-Gram

*   **Paso 1.7 - Email:** `Cluster and edit` con Key collision y n-Gram fingerprint (tamaño 1) detectó **29 clusters** donde el `@` había sido reemplazado por un espacio (ej: `schmidtkrystal@example.org` vs `schmidtkrystal example.org`). Se consolidaron todos al valor correcto.

    p align"center"img src"assets/screenshots/012%20Text%20Facet%20en%20columna%20Email%20-%20n-Gram.png" alt""/p

#### ️ Valores Nulos — Facet by Blank

*   **Paso 1.8 - Nulos:** `Facet by blank` en todas las columnas críticas midió el volumen de datos faltantes.

#### Documentación y Justificación del Diagnóstico

**Paso realizado:** Exploración mediante Text Facets en columnas categóricas (`Categoria`, `Ciudad`, `Metodo_Pago`, `Estado`).
**Ruta:** `Encabezado de columna   Facet  Text facet`
**Justificación lógica:** El Text Facet genera un inventario de todos los valores únicos de una columna. Un valor como `_Efectivo` y `Efectivo` se ven casi iguales en pantalla, pero Power BI los trata como categorías distintas: aparecen dos barras donde debería haber una. Esta vista expone esos errores antes de que contaminen cualquier visualización.

**Paso realizado:** Exploración de columnas numéricas mediante Numeric Facets (`Precio_Unitario`, `Cantidad`).
**Ruta:** `Encabezado de columna   Facet  Numeric facet`
**Justificación lógica:** El Numeric Facet genera un histograma con una barra separada para valores "Non-numeric". Si `Precio_Unitario` o `Cantidad` contienen texto mezclado, Power BI no puede sumarlos ni calcular promedios. Detectar esos casos antes de importar evita errores silenciosos en las medidas DAX.

**Paso realizado:** Identificación de celdas vacías mediante Facet by Blank.
**Ruta:** `Encabezado de columna   Facet  Customized facets  Facet by blank`
**Justificación lógica:** Cuantifica los datos faltantes por columna. Un nulo en un segmentador de Power BI genera una fila en blanco; en una medida DAX puede devolver BLANK donde se espera un número. Conocer cuántos hay define la estrategia: eliminar filas, imputar un valor por defecto, o etiquetar como "Sin dato".

### 2.2 Transformaciones Aplicadas (Corrección y Normalización)

---

####  Fase 2 — Normalización de Espacios y Capitalización

#### Paso 2.1 — Eliminar espacios al inicio/final en columnas categóricas

**Paso realizado:** Eliminación de espacios en blanco al inicio y al final en columnas categóricas (`Producto`, `Ciudad`, `Categoria`, `Cliente`, `Metodo_Pago`, `Estado`)

**Ruta o Comando GREL:** `Encabezado de columna   Edit cells  Common transforms  Trim leading and trailing whitespace`

**Justificación lógica:** Se detectó mediante Text Facets la presencia de espacios en blanco al inicio y/o final de valores en múltiples columnas categóricas. Estos espacios invisibles hacen que Power BI interprete `" Efectivo"` y `"Efectivo"` como dos categorías completamente distintas, generando duplicados en gráficas de barras, segmentadores y tablas dinámicas. La función `trim()` elimina estos espacios superfluos, garantizando la unicidad real de cada categoría y la integridad de los filtros en el modelo de datos.

---

#### Paso 2.2 — Colapsar espacios internos múltiples

**Paso realizado:** Colapso de espacios internos múltiples en columnas de texto libre (`Ciudad`, `Cliente`, `Direccion`)

**Ruta o Comando GREL:** `Edit cells  Transform...  value.replace(/\s/, " ")`

**Justificación lógica:** Se identificaron celdas con doble o triple espacio entre palabras, producto de errores de captura manual de datos. Un doble espacio, aunque visualmente similar, genera una cadena de texto diferente que impide la correcta agrupación en Power BI. La expresión regular `/\s/` captura uno o más espacios consecutivos y los reemplaza por un único espacio normalizado, asegurando la consistencia de los valores de texto para operaciones de agrupación, búsqueda y filtrado.

   p align"center"img src"assets/screenshots/013%20Colapsar%20espacios%20internos%20múltiples.png" alt""/p

---

#### Paso 2.3 — Normalizar texto a formato Título (`toTitlecase()`)

**Paso realizado:** Normalización de texto a formato Título (`toTitlecase()`) en columnas de nombres propios (`Producto`, `Ciudad`, `Cliente`, `Categoria`)

**Ruta o Comando GREL:** `Edit cells  Transform...  value.toTitlecase()`

**Justificación lógica:** Se encontraron valores con inconsistencias de capitalización. La captura muestra la transformación sobre `Producto` (`owner  Owner`, `education  Education`, `strategy  Strategy`). La función `toTitlecase()` coloca mayúscula únicamente en la primera letra de cada palabra. Esta normalización es necesaria porque el motor de Power BI distingue entre mayúsculas y minúsculas al agrupar datos; sin ella, un mismo producto o ciudad aparecería varias veces con nombres distintos en los gráficos.

   p align"center"img src"assets/screenshots/014%20Normalizar%20texto%20a%20formato%20Título.png" alt""/p

---

####  Normalización de Capitalización Específica

#### Paso 2.4 — Normalizar texto a minúsculas (`toLowercase()`) en `Email` y `Metodo_Pago`

**Paso realizado:** Normalización de texto a minúsculas (`toLowercase()`) en las columnas `Email` y `Metodo_Pago`

**Ruta o Comando GREL:** `Edit cells  Transform...  value.toLowercase()`

**Justificación lógica:** Los correos electrónicos son por estándar técnico (RFC 5321) insensibles a mayúsculas, pero como cadena de texto son tratados como valores distintos: `"Pedro@Example.com"` y `"pedro@example.com"` no se consolidarían como el mismo cliente. La misma lógica aplica a `Metodo_Pago`, donde `Efectivo` y `efectivo` deben converger en un único valor canónico. La captura muestra la transformación sobre `Metodo_Pago` (`Efectivo  efectivo`, `Tarjeta  tarjeta`).

   p align"center"img src"assets/screenshots/015%20Normalización%20de%20texto%20a%20minúsculas.png" alt""/p

---

####  Eliminación de Caracteres Especiales

#### Paso 2.5 — Eliminar caracteres especiales sucios (`*`, `_`, `?`) en columnas categóricas

**Paso realizado:** Eliminación de caracteres especiales no válidos (`*`, `_`, `?`) al inicio y final de valores en columnas categóricas (`Producto`, `Ciudad`, `Categoria`, `Metodo_Pago`, `Estado`, `Cliente`, `Email`, `Fecha_Venta`)

**Ruta o Comando GREL:** `Edit cells  Transform...  value.replace(/^[*_?][*_?]$/, "").trim()`

**Justificación lógica:** Durante la exploración con Text Facets se identificaron valores contaminados con caracteres especiales (`*`, `_`, `?`) en posiciones de inicio o final de la cadena (ej: `*Tarjeta`, `_Efectivo`, `budget?`). Estos caracteres son producto de errores en el sistema de origen o en la exportación del archivo. La expresión regular `^[*_?][*_?]$` apunta específicamente a estas posiciones sin alterar el contenido central del dato. Su eliminación es crítica porque Power BI los trataría como categorías únicas y distintas, rompiendo cualquier análisis comparativo o de tendencias.

p align"center"img src"assets/screenshots/016%20Eliminar%20caracteres%20especiales%20sucios.png" alt""/p

---

### 2.3 Transformaciones Avanzadas (Tipos y Nulos)

#### ️ Fase 4 — Conversión de Tipos de Dato y Manejo de Nulos

#### Paso 4.1 — Convertir `Fecha_Venta` a tipo Fecha

**Paso realizado:** Conversión de `Fecha_Venta` al tipo dato Fecha

**Ruta:** `Encabezado de columna   Edit cells  Common transforms  To date`

**Justificación lógica:** OpenRefine almacena las fechas como texto por defecto. Si `Fecha_Venta` llega a Power BI como texto, el modelo no puede construir jerarquías de tiempo (año/trimestre/mes) ni usar funciones de inteligencia temporal en DAX como `DATESYTD` o `DATEADD`. La conversión aquí garantiza que Power BI la reconozca como dimensión de tiempo al importar, sin pasos adicionales en Power Query.

p align"center"img src"assets/screenshots/017%20Limpiar%20y%20convertir%20Fecha_Venta%20al%20tipo%20fecha.png" alt""/p

Los valores en verde con formato ISO (`2023-11-20T00:00:00Z`) confirman que OpenRefine reconoció el tipo correctamente. Power BI interpreta ese formato sin configuración adicional.

p align"center"img src"assets/screenshots/018%20Fecha_Venta%20convertido.png" alt""/p

---

#### Paso 4.2 — Imputar valores nulos en `Comentario`

**Paso realizado:** Sustitución de valores nulos en `Comentario` con el texto `"Sin comentario"`

**Ruta / GREL:** `Edit cells  Transform...  if(isBlank(value), "Sin comentario", value)`

**Justificación lógica:** Los nulos en columnas de texto generan filas en blanco en los segmentadores de Power BI y comportamiento inesperado en tablas. Reemplazarlos con una etiqueta explícita convierte el nulo en un valor manejable: aparece como categoría en los filtros en lugar de desaparecer silenciosamente.

p align"center"img src"assets/screenshots/019%20Manejo%20de%20valores%20nulos%20en%20Comentario.png" alt""/p

---

### 2.4 Exportación del Dataset Limpio

####  Fase 5 — Exportación Final a Power BI

#### Paso 5.1 — Eliminar todos los filtros activos

**Paso realizado:** Eliminación de todos los Facets activos antes de exportar

**Ruta:** `Panel Facet/Filter  Remove All`

**Justificación lógica:** Si algún Facet queda activo al exportar, OpenRefine solo exporta las filas visibles. El archivo se descarga sin advertencias pero con menos de 18,000 registros. El contador superior debe mostrar `18,000 rows` antes de proceder.

#### Paso 5.2 — Exportar como CSV

**Paso realizado:** Exportación del dataset limpio en formato CSV

**Ruta:** `Export  Comma-separated value (CSV)`

**Justificación lógica:** El CSV es el estándar universal de intercambio de datos tabulares, compatible con Power BI sin transformaciones adicionales. Power Query detecta los tipos automáticamente y las fechas en formato ISO (`2023-11-20T00:00:00Z`) son reconocidas sin configuración extra.

p align"center"img src"assets/screenshots/020%20Exportar%20como%20CSV.png" alt""/p

---

### Resumen Ejecutivo — Proceso ETL Completado

 Fase  Descripción  Acciones principales 
-----------------------------------------
 **Fase 1**  Exploración y Diagnóstico  Text Facets, Numeric Facets, Facet by Blank 
 **Fase 2**  Limpieza básica  `trim()`, `replace(/\s/," ")`, `toTitlecase()`, `toLowercase()`, limpieza de `*`, `_`, `?` 
 **Fase 3**  Clustering  Key Collision (fingerprint, n-gram), Nearest Neighbor (levenshtein) en 5 columnas 
 **Fase 4**  Transformaciones avanzadas  `To number`, `To date` en `Fecha_Venta`, imputación de nulos en `Comentario` 
 **Fase 5**  Exportación  Remove All  Export CSV  verificación en Power BI 

**Total de transformaciones registradas: 100 operaciones (Undo/Redo 100/100)**

## 3. Fase 2: Visualización de Datos en Power BI

### 3.0 Importación y Modelado en Power Query

####  A — Carga y Verificación de Tipos en Power Query

El CSV exportado de OpenRefine se importó a Power BI mediante `Obtener datos  Texto/CSV`. La vista previa de Power Query confirmó las 18,000 filas completas con tipos detectados automáticamente.

p align"center"img src"assets/screenshots/021%20Cargar%20dataset%20en%20power%20bi.png" alt""/p

En el editor de Power Query se aplicaron dos ajustes antes de cargar al modelo: se marcó `ID_Venta` como clave de la tabla (`Table.AddKey`) para optimizar relaciones y evitar duplicados en cruces, y se verificaron los tipos de cada columna.

p align"center"img src"assets/screenshots/022%20Transformacion%20ID_Venta%20como%20clave.png" alt""/p

Tipos confirmados: `ID_Venta` (entero, clave), `Precio_Unitario` y `Cantidad` (número), `Fecha_Venta` (Fecha/Hora), resto como texto. Con los tipos correctos, las medidas DAX operan sin conversiones implícitas.

p align"center"img src"assets/screenshots/023%20Tipo%20de%20dato%20de%20cada%20columna.png" alt""/p

---

### 3.1 Modelado DAX

####  B — Tabla Calendario y Dimensión Temporal

#### Tabla Calendario

Las funciones de inteligencia temporal de DAX (`TOTALYTD`, `SAMEPERIODLASTYEAR`, `PREVIOUSMONTH`) requieren una tabla de fechas marcada e independiente. Sin ella, calculan resultados incorrectos. Se creó mediante `Inicio  Nueva tabla`:

```dax
Calendario  
CALENDAR(
    MIN(Ventas[Fecha_Venta]),
    MAX(Ventas[Fecha_Venta])
)
```

p align"center"img src"assets/screenshots/024%20Tabla%20Calendario.png" alt""/p

Columnas calculadas añadidas a la tabla:

```dax
Año        YEAR(Calendario[Date])
Mes        MONTH(Calendario[Date])
NombreMes  FORMAT(Calendario[Date], "MMMM")
Trimestre  "Q" & QUARTER(Calendario[Date])
```

 `NombreMes` debe ordenarse por la columna `Mes` (`Herramientas de columna  Ordenar por columna`) para que todos los ejes temporales sigan la secuencia cronológica (enero, febrero…) y no la alfabética (abril, agosto…).

Relación creada: `Calendario[Date]`  `Ventas[Fecha_Venta]` (muchos a uno, dirección única).

p align"center"img src"assets/screenshots/025%20Columnas%20calculadas.png" alt""/p

---

####  C — Medidas DAX

**Medida 1 — Ingresos Totales**

```dax
Ingresos_Totales  
SUMX(
    Ventas,
    Ventas[Precio_Unitario] * Ventas[Cantidad] * (1 - Ventas[Descuento] / 100)
)
```

`SUMX` aplica el descuento fila por fila antes de sumar. La división `/100` es necesaria porque la columna `Descuento` almacena porcentajes como enteros (ej: `21`  21%), no como decimales (`0.21`). Sin esta corrección, `(1 - 21)  -20` convierte todos los ingresos en valores negativos de varios miles de millones.

**Medida 2 — Variación vs. Período Anterior**

```dax
Variacion_vs_PeriodoAnterior  
VAR VentasActual    [Ingresos_Totales]
VAR VentasAnterior  CALCULATE(
    [Ingresos_Totales],
    PREVIOUSMONTH(Calendario[Date])
)
RETURN
DIVIDE(VentasActual - VentasAnterior, VentasAnterior, 0)
```

El tercer argumento de `DIVIDE` devuelve 0 en lugar de error cuando el período anterior no tiene datos.

**Medida 3 — Ticket Promedio**

```dax
Ticket_Promedio  
DIVIDE(
    [Ingresos_Totales],
    DISTINCTCOUNT(Ventas[ID_Venta]),
    0
)
```

**Formato aplicado a las medidas:**
- `Ingresos_Totales`  Moneda, 2 decimales  resultado: **$241.09 mill.**
- `Ticket_Promedio`  Número, 2 decimales  resultado: **8.04 mill.**
- `Variacion_vs_PeriodoAnterior`  Porcentaje, 2 decimales  resultado: **84.78%**

p align"center"img src"assets/screenshots/032%20Medidas%20DAX%20resultado%20final%20con%20formato.png" alt""/p

---

####  D — Ajustes y Resolución de Errores del Modelo

Durante la construcción del modelo se identificaron y corrigieron tres problemas:

**Ajuste 1 — Tabla Calendario marcada como tabla de fechas**

La tabla Calendario requiere ser declarada explícitamente como tabla de fechas para que las funciones de inteligencia temporal (`PREVIOUSMONTH`, `TOTALYTD`) funcionen correctamente. Se activó desde `Vista Modelo  clic derecho en Calendario  Marcar como tabla de fechas  columna: Date`.

p align"center"img src"assets/screenshots/027%20Tabla%20Calendario%20marcada%20como%20tabla%20de%20fechas.png" alt""/p

**Ajuste 2 — Medidas organizadas en carpeta dentro de tabla Ventas**

Las tres medidas se crearon inicialmente en la tabla Calendario. Se movieron a la tabla `Ventas` y se agruparon en una carpeta `Medidas` para mantener el modelo organizado y facilitar su localización en el panel de campos.

p align"center"img src"assets/screenshots/028%20Medidas%20organizadas%20en%20tabla%20Ventas.png" alt""/p

**Ajuste 3 — Corrección de escala en columna `Descuento`**

La versión inicial de `Ingresos_Totales` arrojó **-1,266,909,200** y **-$19.56 mil M** — valores negativos de magnitud incoherente con el dataset.

p align"center"img src"assets/screenshots/026%20KPI%20resultado%20inicial%20negativo.png" alt""/p

p align"center"img src"assets/screenshots/029%20Error%20Ingresos_Totales%20descuento%20entero.png" alt""/p

El diagnóstico reveló que `Descuento` almacena porcentajes como enteros (`21`, `87`, `12`…), no como proporciones decimales (`0.21`, `0.87`). La fórmula original calculaba `(1 - 21)  -20`, un multiplicador negativo aplicado a cada fila. La corrección fue agregar `/100` en la fórmula, que convierte `21  0.21` antes del cálculo. Tras el ajuste, `Ingresos_Totales` y `Ticket_Promedio` devuelven valores positivos y coherentes con el rango de precios del dataset: **$241.09 mill.** en ingresos totales y **8.04 mill.** de ticket promedio sobre las 18,000 transacciones.

p align"center"img src"assets/screenshots/031%20Ingresos_Totales%20corregido%20resultado%20final.png" alt""/p

**Ajuste 4 — Formato de porcentaje en `Variacion_vs_PeriodoAnterior`**

La medida devolvía `0.85` en lugar de `85%`. Se aplicó formato de porcentaje desde `Herramientas de medición  Formato de datos  Porcentaje`, con 2 posiciones decimales. El resultado final de las 3 medidas con sus formatos definitivos: `Ingresos_Totales` **$241.09 mill.**, `Ticket_Promedio` **8.04 mill.** y `Variacion_vs_PeriodoAnterior` **84.78%**.

p align"center"img src"assets/screenshots/032%20Medidas%20DAX%20resultado%20final%20con%20formato.png" alt""/p

**Ajuste 5 — Corrección de tipo de dato en `Fecha_Venta` (DateTime  Date)**

La tabla de verificación mostraba una sola fila con `NombreMes` en blanco y `Variacion_vs_PeriodoAnterior`  0.00%: síntoma de una relación que existía pero no encontraba coincidencias. La causa: el CSV exportado tenía fechas en formato ISO con componente horario (`2023-11-20T00:00:00Z`), importadas por Power BI como tipo **Fecha/Hora**. La tabla `Calendario` genera valores de tipo **Fecha** puro. En Power BI, `Date  DateTime` — la relación nunca hacía match aunque estuviera correctamente definida.

La corrección se aplicó en Power Query (`Transformar  Tipo de datos: Fecha`), generando el paso `Table.TransformColumnTypes(..., {{"Fecha_Venta", type date}})`. Como el componente horario siempre era `T00:00:00Z`, no se perdió ningún dato. Tras `Cerrar y aplicar`, la relación comenzó a funcionar y el modelo cargó correctamente por mes.

p align"center"img src"assets/screenshots/039%20Fix%20Fecha_Venta%20por%20tipo%20Fecha.png" alt""/p

---

### 3.1 Capturas del Dashboard

p align"center"img src"assets/screenshots/041%20Dashboard%20finalizado.png" alt""/p

---

### 3.2 Descripción de las Visualizaciones Creadas

####  E — Construcción de Visualizaciones del Dashboard

#### 1. Evolución de Ingresos por Categoría (Gráfico de Líneas Multi-Serie)

**Variables usadas:** Eje X: `NombreMes` / `Año` (tabla Calendario) · Eje Y: `Ingresos_Totales` · Leyenda: `Categoria`

**Justificación visual y psicológica:** El gráfico de líneas es la codificación óptima para datos temporales continuos porque usa el canal perceptivo de posición en eje común, el de mayor expresividad según Cairo. Cada serie activa la **Ley de Continuidad de Gestalt**: el ojo sigue cada línea como una entidad separada sin esfuerzo. Se descartaron barras agrupadas porque con más de 3 series la comparación de alturas se vuelve cognitivamente costosa.

**Interpretación:** Revela qué categorías crecen, cuáles se estancan y si hay estacionalidad. Responde directamente la pregunta central del dashboard: ¿qué líneas de negocio impulsan los ingresos y en qué momentos?

p align"center"img src"assets/screenshots/033%20Evolución%20de%20Ingresos%20por%20Categoría.png" alt""/p

---

#### 2. Ranking de Productos por Ingresos (Barras Horizontales)

**Variables usadas:** Eje Y: `Producto` (orden descendente por ingresos) · Eje X: `Ingresos_Totales` (eje desde cero)

**Justificación visual y psicológica:** Las barras horizontales son superiores a las verticales cuando las etiquetas son nombres largos: se leen de izquierda a derecha sin rotación. Iniciar el eje en cero es un imperativo de diseño honesto (Cairo: no distorsionar magnitudes relativas). El azul uniforme aplica la **Ley de Similitud** —todas las barras son la misma categoría visual— y deja que el orden descendente, no el color, guíe la lectura del mayor al menor.

**Interpretación:** Evidencia el grado de concentración de ingresos. En este dataset las barras decrecen de forma gradual (de 9.5 a 5 mill.) sin un Pareto pronunciado: los ingresos están repartidos entre todo el catálogo, no concentrados en pocos productos estrella.

p align"center"img src"assets/screenshots/034%20Ranking%20de%20Productos%20por%20Ingresos.png" alt""/p

---

#### 3. Mapa de Calor: Patrón de Ventas por Día y Mes (Matriz con Formato Condicional)

**Variables usadas:** Filas: `NombreMes` · Columnas: `DiaSemana` · Valores: `Ingresos_Totales` con escala blanco  azul oscuro

**Implementación:** El día de la semana no existe como columna en el dataset original. Se crearon dos columnas calculadas en la tabla `Calendario` (no medidas, ya que describen una propiedad de cada fecha): `DiaSemana  FORMAT(Calendario[Date], "dddd")` y `NumeroDia  WEEKDAY(Calendario[Date], 2)`. La segunda se usó para ordenar la primera en secuencia LunesDomingo y evitar el orden alfabético.

p align"center"img src"assets/screenshots/035%20Columnas%20calculadas%20DiaSemana%20NumeroDia.png" alt""/p

**Justificación visual y psicológica:** El mapa de calor revela distribuciones bidimensionales que ningún gráfico de líneas o barras puede mostrar al mismo tiempo. Usa luminosidad para codificar magnitud, activando la **Ley de Región Común**: celdas de color similar se perciben como parte del mismo patrón. Es especialmente efectivo para detectar estacionalidad intra-semanal e inter-mensual de forma simultánea.

**Interpretación:** Muestra en qué días y meses se concentran las ventas. Esa información es directamente accionable para decisiones de logística, staffing y campañas promocionales focalizadas.

p align"center"img src"assets/screenshots/036%20Mapa%20de%20Calor%20Patrón%20de%20Ventas%20por%20Día%20y%20Mes.png" alt""/p

---

#### 4. KPIs Ejecutivos  Composición de Métodos de Pago (Cards  Anillo)

**Variables usadas:** Cards: `Ingresos_Totales`, `Variacion_vs_PeriodoAnterior` (%), `Ticket_Promedio` · Anillo: `Metodo_Pago` con `Ingresos_Totales`

**Justificación visual y psicológica:** Las tarjetas KPI responden al principio de jerarquía visual: cifras en 32px Sans-Serif permiten responder "¿cómo vamos?" en menos de 3 segundos. El anillo se justifica porque `Metodo_Pago` tiene solo 2 categorías (`Efectivo`, `Tarjeta`); con más, las diferencias angulares serían imperceptibles y se necesitarían barras al 100%. La **Ley de Figura-Fondo** dirige la atención a los segmentos a través del espacio central vacío. La variación porcentual usa formato condicional (verde/rojo) como codificación semántica pre-atentiva.

**Interpretación:** Ofrece lectura instantánea del período. El anillo revela un reparto equilibrado: `efectivo` 53.77% ($130 mill.) frente a `tarjeta` 46.23% ($111 mill.). Sin dependencia crítica de un solo método de pago, el riesgo operativo es bajo.

p align"center"img src"assets/screenshots/037%20Composición%20de%20Métodos%20de%20Pago.png" alt""/p

---

#### 5. Evolución de la Participación por Ciudad (Barras Apiladas al 100%)

**Variables usadas:** Eje X: `Mes`-`Año` · Eje Y: porcentaje de participación (0–100%) · Leyenda: `Ciudad`

**Justificación visual y psicológica:** Las barras apiladas al 100% son la codificación correcta cuando el objetivo es composición proporcional en el tiempo, no magnitud absoluta. Cada barra suma 100%, permitiendo rastrear si una ciudad gana o pierde participación de mercado. La **Ley de Continuidad** se activa en los límites entre segmentos: el ojo rastrea la frontera como si fuera una línea. Se usó paleta categórica cualitativa con máximo 6 colores para que la **Ley de Similitud** funcione sin ambigüedad.

**Interpretación:** Detecta si la concentración geográfica de ventas está cambiando. Una ciudad emergente que gana participación mientras la principal la pierde puede indicar saturación de mercado o éxito de expansión regional.

p align"center"img src"assets/screenshots/038%20Evolución%20de%20la%20Participación%20por%20Ciudad.png" alt""/p

---

### 3.3 Interactividad y Usabilidad

#### ️ F — Slicers, Interacciones y Diseño del Lienzo

**Slicers estratégicos:** Dos segmentadores. El primero de **Período** (`Año`  `Trimestre`, tabla Calendario), estilo *Desplegable* para minimizar espacio. El segundo de **Categoría de Producto**, estilo *Lista* con selección múltiple. Ambos en la zona superior siguiendo el patrón de lectura F/Z: el usuario escanea primero esa área, así los controles se descubren antes de llegar a los gráficos.

**Interacciones visuales:** Todos los filtros cruzados configurados en modo **Filtrar** (no Resaltar) vía `Formato  Editar interacciones`. El modo Resaltar atenúa datos no seleccionados generando ruido visual; el modo Filtrar elimina lo irrelevante y enfoca al usuario en el subconjunto analizado.

**Tipografía:** Exclusivamente **Segoe UI** en todo el dashboard. Títulos de gráficos: 14px Bold · Etiquetas de ejes: 13px Regular · Valores de KPI Cards: 28–32px Light. El mínimo de 13px garantiza legibilidad sin acercamiento, cumpliendo WCAG 2.1.

**Organización del lienzo (Gestalt):**
- **Ley de Proximidad:** KPI Cards y anillo junto al gráfico de líneas en la fila superior (respuesta ejecutiva rápida). Detalle analítico en filas inferiores.
- **Ley de Cercado:** cada visual con fondo blanco y borde redondeado `#E0E0E0` sobre lienzo `#F5F5F5`. Crea grupos sin líneas duras.
- **Ley de Similitud:** paleta consistente en todos los gráficos. El mismo color siempre representa la misma categoría.

```

  [SLICER: Año/Trimestre]    [SLICER: Categoría]      

  KPI CARDS        Gráfico 1: Líneas temporales      
   Anillo                                           

  Gráfico 2: Ranking Productos    Gráfico 3: Mapa    
                                  de Calor           

         Gráfico 5: Barras 100% Ciudades              

```

---

## 4. Análisis e Interpretación de Resultados

**Hallazgo 1 — Evolución temporal sin crecimiento sostenido ni colapso**

El gráfico de líneas muestra ingresos que oscilan mes a mes sin una tendencia clara de crecimiento o caída a lo largo del período 2022-2023. Las 30 categorías se comportan de forma paralela: cuando sube una, las demás tienden a subir también, lo que sugiere que las fluctuaciones responden a factores externos (estacionalidad, temporadas) más que a dinámicas internas entre categorías. No hay una categoría que claramente lidere ni una que esté en declive estructural. El negocio opera de forma uniforme.

**Hallazgo 2 — Distribución equilibrada sin concentración de riesgo**

El cruce entre las visualizaciones revela un patrón consistente: el negocio no depende de ningún único factor. El ranking de productos muestra ingresos que decaen gradualmente de \$9.5 mill. (Door) a \$5 mill. (Worry) — sin un producto estrella que concentre el 50% de los ingresos. Los métodos de pago se dividen casi en partes iguales (efectivo 53.77% / tarjeta 46.23%). El mapa de calor expone que la actividad no se concentra en un solo día de la semana ni en un mes puntual. Ningún punto único de falla domina la estructura de ingresos.

**Hallazgo 3 — Métricas base confiables tras el proceso ETL**

Las 100 transformaciones aplicadas en OpenRefine produjeron un dataset íntegro: 18,000 filas sin valores nulos críticos, columnas con tipos correctos y categorías unificadas. Sobre ese modelo, las tres medidas DAX calcularon con precisión: `Ingresos_Totales` \$241.09 mill., `Ticket_Promedio` \$8.04 mill. por transacción y `Variacion_vs_PeriodoAnterior` operativa por mes. Ninguna cifra del dashboard depende de datos sucios o de relaciones mal configuradas — los cinco ajustes de modelado documentados garantizan esa confiabilidad.

**Conclusión general**

El análisis temporal del dataset de ventas muestra un negocio con actividad comercial estable a lo largo de aproximadamente 13 meses. No hay señales de crecimiento acelerado ni de crisis, sino fluctuaciones propias de la demanda normal. La fortaleza estructural está en su diversificación: 30 productos, 30 ciudades, 30 categorías y dos métodos de pago con participación similar. Esa distribución reduce el riesgo operativo y hace que el negocio sea menos vulnerable a la caída de cualquier variable individual. Para el siguiente ciclo de análisis, el foco debería estar en identificar los meses y días de la semana con mayor concentración de ventas — dato que el mapa de calor ya comienza a revelar — para orientar campañas y decisiones de inventario con base en evidencia.
