# Procesamiento Masivo de Currículums Vitae con LLM

Este repositorio documenta el desarrollo de un sistema automatizado para la extracción estructurada de información desde Currículums Vitae en formato PDF, utilizando modelos de lenguaje de última generación (LLMs).

## Objetivo

Transformar grandes volúmenes de CVs en archivos estructurados `.csv` que puedan ser integrados fácilmente en sistemas de gestión de recursos humanos o utilizados para análisis posterior.

## Modelo de Lenguaje Utilizado

Se evaluaron múltiples modelos de lenguaje antes de seleccionar [`Zephyr-7b-beta`](https://huggingface.co/HuggingFaceH4/zephyr-7b-beta), un modelo de tipo instruct open source, por su capacidad de comprensión en español y su rendimiento optimizado. Fue desplegado sobre una GPU Nvidia con carga en 4 bits, lo que permitió alcanzar un tiempo promedio de respuesta de aproximadamente 20 segundos por prompt.

## Preprocesamiento

Para la lectura y extracción de texto desde archivos PDF, se empleó la librería `pdfplumber`. El texto resultante fue posteriormente procesado por el modelo de lenguaje, guiado mediante un prompt diseñado específicamente para devolver un único objeto JSON por CV, conteniendo los campos clave relevantes para el análisis laboral.

El prompt utilizado fue el siguiente:

PROMPT_TEMPLATE = """
Eres un asistente que extrae información estructurada de currículums vitae (CV) en español.

Devuelve exclusivamente un sólo objeto JSON con las siguientes claves y NINGÚN texto adicional:

nombre (puede estar en el encabezado)

domicilio

telefono

titulo (titulo universitario)

institucion (institución desde la que se egreso si es universitaria)

anios_experiencia (respuesta int con un sólo valor suma de los años que van desde el primer trabajo al último)

cantidad_trabajos (respuesta int con un sólo valor que será mayor a uno si ha habido cambio de trabajo)

ultimo_empleador

Curriculum del que se extraera la informacion:
"""




## Arquitectura del Procesamiento

El sistema se compone de tres funciones principales:

1. **Extracción** del texto desde archivos PDF.
2. **Inferencia** mediante LLM para obtención del JSON estructurado.
3. **Agregación y transformación** (ETL) de los resultados en una lista de objetos que luego se convierten en un `DataFrame` de `pandas`.

Finalmente, se exporta el resultado como un archivo `.csv`, listo para ser utilizado en sistemas de análisis o carga automatizada.

---

