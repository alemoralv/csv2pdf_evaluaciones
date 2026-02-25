# csv2pdf_evaluaciones

Genera reportes PDF de evaluaciones a partir de archivos CSV o Excel. Convierte los datos en un documento LaTeX profesional y lo compila a PDF.

---

## Requisitos previos

### 1. Python 3

Verifica que tienes Python instalado:

```bash
python --version
```

Si no tienes Python, descárgalo desde [python.org](https://www.python.org/downloads/).

### 2. Dependencias de Python

Instala las librerías necesarias ejecutando:

```bash
pip install requests
```

**Solo si vas a usar archivos Excel (.xlsx o .xls)**, instala también:

```bash
pip install openpyxl
```

### 3. LaTeX (opcional)

El script intenta compilar el PDF primero por internet. Si falla, usa `pdflatex` instalado en tu equipo. Si quieres compilación local sin depender de internet:

- **Windows:** Instala [MiKTeX](https://miktex.org/download)
- **Mac:** Instala [MacTeX](https://www.tug.org/mactex/)
- **Linux:** `sudo apt install texlive-latex-base texlive-latex-extra` (o equivalente)

---

## Estructura del proyecto

Tu carpeta debe contener:

| Archivo | Obligatorio | Descripción |
|---------|-------------|-------------|
| `instrucciones.txt` | Sí | Instrucciones o contexto del agente evaluado |
| `informacion.txt` | Sí | Metadatos: evaluador, agente, modelo, etc. |
| `*.csv` o `*.xlsx` o `*.xls` | Sí | Datos de las evaluaciones (solo uno de estos) |
| `txt2latex.py` | Sí | Script principal |

---

## Formato de `informacion.txt`

Cada línea debe tener el formato `Clave: Valor`. Ejemplo:

```
Evaluador: Alejandro Morera Alvarez
Agente: LSAR (Carbot)
Modelo: GPT-4.1
Conocimiento: Ley de SAR
Búsqueda Web: no
Conocimiento General: no
Orquestación: no
Herramientas: no
```

Claves reconocidas: `Evaluador`, `Agente`, `Modelo`, `Conocimiento`, `Búsqueda Web`, `Conocimiento General`, `Orquestación`, `Herramientas`.

---

## Formato del CSV o Excel

El archivo debe tener una fila de encabezados. Se aceptan varios nombres para cada columna:

| Columna interna | Nombres aceptados (ejemplos) |
|-----------------|-----------------------------|
| Pregunta | `question`, `pregunta` |
| Respuesta esperada | `expectedResponse`, `expected response`, `respuesta esperada` |
| Respuesta del agente | `actualResponse`, `agent's response`, `respuesta del agente` |
| Resultado | `result`, `resultado` |
| Explicación | `explanation`, `análisis`, `explicación` |
| Método de evaluación | `testMethodType`, `test method`, `método de prueba` |
| Calificación aprobatoria | `passingScore`, `passing score` |
| Contexto recuperado | `retrievedContext`, `contexto recuperado` |
| Modelo generador | `generatorModel`, `modelo generador` |

La columna de **pregunta** es obligatoria. Las demás son opcionales.

---

## Cómo ejecutar

### Opción A: Desde la carpeta del proyecto

Abre la terminal, entra a la carpeta del proyecto y ejecuta:

```bash
cd ruta\a\evaluacionesPRO
python txt2latex.py
```

Ejemplo en Windows:

```bash
cd C:\Users\alexm\projects\Profuturo\evaluacionesPRO
python txt2latex.py
```

### Opción B: Especificando la carpeta

Si estás en otra ubicación, indica la ruta de la carpeta del proyecto:

```bash
python txt2latex.py C:\Users\alexm\projects\Profuturo\evaluacionesPRO
```

En Mac/Linux:

```bash
python txt2latex.py /ruta/a/evaluacionesPRO
```

---

## Salida

El script crea la carpeta `output/` (si no existe) y genera:

- Un archivo `.tex` (LaTeX)
- Un archivo `.pdf` (reporte final)

Los nombres siguen el patrón: `{Agente}_{Evaluador}_evaluaciones.tex` y `.pdf`.

Ejemplo: `LSAR_Carbot_Alejandro_Morera_Alvarez_evaluaciones.pdf`

---

## Resolución de problemas

### "No se encontró ningún archivo .csv / .xlsx / .xls"

- Asegúrate de tener exactamente un archivo CSV o Excel en la carpeta del proyecto.
- Si hay varios, se usará el primero que encuentre.

### "No se encontró una columna de pregunta"

- Revisa que la primera fila del CSV/Excel tenga un encabezado de pregunta (`question`, `pregunta`, etc.).

### "No se encontró 'informacion.txt'" o "No se encontró 'instrucciones.txt'"

- Ambos archivos deben estar en la misma carpeta que el script o la carpeta que indicaste.

### "No se pudo compilar a PDF via API"

- El script intenta compilar por internet. Si falla, usará `pdflatex` local.
- Si tampoco tienes LaTeX instalado, el archivo `.tex` se genera igual; puedes compilarlo manualmente con un editor LaTeX o con `pdflatex archivo.tex`.

### Error de codificación en el CSV

- Guarda el CSV en UTF-8. En Excel: "Guardar como" → "CSV UTF-8 (delimitado por comas)".

---

## Resumen rápido (copiar y pegar)

```bash
# 1. Instalar dependencias (solo la primera vez)
pip install requests

# 2. Ir a la carpeta del proyecto
cd C:\Users\alexm\projects\Profuturo\evaluacionesPRO

# 3. Ejecutar
python txt2latex.py
```

El PDF aparecerá en la carpeta `output/`.
