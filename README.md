# Sprinfield-vs-Shelbyville-

[![Licencia MIT](https://img.shields.io/badge/Licencia-MIT-blue.svg)](LICENSE)
[![Python Version](https://img.shields.io/badge/Python-3.x-blue.svg)](https://www.python.org/)

## Contenido
- [Introducción](#introducción)
- [Etapa 1. Descripción de los Datos](#etapa-1-descripción-de-los-datos)
  - [Observaciones Iniciales](#observaciones-iniciales)
  - [Documentación del Dataset](#documentación-del-dataset)
- [Etapa 2. Preprocesamiento de Datos](#etapa-2-preprocesamiento-de-datos)
  - [2.1 Estilo del Encabezado](#21-estilo-del-encabezado)
  - [2.2 Valores Ausentes](#22-valores-ausentes)
  - [2.3 Duplicados](#23-duplicados)
  - [2.4 Implicit Duplicates in Genre](#24-duplicados-implícitos-en-genre)
  - [Conclusiones del Preprocesamiento](#conclusiones-del-preprocesamiento)
- [Etapa 3. Prueba de Hipótesis](#etapa-3-prueba-de-hipótesis)
  - [Hipótesis 1: Actividad en Springfield vs. Shelbyville](#hipótesis-1-actividad-en-springfield-vs-shelbyville)
- [Conclusiones Generales](#conclusiones-generales)
- [Aplicaciones en Contextos de Negocios y Finanzas](#aplicaciones-en-contextos-de-negocios-y-finanzas)
- [Instalación y Uso](#instalación-y-uso)
- [Contribución](#contribución)
- [Licencia](#licencia)
- [Contacto](#contacto)

---

## Introducción
Este es es un proyecto orientado al análisis de datos musicales recopilados de usuarios de una plataforma de streaming. El objetivo es describir y preprocesar los datos para poder realizar una prueba de hipótesis acerca del comportamiento musical de los usuarios en dos ciudades, **Springfield** y **Shelbyville**, durante días específicos de la semana. Este estudio puede ayudar a comprender mejor las preferencias de los usuarios y orientar decisiones de marketing o estrategias de monetización.

---

## Etapa 1. Descripción de los Datos

### Observaciones Iniciales
El proyecto utiliza el dataset `music_project_en.csv` ubicado en la carpeta `/datasets/`. Al cargar los datos con **pandas**, se observan las primeras 10 filas que revelan información sobre:
- **userID**: Identificador del usuario.
- **Track**: Título de la canción.
- **artist**: Nombre del artista.
- **genre**: Género de la pista.
- **City**: Ciudad del usuario.
- **time**: Hora exacta de reproducción.
- **Day**: Día de la semana.

### Documentación del Dataset
Según la documentación:
- Los encabezados presentan problemas de formato:
  - Inconsistencia en mayúsculas/minúsculas.
  - Espacios innecesarios.
  - Uso no uniforme de _snake_case_ (por ejemplo, `userID` en lugar de `user_id`).
- Se detectan valores nulos en las columnas **track**, **artist** y **genre**, lo cual puede afectar análisis futuros (por ejemplo, determinar las canciones o artistas más populares).

---

## Etapa 2. Preprocesamiento de Datos

El objetivo en esta fase es estandarizar los encabezados, tratar los valores ausentes y eliminar duplicados tanto explícitos como implícitos.

### 2.1 Estilo del Encabezado
- **Conversión a minúsculas:** Se recorre la lista de encabezados para transformar todos los caracteres en minúsculas.
- **Eliminación de espacios:** Se eliminan los espacios en blanco iniciales y finales.
- **Formato _snake_case_:** Se renombra la columna `userid` a `user_id` para cumplir con el estándar.

### 2.2 Valores Ausentes
- Se utiliza el método `.isna().sum()` para identificar la cantidad de valores ausentes por columna.
- Los valores nulos en **track**, **artist** y **genre** se reemplazan por el string `'unknown'` para no afectar futuros análisis, aunque se reconoce que esto puede alterar el recuento de géneros o artistas populares.

### 2.3 Duplicados
- Se identifican 3826 filas duplicadas utilizando `.duplicated().sum()`.
- Se eliminan los duplicados explícitos con el método `.drop_duplicates()`, dejando un total de 61,252 registros.

### 2.4 Duplicados Implícitos en `genre`
- Se observa que existen variantes del género _hiphop_ (por ejemplo, `'hip'`, `'hop'`, `'hip-hop'`).
- Se define una función `replace_wrong_genres()` que, a partir de una lista de valores incorrectos, reemplaza dichos valores por el correcto (`hiphop`), asegurando consistencia en la columna `genre`.

### Conclusiones del Preprocesamiento
- Se logró estandarizar los encabezados y eliminar duplicados.
- Se gestionaron los valores ausentes, asegurando que el dataset quede limpio para análisis posteriores.
- La corrección de duplicados implícitos en el género mejora la calidad de las comparaciones sobre tendencias musicales.

---

## Etapa 3. Prueba de Hipótesis

### Hipótesis 1: Actividad en Springfield vs. Shelbyville
La hipótesis plantea que existen diferencias en el comportamiento musical de los usuarios de **Springfield** y **Shelbyville**, particularmente en los días lunes, miércoles y viernes.

- **Agrupación por Ciudad:**  
  Se agrupa el dataset por la columna `city` y se cuenta el número de canciones reproducidas en cada grupo.  
  - *Ejemplo de resultado:*  
    - Shelbyville: 18,512 reproducciones  
    - Springfield: 42,741 reproducciones

- **Agrupación por Día:**  
  Se cuenta la cantidad de reproducciones para los días **Monday**, **Wednesday** y **Friday**.  
  - *Ejemplo de resultado:*  
    - Friday: 21,840  
    - Monday: 21,354  
    - Wednesday: 18,059

- **Función `number_tracks()`:**  
  Se crea una función que recibe como parámetros un día y una ciudad, y devuelve el número de canciones reproducidas, permitiendo comparar los comportamientos de ambas ciudades en los días analizados.
  
  *Ejemplos de resultados obtenidos:*
  - Springfield, Monday: 15,740
  - Shelbyville, Monday: 5,614
  - Springfield, Wednesday: 11,056
  - Shelbyville, Wednesday: 7,003
  - Springfield, Friday: 15,945
  - Shelbyville, Friday: 5,895

---

## Conclusiones Generales
- **Comparación entre Ciudades:**  
  Springfield registra significativamente más reproducciones que Shelbyville.  
  - En Springfield, las reproducciones son más elevadas los lunes y viernes, y disminuyen los miércoles.  
  - En Shelbyville, se observa una mayor actividad el miércoles.
  
- **Impacto de los Datos Faltantes:**  
  La presencia de valores ausentes en columnas clave como **genre** puede influir en los análisis de preferencias y, por ende, en la formulación de estrategias.

---

## Aplicaciones en Contextos de Negocios y Finanzas
Este proyecto, aunque centrado en la música, utiliza técnicas y metodologías aplicables a otros ámbitos:
- **Análisis de Comportamiento del Consumidor:**  
  Las técnicas de agrupación y filtrado pueden aplicarse para analizar hábitos de compra en el retail o transacciones financieras.
- **Toma de Decisiones Basadas en Datos:**  
  El preprocesamiento y análisis de datos limpios es fundamental para realizar predicciones y tomar decisiones informadas en áreas como marketing, gestión de riesgos o análisis de inversiones.
- **Optimización de Estrategias de Marketing:**  
  Conocer el comportamiento de los usuarios según la ciudad y día permite segmentar audiencias y diseñar campañas publicitarias específicas, optimizando recursos financieros y maximizando el retorno de inversión.

---

## Instalación y Uso

### Requisitos
- Python 3.x
- pandas

### Pasos para Ejecutar el Proyecto
1. **Clonar el repositorio:**
   ```bash
   git clone https://github.com/tu_usuario/dejame-escuchar-la-musica.git

