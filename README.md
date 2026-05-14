# 📊 Análisis y Visualización de Ventas Temporales

![OpenRefine](https://img.shields.io/badge/OpenRefine-3.7+-orange?style=flat-square)
![Power BI](https://img.shields.io/badge/Power%20BI-Desktop-F2C811?style=flat-square&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-Measures-0078D4?style=flat-square)
![Dataset](https://img.shields.io/badge/Dataset-18%2C000%20rows-brightgreen?style=flat-square)
![ETL](https://img.shields.io/badge/ETL-100%20transforms-blue?style=flat-square)
![Status](https://img.shields.io/badge/Status-Completed-success?style=flat-square)

> Pipeline ETL completo sobre 18,000 registros de ventas: limpieza y normalización en OpenRefine (100 transformaciones) + dashboard interactivo en Power BI con 5 visualizaciones, tabla Calendario y 3 medidas DAX.

---

## 📑 Tabla de Contenidos

- [📊 Análisis y Visualización de Ventas Temporales](#-análisis-y-visualización-de-ventas-temporales)
  - [📑 Tabla de Contenidos](#-tabla-de-contenidos)
  - [📋 Descripción del proyecto](#-descripción-del-proyecto)
  - [🖼️ Demo](#️-demo)
  - [📂 Dataset](#-dataset)
    - [Archivo fuente](#archivo-fuente)
    - [Esquema (15 columnas)](#esquema-15-columnas)
    - [Problemas de calidad detectados](#problemas-de-calidad-detectados)
  - [🗂️ Estructura del repositorio](#️-estructura-del-repositorio)
  - [🛠️ Stack tecnológico](#️-stack-tecnológico)
  - [⚙️ Pipeline ETL — OpenRefine](#️-pipeline-etl--openrefine)
    - [Fase 1 — Exploración y Diagnóstico](#fase-1--exploración-y-diagnóstico)
    - [Fase 2 — Limpieza Básica y Normalización](#fase-2--limpieza-básica-y-normalización)
    - [Fase 3 — Clustering](#fase-3--clustering)
    - [Fase 4 — Transformaciones Avanzadas de Tipos y Nulos](#fase-4--transformaciones-avanzadas-de-tipos-y-nulos)
    - [Fase 5 — Exportación](#fase-5--exportación)
  - [📈 Dashboard Power BI](#-dashboard-power-bi)
    - [Tabla Calendario](#tabla-calendario)
  - [🧮 Medidas DAX](#-medidas-dax)
    - [Visualizaciones del dashboard](#visualizaciones-del-dashboard)
  - [🔧 Ajustes de modelado](#-ajustes-de-modelado)
  - [🔍 Hallazgos principales](#-hallazgos-principales)
  - [▶️ Cómo reproducir](#️-cómo-reproducir)
  - [👤 Autor](#-autor)

---

## 📋 Descripción del proyecto

Este proyecto implementa un pipeline ETL completo sobre un dataset de ventas comerciales, aplicando:

- **Limpieza de datos** en OpenRefine con expresiones GREL y algoritmos de clustering para unificar variantes sucias en columnas categóricas.
- **Modelado dimensional** en Power BI con una tabla Calendario explícita y medidas DAX para análisis temporal.
- **Dashboard interactivo** con 5 tipos de visualización distintos, slicers de período y categoría, y principios de Gestalt aplicados al diseño del lienzo.

El objetivo del análisis es estudiar la **evolución temporal de ingresos** — identificar estacionalidad, distribución por categoría, ciudad y método de pago, y calcular variaciones mes a mes.

---

## 🖼️ Demo

<p align="center">
  <img src="assets/dashboard/Visualización de evolución en el tiempo_001.png" alt="Dashboard Power BI" width="800">
</p>

| KPI | Valor |
|-----|-------|
| Ingresos Totales | **$241.09 mill.** |
| Ticket Promedio | **$8.04 mill.** |
| Métodos de pago | Efectivo 53.77% / Tarjeta 46.23% |
| Período cubierto | Dic 2022 — Dic 2023 |
| Transacciones | 18,000 |

---

## 📂 Dataset

### Archivo fuente
`assets/dataset/raw/datos_ventas.csv` — exportado de un sistema de origen con múltiples errores de calidad.

### Esquema (15 columnas)

| Columna | Tipo original | Tipo limpio | Notas |
|---------|--------------|-------------|-------|
| `ID_Venta` | Texto | Entero (clave) | Marcado como clave en Power Query |
| `Producto` | Texto | Texto (TitleCase) | Contenía `wait*`, `*democratic`, `door_` |
| `Ciudad` | Texto | Texto (TitleCase) | Contenía `Jodichester?`, `East Brittany?` |
| `Categoria` | Texto | Texto (TitleCase) | 227 variantes → 30 valores canónicos |
| `Precio_Unitario` | Texto | Número | Espacios embebidos eliminados |
| `Cantidad` | Texto | Número | Espacios embebidos eliminados |
| `Fecha_Venta` | Texto | **Date** (no DateTime) | ISO `2023-11-20T00:00:00Z` → `date` |
| `Cliente` | Texto | Texto (TitleCase) | Normalización de capitalización |
| `Email` | Texto | Texto (lowercase) | 29 clusters con `@` reemplazado por espacio |
| `Telefono` | Texto | Texto | Limpieza de caracteres especiales |
| `Direccion` | Texto | Texto | Espacios internos normalizados |
| `Metodo_Pago` | Texto | Texto (lowercase) | `_Efectivo`, `Efectivo*` → `efectivo` |
| `Estado` | Texto | Texto | `En?cami`, `En cam*` → `En camino` |
| `Comentario` | Texto / Nulo | Texto | Nulos → `"Sin comentario"` |
| `Descuento` | Número | Número | Almacenado como entero (21 = 21%) |

### Problemas de calidad detectados

```
- Caracteres especiales:  *, _, ? al inicio/final de valores categóricos
- Capitalización mixta:   "east jonathan", "EAST JONATHAN", "East jonathan"
- Espacios invisibles:    " Efectivo" ≠ "Efectivo" para Power BI
- Emails corruptos:       29 registros con @ reemplazado por espacio
- Fechas como texto:      formato ISO con componente horario (DateTime ≠ Date)
- Nulos en Comentario:    celdas vacías sin valor predeterminado
- Descuento como entero:  21 almacenado en lugar de 0.21 — crítico para DAX
```

---

## 🗂️ Estructura del repositorio

```
data_refine/
│
├── README.md
├── Arbelaez_Daniel_Actividad_Visualizacion.md   # Informe técnico completo
├── Arbelaez_Daniel_Actividad_Visualizacion.pdf  # Versión PDF del informe
│
├── assets/
│   ├── dashboard/
│   │   ├── Visualización de evolución en el tiempo.pbix   # Archivo Power BI
│   │   └── Visualización de evolución en el tiempo_001.png
│   │
│   ├── dataset/
│   │   ├── raw/
│   │   │   └── datos_ventas.csv           # Dataset original sin limpiar
│   │   └── clean/
│   │       ├── datos-ventas-csv.csv       # Dataset limpio (exportado de OpenRefine)
│   │       └── datos-ventas-csv.xlsx      # Mismo dataset en formato Excel
│   │
│   └── screenshots/                       # 42 capturas del proceso ETL y dashboard
│       ├── 001–020  OpenRefine ETL
│       ├── 021–039  Power BI modelado
│       └── 040      Dashboard final
│
└── docs/
    ├── actividad/
    │   └── Actividad I.pdf                # Enunciado de la actividad académica
    └── prompt/
        ├── 00_initial_prompt.md
        ├── 01_fase1.md
        └── 02_fase2.md
```

---

## 🛠️ Stack tecnológico

| Herramienta | Versión | Uso |
|-------------|---------|-----|
| **OpenRefine** | 3.7+ | Limpieza, normalización y clustering de datos |
| **GREL** | — | General Refine Expression Language para transformaciones |
| **Power BI Desktop** | 2024+ | Modelado, DAX y visualización |
| **DAX** | — | Data Analysis Expressions para medidas calculadas |
| **Power Query (M)** | — | Transformación de tipos en importación |
| **CSV / UTF-8** | — | Formato de intercambio entre herramientas |

---

## ⚙️ Pipeline ETL — OpenRefine

El proceso completo registró **100 transformaciones** (Undo/Redo 100/100).

### Fase 1 — Exploración y Diagnóstico

| Técnica | Columnas | Objetivo |
|---------|----------|----------|
| `Text Facet` | Categoria, Ciudad, Metodo_Pago, Estado | Inventario de variantes sucias |
| `Numeric Facet` | Precio_Unitario, Cantidad | Detección de valores non-numeric |
| `Facet by blank` | Todas las críticas | Cuantificación de nulos |

> **Hallazgo crítico de fase 1:** `Categoria` tenía 227 opciones únicas para ~30 valores esperados.

---

### Fase 2 — Limpieza Básica y Normalización

<details>
<summary><strong>Ver todas las transformaciones GREL</strong></summary>

**Paso 2.1 — Trim espacios al inicio/final**
```
Ruta: Edit cells → Common transforms → Trim leading and trailing whitespace
Columnas: Producto, Ciudad, Categoria, Cliente, Metodo_Pago, Estado
```

**Paso 2.2 — Colapsar espacios internos múltiples**
```grel
value.replace(/\s+/, " ")
Columnas: Ciudad, Cliente, Direccion
```

**Paso 2.3 — Normalizar a TitleCase**
```grel
value.toTitlecase()
Columnas: Producto, Ciudad, Cliente, Categoria
Ejemplo: "owner" → "Owner", "education" → "Education"
```

**Paso 2.4 — Normalizar a lowercase**
```grel
value.toLowercase()
Columnas: Email, Metodo_Pago
Ejemplo: "Efectivo" → "efectivo", "Pedro@Example.com" → "pedro@example.com"
```

**Paso 2.5 — Eliminar caracteres especiales `*`, `_`, `?`**
```grel
value.replace(/^[*_?]+|[*_?]+$/, "").trim()
Columnas: Producto, Ciudad, Categoria, Metodo_Pago, Estado, Cliente, Email, Fecha_Venta
Ejemplo: "*Tarjeta" → "Tarjeta", "_Efectivo" → "efectivo", "budget?" → "budget"
```

</details>

---

### Fase 3 — Clustering

| Columna | Método | Función | Clusters encontrados |
|---------|--------|---------|---------------------|
| `Categoria` | Key Collision | Fingerprint | 28 grupos → 227 variantes a 30 valores |
| `Email` | Key Collision | n-Gram (tamaño 1) | 29 clusters (@ reemplazado por espacio) |

---

### Fase 4 — Transformaciones Avanzadas de Tipos y Nulos

**Conversión de `Fecha_Venta` a tipo Fecha:**
```
Ruta: Edit cells → Common transforms → To date
Resultado: "2023-11-20T00:00:00Z" (texto) → 2023-11-20 (fecha verde)
```

**Imputación de nulos en `Comentario`:**
```grel
if(isBlank(value), "Sin comentario", value)
```

---

### Fase 5 — Exportación

```
1. Panel Facet/Filter → Remove All          (verificar: 18,000 rows)
2. Export → Comma-separated value (CSV)
3. Formato: UTF-8
```

---

## 📈 Dashboard Power BI

### Tabla Calendario

Requerida para que las funciones de inteligencia temporal (`PREVIOUSMONTH`, `TOTALYTD`) funcionen correctamente.

```dax
Calendario = 
CALENDAR(
    MIN(Ventas[Fecha_Venta]),
    MAX(Ventas[Fecha_Venta])
)
```

Columnas calculadas:
```dax
Año       = YEAR(Calendario[Date])
Mes       = MONTH(Calendario[Date])
NombreMes = FORMAT(Calendario[Date], "MMMM")
Trimestre = "Q" & QUARTER(Calendario[Date])
DiaSemana = FORMAT(Calendario[Date], "dddd")
NumeroDia = WEEKDAY(Calendario[Date], 2)   -- Lunes=1, Domingo=7
```

> ⚠️ `NombreMes` debe ordenarse por `Mes` (`Herramientas de columna → Ordenar por columna`) para que los ejes usen orden cronológico, no alfabético.

**Relación:** `Calendario[Date]` → `Ventas[Fecha_Venta]` (muchos a uno, dirección única)

---

## 🧮 Medidas DAX

```dax
-- Medida 1: Ingresos reales aplicando descuento por fila
-- Nota: Descuento almacenado como entero (21 = 21%), requiere /100
Ingresos_Totales = 
SUMX(
    Ventas,
    Ventas[Precio_Unitario] * Ventas[Cantidad] * (1 - Ventas[Descuento] / 100)
)
```

```dax
-- Medida 2: Variación porcentual respecto al mes anterior
-- Requiere tabla Calendario marcada como Date Table y slicer de período activo
Variacion_vs_PeriodoAnterior = 
VAR VentasActual   = [Ingresos_Totales]
VAR VentasAnterior = CALCULATE(
    [Ingresos_Totales],
    PREVIOUSMONTH(Calendario[Date])
)
RETURN
DIVIDE(VentasActual - VentasAnterior, VentasAnterior, 0)
```

```dax
-- Medida 3: Ingreso promedio por transacción única
Ticket_Promedio = 
DIVIDE(
    [Ingresos_Totales],
    DISTINCTCOUNT(Ventas[ID_Venta]),
    0
)
```

### Visualizaciones del dashboard

| # | Tipo | Variables | Gestalt aplicada |
|---|------|-----------|-----------------|
| 1 | Líneas multi-serie | Eje X: NombreMes/Año · Eje Y: Ingresos_Totales · Leyenda: Categoria | Ley de Continuidad |
| 2 | Barras horizontales | Eje Y: Producto · Eje X: Ingresos_Totales | Ley de Similitud |
| 3 | Mapa de calor (Matriz) | Filas: NombreMes · Columnas: DiaSemana · Valores: Ingresos_Totales | Ley de Región Común |
| 4 | Cards + Anillo | Cards: 3 medidas · Anillo: Metodo_Pago | Ley de Figura-Fondo |
| 5 | Barras apiladas 100% | Eje X: Mes-Año · Leyenda: Ciudad | Ley de Continuidad |

**Slicers:** Período (Año/Trimestre, estilo Desplegable) + Categoría (Lista, multi-selección)  
**Interacciones:** Modo Filtrar (no Resaltar) en todos los cruces  
**Tipografía:** Segoe UI — 14px Bold títulos / 13px Regular ejes / 28-32px Light KPIs

---

## 🔧 Ajustes de modelado

Problemas encontrados y corregidos durante la construcción del modelo:

| # | Problema | Síntoma | Fix aplicado |
|---|---------|---------|-------------|
| 1 | Calendario sin marcar como Date Table | `PREVIOUSMONTH` devuelve BLANK | `Clic derecho Calendario → Marcar como tabla de fechas → Date` |
| 2 | Medidas creadas en tabla Calendario | Desorden en panel de campos | Movidas a carpeta `Medidas` dentro de tabla `Ventas` |
| 3 | `Descuento` almacenado como entero | `Ingresos_Totales` = -$19.56 mil M (negativo) | `(1 - Ventas[Descuento] / 100)` en lugar de `(1 - Ventas[Descuento])` |
| 4 | `Variacion` mostraba `0.85` | Formato decimal sin contexto visual | `Herramientas de medición → Formato → Porcentaje` |
| 5 | `Fecha_Venta` importada como DateTime | `NombreMes` en blanco, relación sin match | Power Query: `Tipo de datos → Fecha` (type date, no type datetime) |

> **El ajuste #5 es el más crítico y menos obvio:** la relación entre `Calendario[Date]` (tipo `Date`) y `Ventas[Fecha_Venta]` (tipo `DateTime`) existía visualmente en el modelo pero no transfería filtros — `Date ≠ DateTime` en el motor de Power BI.

---

## 🔍 Hallazgos principales

**1. Evolución sin tendencia sostenida**  
Los ingresos oscilan mes a mes durante 2022-2023 sin crecimiento ni caída estructural. Las 30 categorías se mueven en paralelo — las fluctuaciones responden a estacionalidad general, no a dinámicas internas entre líneas de negocio.

**2. Distribución diversificada sin concentración de riesgo**  
El ranking de productos muestra decrecimiento gradual (Door ~$9.5M → Worry ~$5M) sin un Pareto pronunciado. Los métodos de pago tienen una división casi equilibrada: efectivo 53.77% ($130M) / tarjeta 46.23% ($111M). Ningún factor único concentra la dependencia del negocio.

**3. Métricas base post-ETL**  
| Métrica | Valor |
|---------|-------|
| Ingresos Totales | $241.09 mill. |
| Ticket Promedio | $8.04 mill. |
| Variación mes anterior | 84.78% (con filtro de período activo) |
| Transacciones | 18,000 |
| Transformaciones ETL | 100 operaciones |

---

## ▶️ Cómo reproducir

El paso a paso completo está documentado en la guía maestra del proyecto:

📄 **[`Arbelaez_Daniel_Actividad_Visualizacion.md`](./Arbelaez_Daniel_Actividad_Visualizacion.md)**

La guía cubre en detalle:

- **Portada y metadatos** del proyecto
- **Sección 1** — Descripción del dataset: origen, dimensiones y columnas
- **Sección 2 — Fase 1 (OpenRefine):** las 5 fases del pipeline ETL con comandos GREL exactos, capturas de cada transformación y justificación técnica de cada decisión
- **Sección 3 — Fase 2 (Power BI):** importación en Power Query, creación de la tabla Calendario, las 3 medidas DAX, los 5 ajustes de modelado encontrados y corregidos, y la descripción de cada visualización con principios de Gestalt aplicados
- **Sección 4** — Análisis e interpretación de resultados con hallazgos concretos basados en los datos

Cada paso incluye la ruta de menú o expresión GREL/DAX, la justificación lógica de por qué se aplicó, y la captura de pantalla que evidencia el resultado.

---

## 👤 Autor

**Daniel Arbeláez Álvarez**  
Módulo: Análisis y Visualización de Datos

---

<p align="center">
  <img src="https://img.shields.io/badge/OpenRefine-ETL-orange?style=for-the-badge" alt="OpenRefine">
  <img src="https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?style=for-the-badge&logo=powerbi&logoColor=black" alt="Power BI">
  <img src="https://img.shields.io/badge/DAX-Measures-0078D4?style=for-the-badge" alt="DAX">
</p>
