# Recomendaciones de mejora

Documento elaborado por el colaborador externo (Orlando Villalobos Gutiérrez) durante
la revisión del proyecto **Organizador Personal**. Las siguientes recomendaciones están
orientadas a que el proyecto cumpla con los criterios de la práctica integradora y con
buenas prácticas generales de control de versiones.

## 1. Excluir el entorno virtual `.venv/` del repositorio

El repositorio contiene actualmente la carpeta `.venv/` con 490 archivos versionados
(rutas `.venv/Lib/site-packages/...`). Esto contradice el punto 7 de la práctica, que
pide verificar expresamente que `.venv` no se agregue al repositorio.

El entorno virtual no debe almacenarse por tres razones:

- Es específico de cada equipo (contiene binarios de Windows: `.venv/Scripts/python.exe`),
  por lo que no funciona en Linux ni en macOS.
- Se regenera con `python -m venv .venv` más `pip install -r requirements.txt`.
- Infla el historial del repositorio de forma innecesaria.

**Acción sugerida:** agregar en `.gitignore` una omisión explícita de `.venv/` y retirar
la carpeta del índice de Git sin borrarla del disco local.

## 2. Crear el archivo `.gitignore` en la raíz del proyecto

El proyecto no cuenta con un `.gitignore` en la raíz. El único archivo con ese nombre
está dentro de `.venv/` (`.venv/.gitignore`), que fue generado automáticamente por
`python -m venv` y únicamente ignora archivos dentro de esa misma carpeta.

**Acción sugerida:** crear `.gitignore` en la raíz con, como mínimo, las siguientes reglas:

    .venv/
    __pycache__/
    *.pyc
    .env
    .vscode/
    .DS_Store

## 3. Completar los archivos de documentación vacíos

Los siguientes archivos existen en el repositorio pero no tienen contenido:

- `LICENSE.txt` (0 bytes)
- `respuestas.txt` (0 bytes)
- `docs/descripcion.md` (0 bytes)

El punto 8 de la práctica pide describir el propósito del proyecto en
`docs/descripcion.md`. Asimismo, el punto 12 solicita que `respuestas.txt` contenga las
respuestas individuales, y el README declara una sección **Licencia** que actualmente no
tiene texto.

**Acción sugerida:** redactar el contenido de estos tres archivos antes de la entrega.

## 4. Retirar del repositorio la configuración del entorno de cada integrante

Dentro de la carpeta `.venv/` se encuentran los archivos `.venv/Lib/site-packages/pip/...`
y `.venv/pyvenv.cfg`, que registran las rutas absolutas del equipo donde se creó el
entorno (por ejemplo, rutas con la unidad `C:\`). Al versionarlos, cualquier otro
integrante que clone el repositorio hereda una configuración que no le corresponde.

Este punto se resuelve junto con la recomendación 1.

## 5. Revisar la convención de los mensajes de commit

El historial actual presenta dos mensajes repetidos con el mismo texto
(`Crea estructura inicial del proyecto`, commits `b594ca9` y `bc401d2`), además de un
error de escritura en el primer commit (`Rename funcioalidades.md to funcionalidades.md`).

Aunque la práctica solo pide mensajes claros, conviene que cada commit describa un cambio
distinto y que el mensaje esté en un solo idioma y sin errores de escritura, ya que el
historial es parte de la evidencia que se revisa.

## 6. Homologar el formato de los archivos de texto

`requirements.txt` y `docs/funcionalidades.md` están guardados con terminación de línea
de Windows (CRLF) y `README.md` con terminación de Unix (LF). Al mezclar formatos, Git
reporta diferencias de todo el archivo cuando se modifica una sola línea, lo que
dificulta la revisión de cambios en un Pull Request.

**Acción sugerida:** agregar un archivo `.gitattributes` con la regla
`* text=auto eol=lf` para que Git normalice las terminaciones de línea en todo el
proyecto.

## Resumen de archivos involucrados

| Archivo | Estado actual | Acción recomendada |
| --- | --- | --- |
| `.gitignore` (raíz) | No existe | Crear con las reglas mínimas |
| `.venv/` | Versionado (490 archivos) | Retirar del índice y añadir a `.gitignore` |
| `LICENSE.txt` | Vacío | Redactar el contenido |
| `respuestas.txt` | Vacío | Registrar las respuestas individuales |
| `docs/descripcion.md` | Vacío | Describir el propósito del proyecto |
| `README.md` | Secciones sin contenido | Completar objetivo, instalación y dependencias |
| `.gitattributes` | No existe | Crear para normalizar terminaciones de línea |
