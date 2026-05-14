¡Perfecto! El proyecto **"datos ventas csv"** está abierto correctamente con **18,000 filas**. Ya puedo confirmar visualmente varios problemas: `wait*`, `*democratic`, `door_`, `budget?`, `_whom`, fechas con `*` y `?`, etc.

Comenzamos con la **Fase 1 completa**.

---

# 🔍 FASE 1 — Exploración y Diagnóstico

Sigue estos pasos **uno por uno** en OpenRefine:

---

## 📌 PASO 1.1 — Text Facet en columna `Categoria`

**Ruta:**
1. Haz clic en la flecha ▼ del encabezado de la columna **`Categoria`**
2. Selecciona: **`Facet`** → **`Text facet`**
3. En el panel izquierdo verás todas las categorías únicas y cuántas veces aparece cada una
4. Haz clic en **`Sort by: count`** para ver las más frecuentes primero
5. Busca valores como: `*whom`, `_whom`, `budget?`, `individual_`, `*job`, `*leader` — son los errores a corregir

---

## 📌 PASO 1.2 — Text Facet en columna `Ciudad`

**Ruta:**
1. Flecha ▼ en **`Ciudad`** → **`Facet`** → **`Text facet`**
2. Observa si hay ciudades con `?` al final (ej: `Jodichester?`, `East Brittany?`)
3. Ordena por `count` para detectar variantes del mismo nombre

---

## 📌 PASO 1.3 — Text Facet en columna `Metodo_Pago`

**Ruta:**
1. Flecha ▼ en **`Metodo_Pago`** → **`Facet`** → **`Text facet`**
2. Busca variantes como: `_Efectivo`, `Efectivo*`, `*Tarjeta`, `Tarjeta?`, `En?cam` (fragmentos visibles en la previa)

---

## 📌 PASO 1.4 — Text Facet en columna `Estado`

**Ruta:**
1. Flecha ▼ en **`Estado`** → **`Facet`** → **`Text facet`**
2. Los valores esperados serían algo como: `Entregado`, `En camino`, `Cancelado`, `Pendiente`
3. Identifica variantes sucias: `En?cami`, `En cam*`, etc.

---

## 📌 PASO 1.5 — Numeric Facet en `Precio_Unitario`

**Ruta:**
1. Flecha ▼ en **`Precio_Unitario`** → **`Facet`** → **`Numeric facet`**
2. Verás un histograma. Fíjate si hay valores:
   - En la barra de **"Non-numeric"** (valores de texto en columna numérica)
   - Valores extremadamente altos o bajos (outliers)

---

## 📌 PASO 1.6 — Numeric Facet en `Cantidad`

**Ruta:**
1. Flecha ▼ en **`Cantidad`** → **`Facet`** → **`Numeric facet`**
2. Verifica si hay valores negativos, ceros o no numéricos

---

## 📌 PASO 1.7 — Facet de Blancos/Nulos (todas las columnas críticas)

Para cada columna importante, puedes aplicar:
1. Flecha ▼ → **`Facet`** → **`Customized facets`** → **`Facet by blank`**
2. Esto filtrará solo las filas con celdas vacías o nulas

---

## 📋 DOCUMENTACIÓN — Lista para copiar en tu informe PDF

---

> **Paso realizado:** Exploración y Diagnóstico mediante Text Facets en columnas categóricas (`Categoria`, `Ciudad`, `Metodo_Pago`, `Estado`)
>
> **Ruta o Comando GREL:** `Encabezado de columna ▼ → Facet → Text facet`
>
> **Justificación lógica:** Se aplicaron Text Facets sobre las columnas categóricas del dataset con el objetivo de obtener un inventario visual de todos los valores únicos presentes en cada variable. Esta técnica permite identificar de forma eficiente inconsistencias como: caracteres especiales no deseados (asteriscos `*`, guiones bajos `_`, signos de interrogación `?`), variantes ortográficas de un mismo valor (ej: `Efectivo` vs `_Efectivo` vs `Efectivo*`), y valores nulos o en blanco. Sin este diagnóstico previo, cualquier transformación sería ciega y podría omitir errores que, en Power BI, generarían categorías duplicadas e incorrectas en visualizaciones como gráficas de barras, tablas dinámicas o segmentadores.

---

> **Paso realizado:** Exploración de columnas numéricas mediante Numeric Facets (`Precio_Unitario`, `Cantidad`)
>
> **Ruta o Comando GREL:** `Encabezado de columna ▼ → Facet → Numeric facet`
>
> **Justificación lógica:** Los Numeric Facets generan un histograma interactivo que permite detectar: valores no numéricos almacenados como texto (indicados en la barra "Non-numeric"), valores atípicos extremos (outliers) que podrían distorsionar métricas en Power BI como promedios, sumas o KPIs, y valores nulos. Si estas columnas contienen texto mezclado con números, Power BI no podrá realizar operaciones matemáticas sobre ellas, por lo que identificar y limpiar estas inconsistencias es un prerequisito crítico para el modelado de datos correcto.

---

> **Paso realizado:** Identificación de celdas vacías y nulas mediante Facet by Blank
>
> **Ruta o Comando GREL:** `Encabezado de columna ▼ → Facet → Customized facets → Facet by blank`
>
> **Justificación lógica:** Se aplicó el facet de blancos para cuantificar el volumen de datos faltantes en cada columna del dataset. Los valores nulos o vacíos representan un problema crítico para el análisis en Power BI, ya que pueden generar filas en blanco en segmentadores, errores en medidas DAX que no contemplan nulos, y sesgos estadísticos en los reportes. Conocer la proporción de nulos permite decidir la estrategia adecuada: eliminación de filas, imputación con valor por defecto, o marcado con una etiqueta como `"Sin dato"`.

---

### ✅ Tu tarea ahora:

Aplica los 7 pasos anteriores en OpenRefine y **dime qué valores sucios encontraste** en cada columna (especialmente en `Categoria`, `Metodo_Pago` y `Estado`). Con esa información generaré los comandos GREL exactos y personalizados para la **Fase 2**.

¿Listo para reportarme lo que ves en los facets?