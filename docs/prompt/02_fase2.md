Comprendo perfectamente tu situación. Trabajar con Python (pandas, matplotlib, seaborn, plotly) te da un control programático fascinante, pero como analistas de datos, nuestra primera regla es **cumplir con los requerimientos del cliente o proyecto** (en este caso, la rúbrica que te exige Power BI con un peso del 60%). 

He analizado a fondo los tres nuevos documentos. El material es excelente y eleva el nivel de la visualización de un simple "hacer gráficas" a una verdadera **arquitectura cognitiva**. Para sacar el nivel "Excelente", tu dashboard no solo debe funcionar, sino aplicar las **Leyes de la Gestalt** (proximidad, similitud, cercado), usar la **tipografía correcta para pantallas** (Sans-Serif, >13px) y seguir el **"Algoritmo del Diseñador Visual"** de Alberto Cairo (usar líneas para tiempo, barras para ranking, evitar tartas con más de 3 variables).

Aquí tienes el **Prompt Maestro para la Fase 2**, diseñado para que Claude te guíe en Power BI y, al mismo tiempo, te redacte el texto exacto que debes pegar en tu archivo `Arbelaez_Daniel_Actividad_Visualizacion.md`.

***

### 📋 Copia el siguiente Prompt Maestro para Claude (Fase 2):

> **Rol:** Eres un Experto en Business Intelligence, especialista en Power BI y maestro en Psicología del Diseño Visual aplicado a datos (basado en los principios de Alberto Cairo y las Leyes de la Gestalt).
>
> **Contexto de la Tarea:** 
> He terminado la limpieza de mis datos en OpenRefine y ahora debo importarlos a Power BI para crear un dashboard interactivo enfocado en la **evolución en el tiempo**. Mi rúbrica exige: 
> 1. Crear 5 o más visualizaciones diferentes y complejas.
> 2. Alta interactividad y usabilidad (filtros, segmentadores).
> 3. Aplicar principios de percepción visual (Gestalt) y diseño de datos.
>
> **Instrucciones de Ejecución:**
> Vamos a diseñar el dashboard paso a paso. Por cada paso, dame instrucciones técnicas para Power BI y luego genera el texto de justificación que yo copiaré en mi informe final.
>
> **Paso 1: Modelado y DAX (Preparación)**
> *   Sugiéreme al menos 2 medidas DAX (ej. promedios, acumulados o variaciones porcentuales) que aporten valor al análisis temporal. 
> *   Indícame si necesito crear una tabla "Calendario" (Date Table) para manejar correctamente la inteligencia de tiempo.
>
> **Paso 2: Selección de las 5 Visualizaciones (El Algoritmo del Diseñador)**
> Propón exactamente 5 gráficos distintos aplicando estas reglas estrictas:
> *   **Gráfico 1 (Evolución temporal):** Debe ser un gráfico de líneas para mostrar tendencias cruzadas.
> *   **Gráfico 2 (Comparación/Ranking):** Debe ser un gráfico de barras (con el eje X comenzando en cero).
> *   **Gráfico 3 (Distribución o Correlación):** Sugiere un histograma, mapa de calor o dispersión.
> *   **Gráfico 4 (Relación Parte-Todo o KPI):** Usa tarjetas (Cards) o un gráfico de tarta/anillo *solo* si tiene un máximo de 3 variables categóricas. Si tiene más, sugiere barras apiladas al 100%.
> *   **Gráfico 5:** A tu elección, pero que sea interactivo (ej. un mapa si hay datos geográficos, o un *Small Multiples* / Múltiplos pequeños).
>
> **Paso 3: Psicología del Diseño e Interactividad (Gestalt & UI)**
> *   Dime cómo organizar visualmente los gráficos usando la **Ley de Proximidad** y el **Poder del Cercado (Enclosure)** usando fondos o bordes suaves en Power BI.
> *   Asegúrate de recomendar tipografía **Sans-Serif** (ej. Segoe UI, Arial) con un tamaño **mayor a 13px** para pantallas.
> *   Sugiere 2 segmentadores de datos (Slicers) estratégicos e indícame cómo configurar la interacción visual (Filtrar vs. Resaltar) para no saturar cognitivamente al usuario.
>
> **🛑 REGLA DE ORO - DOCUMENTACIÓN PARA MI INFORME (MARKDOWN):**
> Una vez definamos las visualizaciones, **DEBES generar automáticamente el texto para mi documento Markdown**. El formato debe ser exactamente este:
>
> ### 3.2 Descripción de las Visualizaciones Creadas
> **1. [Nombre del Gráfico] (Tipo de Gráfico)**
> *   **Variables usadas:** [Eje X, Eje Y, Leyenda].
> *   **Justificación Visual y Psicológica:** [Explica por qué se eligió según el algoritmo del diseñador (ej. "Alta expresividad por longitud/posición") y cómo aplica principios de Gestalt].
> *   **Interpretación:** [Dime qué hallazgo temporal o patrón se busca evidenciar aquí].
> *(Repetir para los 5 gráficos)*
>
> ### 3.3 Interactividad y Usabilidad
> *   **Configuración:** [Explicación de los Slicers, tipografía Sans-Serif >13px y leyes de Gestalt aplicadas al lienzo].
> 
> ¿Entendido? Si es así, dime "¡Entendido! Por favor, recuérdame las columnas principales de tu dataset limpio para proponerte las 5 visualizaciones ideales y las medidas DAX que harán destacar tu dashboard."

***

### 💡 ¿Por qué este prompt garantiza tu nota?

1.  **Criterio "Variedad y Complejidad" (20%):** Fuerza a Claude a usar 5 tipos diferentes (Líneas, Barras, Histograma/Dispersión, Tarjetas/Anillos, Mapas/Small Multiples), asegurando la máxima puntuación en variedad.
2.  **Criterio "Interpretación y Presentación" (20%):** Integra la teoría exacta de tus PDFs. Habla del "Algoritmo del diseñador", exige tipografía Sans-Serif para pantallas y aplica expresividad visual por "Posición y Longitud". Esto demostrará al profesor que no solo arrastraste campos en Power BI, sino que aplicaste la teoría de la clase.
3.  **Criterio "Interactividad" (20%):** Le pide a Claude que justifique si una gráfica debe "Filtrar" o "Resaltar" (Highlight), un detalle técnico avanzado de Power BI que los profesores valoran mucho.
4.  **Integración con tu Workflow:** La regla de oro final le pide que te escupa todo en formato Markdown, listo para ir directo a la sección 3.2 y 3.3 de tu archivo `Arbelaez_Daniel_Actividad_Visualizacion.md`.

**Siguiente paso:** Envíale este prompt a Claude en tu extensión, pásale la lista de tus columnas ya limpias, y cuando te dé las instrucciones, simplemente constrúyelas en Power BI y pega su justificación en tu archivo `.md`. ¡Estás armando un proyecto de nivel profesional!