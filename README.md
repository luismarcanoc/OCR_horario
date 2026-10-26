# OCR de horarios UNIMET

Aplicación de escritorio en Python que extrae las materias de planes de horario de la Universidad Metropolitana y las convierte en datos estructurados. Reconstruye las columnas del PDF, relaciona cada código con el nombre de la materia y genera archivos CSV y Excel listos para revisar o analizar.

El procesamiento se realiza localmente: los horarios no se envían a servicios externos.

## Funcionalidades principales

- Importación de horarios en formato PDF.
- Lectura directa de la capa de texto para obtener mayor precisión.
- OCR opcional con Tesseract para documentos escaneados.
- Validación del formato Plan Horario UNIMET.
- Interfaz gráfica con soporte para arrastrar y soltar archivos.
- Clasificación visual de las clases en seis categorías.
- Consolidación de materias repetidas en varios días.
- Exportación individual y consolidada a CSV y XLSX.
- Catálogo local para relacionar códigos y nombres de materias.
- Herramienta de línea de comandos para procesar varios archivos.

## Tecnologías

- Python
- pypdf
- openpyxl
- Tkinter y tkinterdnd2
- Pillow
- Tesseract OCR como componente opcional

## ¿Por qué no se aplica OCR a todos los archivos?

Los planes de horario habituales contienen una capa de texto. Leer esa información es más rápido y preciso que reconocer visualmente cada página, especialmente para códigos, secciones y aulas como `A2-002`.

Cuando el archivo proviene de un escáner y no contiene texto utilizable, la aplicación puede recurrir a Tesseract como alternativa local.

## Instalación

Requiere Python 3.10 o una versión posterior.

```powershell
git clone https://github.com/luismarcanoc/OCR_horario.git
cd OCR_horario
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

`openpyxl` permite generar libros XLSX. Si no está disponible, el programa conserva la salida CSV y muestra un aviso.

## Uso con interfaz gráfica

```powershell
python main.py
```

En la ventana puedes arrastrar un horario PDF o seleccionarlo desde el equipo. Después de validarlo, la aplicación muestra las clases y guarda el resultado dentro de `resultados/`.

Solo se aceptan documentos que contengan las marcas correspondientes al formato Plan Horario UNIMET.

## Procesamiento por línea de comandos

```powershell
python -m ocr_horarios.cli . --salida resultados
```

Los PDF cuyo nombre contiene `flujograma` se omiten automáticamente.

## Archivos generados

- Un CSV por cada horario procesado.
- `todos_los_horarios.xlsx`, con una hoja por PDF.
- `todos_los_horarios.csv`, con `CODIGO`, `PERIODO` y `ARCHIVO` para auditoría.

Las clases que comparten hora, aula, sección y profesor se consolidan en una sola fila. Por ejemplo: `Lunes / Miércoles`.

## Catálogo de materias

`data/materias.json` contiene la relación entre códigos y nombres obtenida del flujograma académico. Puede ampliarse manualmente.

Si una electiva `FGE...` no existe en el catálogo, el programa conserva el nombre disponible en el horario y nunca inventa datos faltantes.

## PDF escaneado y OCR opcional

```powershell
python -m pip install -r requirements-ocr.txt
```

También debes instalar Tesseract OCR con el idioma español y dejar `tesseract.exe` disponible en `PATH`. No es necesario para los PDF que ya contienen texto.

## Estructura principal

```text
ocr_horarios/   Lógica de extracción, normalización y exportación
data/           Catálogo local de materias
assets/         Recursos de la interfaz
tests/          Pruebas automatizadas
resultados/     Archivos generados localmente
main.py         Punto de entrada de la interfaz gráfica
```

## Privacidad

Los horarios pueden contener información académica o personal. Antes de publicar archivos de ejemplo, elimina nombres, identificadores y cualquier dato sensible. Para una demostración pública es preferible utilizar documentos anonimizados o datos sintéticos.

## Estado del proyecto

Proyecto funcional orientado a automatizar la lectura de horarios académicos y convertir documentos semiestructurados en datos reutilizables.
