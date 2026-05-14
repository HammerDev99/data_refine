# Initial prompt para manipulación de datos en OpenRefine con Claude Chrome Extensión

Tengo una actividad que resolver (@Actividad I.docx ) de aquí se desprende la necesidad de crear un prompt especializado para Claude Chrome extensión para que haga la manipulación del open refine y puedo evidenciar todo lo necesario para cumplir con lo que se requiere de la actividad antes mencionada. De lo anterior, también te comparto contexto adicional de un taller con actividades de refinamiento de datos (@taller openrefine.pdf  ) y que adicionalmente para tu consulta en la medida de lo necesario y conformación de un prompt bien completo te comparto en primera instancia @Primeros_pasos_con_Open_Ref...   y seguidamente @Guía de uso básico de OpenR...  .


Quiero que este trabajo detallado de directrices para ese modelo de claude para manipular la web quede bien completo con las instrucciones adecuadas para refinar la data. Mas adelante en otra sesión se hará la gestión para proceder con la actividad de visualización


Hazme las preguntas que consideres necesarias para proceder a mejorar tu entendimiento y hacer un proceso de razonamiento extendido y profundo para mejorar la respuesta

***

Este prompt ha sido diseñado meticulosamente basándose en la rúbrica de tu actividad y en las guías técnicas de OpenRefine que proporcionaste. Su objetivo principal es asegurar que Claude no solo limpie los datos, sino que **documente y justifique cada paso**, garantizando que obtengas la máxima puntuación en los criterios de "Transformación y Limpieza" (9%) y "Documentación y Justificación" (7%).

Por favor, copia el siguiente texto y ajústalo en las partes indicadas con corchetes `[ ]` antes de enviárselo a Claude:

***

### 📋 Copia el siguiente Prompt Maestro para Claude:

> **Rol:** Eres un Analista de Datos Experto y especialista en la herramienta OpenRefine. Tu objetivo es guiarme y ejecutar comandos paso a paso para la limpieza, transformación y enriquecimiento de un conjunto de datos, preparándolo para su visualización en Power BI.
>
> **Contexto de la Tarea:** 
> Estoy realizando un laboratorio académico cuya rúbrica exige realizar una limpieza exhaustiva (separación de columnas, corrección de errores, normalización de texto y manejo de nulos). El criterio más importante es que **por cada transformación que realices o sugieras, debes redactar una documentación detallada con justificaciones claras y lógicas** para que yo pueda incluirla en mi informe final.
>
> **Sobre los datos:**
> El dataset trata sobre: `[Describe aquí brevemente de qué trata tu dataset, ej: Ventas de una empresa, registros de estudiantes, etc.]`.
> Las columnas principales son: `[Lista aquí 3 o 4 columnas clave]`.
> 
> **Instrucciones de Ejecución:**
> Vamos a trabajar por fases. No avances a la siguiente fase hasta que yo te confirme que el paso actual se ejecutó correctamente en mi entorno local de OpenRefine.
> 
> **Fase 1: Exploración y Diagnóstico (Faceting y Filtrado)**
> *   Sugiere la creación de "Text facets" y "Numeric facets" para identificar valores nulos, espacios en blanco, duplicados o inconsistencias en las columnas principales.
> 
> **Fase 2: Limpieza Básica y Normalización**
> *   Proporciona las instrucciones o comandos GREL para eliminar espacios extra al inicio y al final usando `value.trim()` y colapsar espacios internos repetidos con `value.replace(/\s+/, " ")`.
> *   Indica cómo normalizar el texto (mayúsculas/minúsculas) usando funciones como `toLowercase()` o `toTitlecase()` según corresponda al contexto de la columna.
> 
> **Fase 3: Corrección de Errores y Agrupación (Clustering)**
> *   Guíame para usar la herramienta de agrupamiento ("Cluster and Edit"). Sugiere probar métodos como "Key collision" y "Nearest neighbor" para unificar valores mal escritos (ej. unificar diferentes formas de escribir una misma ciudad o categoría).
> 
> **Fase 4: Transformaciones Avanzadas y Separación (GREL)**
> *   Si hay columnas que contienen múltiples datos combinados, proporciona los pasos para usar la función "Split into several columns...".
> *   Usa expresiones GREL avanzadas como `value.replace("x", "y")` para limpiar caracteres extraños o símbolos de moneda en columnas numéricas.
> *   Guíame para convertir los tipos de datos a su formato correcto (Ej: de Texto a Número, o usar `value.toDate()` para fechas).
>
> **Fase 5: Exportación para Power BI**
> *   Indícame cómo asegurar que los datos no tengan filtros activos (Remove All) y el paso a paso para exportar el resultado final en formato CSV o Excel (.xls) sin alterar la codificación.
> 
> **🛑 REGLA DE ORO - DOCUMENTACIÓN OBLIGATORIA:**
> Después de indicarme cada instrucción o código GREL, **DEBES generar automáticamente un bloque de texto listo para copiar y pegar en mi informe PDF**. Este bloque debe tener el siguiente formato exacto:
> 
> **Paso realizado:** [Nombre técnico de la acción, ej: Normalización de texto y eliminación de espacios en la columna 'Ciudad'].
> **Ruta o Comando GREL:** [Ej: Edit cells > Common transforms > Trim leading and trailing whitespace].
> **Justificación lógica:** [Explica de forma técnica por qué se hizo esto y cómo mejora la calidad del dato para el análisis posterior en Power BI. Ej: "Se identificaron espacios en blanco al inicio de las celdas, lo que PowerBI interpretaría como categorías distintas. Se aplicó la función trim() para asegurar la estandarización de la variable categórica y evitar duplicidad en las gráficas"].
> 
> ¿Entendido? Si es así, dime "¡Entendido! Por favor, comparte una muestra de tus datos o indícame con qué columna deseas comenzar nuestra Fase 1."

***

### ¿Por qué este prompt está diseñado así?

1.  **Alineación con la Rúbrica:** Garantiza el 25% de la nota de OpenRefine, exigiendo específicamente el uso exhaustivo de limpieza (normalización y separación) y forzando al modelo a crear la justificación lógica que exige tu rúbrica (Nivel Excelente: *"Todas las operaciones se justifican adecuadamente"* y *"Se proporciona una documentación detallada de cada paso"*).
2.  **Uso de GREL y Mejores Prácticas:** Incorpora funciones extraídas directamente de tus PDFs de estudio, como `value.trim()`, agrupamiento por *Key collision* o *Nearest neighbor*, y *Split into several columns*.
3.  **Preparación para Power BI:** El prompt le recuerda a Claude que el fin último es que Power BI consuma estos datos (el cual vale el 60% de tu nota final), asegurando que los tipos de datos (números y fechas) queden correctamente casteados desde OpenRefine.

Una vez termines con este proceso en OpenRefine (recuerda ir tomando capturas de pantalla de los "Text Facets" y de los cambios masivos para incluirlos en tu Word/PDF), podremos preparar el siguiente paso enfocado en la visualización temporal de datos en Power BI. 

¿Hay algún ajuste particular sobre el dataset que quieras que incluyamos en este prompt antes de que lo uses?