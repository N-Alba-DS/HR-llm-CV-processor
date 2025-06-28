# Unstructured Text Curriculum Vitae Processing with LLMs

This repository documents the development of an automated system for structured information extraction from Curriculum Vitae in PDF format, using large language models (LLMs). For personal data protection reasons, all CVs included in this repository are synthetic and do not contain real information.

## Objective

To convert large volumes of CVs into structured `.csv` files that can be easily integrated into human resource management systems or used for further analysis.

## Language Model Used

Several language models were evaluated before selecting [`Zephyr-7b-beta`](https://huggingface.co/HuggingFaceH4/zephyr-7b-beta), an open-source instruction model, for its strong comprehension in Spanish and optimized performance. After several tests, it was deployed on a Nvidia GPU with 8GB VRAM, quantized to 4 bits, achieving an average response time of approximately 20 seconds per prompt.

## Preprocessing

The `pdfplumber` library was used to read and extract text from PDF files. The resulting text was then processed by the language model, guided by a specifically designed prompt to return a single JSON object per CV, containing key fields relevant for job analysis.

The prompt used was:

```python
PROMPT_TEMPLATE = """
You are an assistant that extracts structured information from Curriculum Vitae (CVs) in Spanish.

Return only a single JSON object with the following keys and NO additional text:

nombre (name, may appear in the header)

domicilio (address)

telefono (phone number)

titulo (university degree)

institucion (graduated institution, if university-level)

anios_experiencia (int: total years from first to last job)

cantidad_trabajos (int: total number of jobs held)

ultimo_empleador (last employer)

Curriculum to extract information from:
"""

