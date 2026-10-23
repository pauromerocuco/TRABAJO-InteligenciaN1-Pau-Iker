# README del proyecto: ¿Qué tipo de empresas optan por la inteligencia artificial?

Trabajo final de la asignatura **Inteligencia de Negocios I** (Grado en ADE, Universitat Politècnica de València, curso 2026-27).

**Autores:** Iker Melero Rodríguez y Pau Romero Cucó

## 1. Descripción general

Este repositorio contiene el código y el informe de un análisis de la adopción de la inteligencia artificial (IA) en una muestra de empresas españolas a partir del contenido de sus páginas web en 2023 y 2025.

El análisis responde a tres preguntas:

| Pregunta | Hipótesis | Resultado |
|---|---|---|
| ¿Qué empresas adoptan la IA y para qué la utilizan? | H1: más adopción en empresas grandes y sectores tecnológicos | Se confirma |
| ¿Ha aumentado su uso entre 2023 y 2025, y de qué forma? | H2: crecimiento desigual entre tamaños y sectores | Se confirma |
| ¿Se asocia el uso de la IA con mejores resultados empresariales? | H3: las empresas con IA son más productivas | No se confirma |

## 2. Contenido del repositorio

| Archivo / carpeta | Contenido |
|---|---|
| `proyectogrande1.Rmd` | Informe completo en R Markdown: código, tablas, gráficos y texto |
| `proyectogrande1.pdf` | Informe final generado a partir del `.Rmd` |
| `IMAGENES/` | Capturas de las tablas dinámicas elaboradas en Excel |
| `README.md` | Este archivo |
| `DATOS/` | Carpeta donde deben colocarse los datos originales (**no incluidos**) |

## 3. Datos utilizados

Se utilizan los cuatro datasets facilitados por el profesorado de la asignatura:

| Archivo | Contenido principal | Unidad de observación | Registros |
|---|---|---:|---:|
| `indicadores_ia_web_2023.xlsx` | Indicadores de IA detectados en páginas web en 2023 | Página web–año | 16.805 |
| `indicadores_ia_web_2025.xlsx` | Indicadores de IA detectados en páginas web en 2025 | Página web–año | 16.252 |
| `muestra_empresarial_sabi.xlsx` | Identificación, localización, actividad y tamaño de las empresas | Empresa | 20.470 |
| `informacion_financiera.xlsx` | Información financiera, empleo, capital e innovación | Empresa | 8.100 |

### Relación entre las tablas

| Unión | Variable de enlace |
|---|---|
| Indicadores de IA ↔ Muestra SABI | URL de cada año (`url` ↔ `URL.REDIRECCIONADAS.2023` / `URL.REDIRECCIONADAS.2025`) |
| Muestra SABI ↔ Información financiera | Código de empresa `BvD` |

## 4. Preparación de los datos

| Problema detectado | Tratamiento aplicado |
|---|---|
| 152 filas duplicadas en la información financiera | Eliminadas (quedan 7.948 empresas) |
| Valores «n.d.» en empleados (1.967, todos microempresas) | Convertidos en `NA`; no se imputan (faltantes tipo MAR) |
| Valor añadido mal calculado (superaba a las ventas en 7.406 empresas) | Recalculado como `SALES + MAT` (los consumos ya son negativos) |
| Fecha de constitución como número de serie de Excel | Convertida a formato fecha |
| Provincias escritas de dos formas y valor «0» | Unificadas; el «0» pasa a `NA` |
| Categorías de tamaño en inglés | Traducidas y ordenadas: Micro, Pequeña, Mediana, Grande |
| Columnas de configuración del modelo (constantes) | Eliminadas; indicadores renombrados |
| Webs compartidas por varias empresas (≈ 4.400 por año) | Excluidas, ya que el indicador mide la web y no la empresa |

**Base final:** 29.824 observaciones (empresa–año), 15.949 empresas distintas, 13.875 presentes en los dos años. El 41 % de las observaciones dispone de información financiera.

## 5. Variables creadas para el análisis

| Variable | Descripción |
|---|---|
| `usa_ia` | 1 si la web muestra evidencias de uso de IA; 0 en caso contrario |
| `grupo_ia` | Versión en texto de `usa_ia`: «Con IA» / «Sin IA» |
| `ia_produccion`, `ia_logistica`, `ia_marketing`, `ia_sistemas`, `ia_administracion`, `ia_desarrollo` | Uso de IA en cada una de las seis áreas de la empresa (0/1) |
| `ia_producto` | 1 si la empresa ofrece un producto o servicio basado en IA |
| `n_areas` | Intensidad de uso: número de áreas (0 a 6) en las que se usa IA |
| `VA2022`, `VA2023` | Valor añadido recalculado (miles de euros) |
| `productividad` | Valor añadido por empleado en 2023 (miles de euros por empleado) |
| `crec_ventas` | Variación porcentual de las ventas entre 2022 y 2023 |
| `antiguedad` | Años transcurridos desde la constitución de la empresa hasta 2023 |
| `Size` | Tamaño de la empresa: Micro, Pequeña, Mediana o Grande |

## 6. Técnicas de análisis

| Pregunta | Técnicas |
|---|---|
| P1. ¿Qué empresas adoptan IA? | Porcentajes por tamaño, sector y área; tablas y gráficos de barras |
| P2. Evolución 2023-2025 | Comparación de porcentajes por año; seguimiento de las empresas presentes en ambos años |
| P3. IA y productividad | Estadística descriptiva, regla IQR para valores atípicos, diagrama de caja, test t (Welch) y regresión lineal múltiple con diagnóstico de asunciones |

Además, se elaboraron tres **tablas dinámicas** en Excel a partir de los datos limpios:

1. Porcentaje de empresas con IA según su tamaño (2023 y 2025).
2. Productividad media según el tamaño y el uso de IA (2023).
3. Porcentaje de empresas con IA en las diez provincias con más empresas (2023 y 2025).

## 7. Cómo reproducir el análisis

1. Descargar el repositorio y abrirlo como proyecto en RStudio.
2. Crear la carpeta `DATOS/` y colocar en ella los cuatro archivos de datos originales.
3. Instalar los paquetes necesarios (solo la primera vez):
   ```r
   install.packages(c("tidyverse", "readxl", "knitr", "ggthemes"))
   ```
4. Abrir `proyectogrande1.Rmd` y pulsar **Knit** para generar el informe en PDF.

**Software:** R 4.6 · RStudio · R Markdown · tidyverse · readxl · knitr · ggthemes · Microsoft Excel.

## 8. Principales resultados

| Resultado | Valor |
|---|---|
| Empresas con IA en su web | 3,3 % (2023) → 4,5 % (2025) |
| Adopción por tamaño (2025) | Micro 3,5 % · Pequeña 6,0 % · Mediana 9,0 % |
| Sector con mayor adopción (2025) | Información y comunicaciones: 25,8 % |
| Diferencia de productividad con y sin IA | No significativa (test t: p = 0,455) |
| Efecto de la IA a igual tamaño, sector y antigüedad | Nulo (regresión: coeficiente −0,001; p = 0,977) |

