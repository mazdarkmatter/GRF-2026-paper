# GRF 2026 — Compilación LaTeX

Documento del ensayo para los **Gravity Research Foundation Awards 2026**, en dos versiones:

- `./TEX/GRF_2026_<version>_EN.tex` — versión en inglés
- `./TEX/GRF_2026_<version>_ES.tex` — versión en español
- `./TEX/GRF_ECUATION_vN_ES.tex` — notas técnicas/matemáticas en español (versionadas por incremento)

---

## Publicación en Zenodo

Este repositorio está preparado para archivado en Zenodo con:

- `LICENSE.md` (CC BY 4.0)
- `.zenodo.json` (metadatos del depósito)

### Flujo recomendado (GitHub + Zenodo)

1. Crear o actualizar una **Release** en GitHub (tag de versión).
2. Zenodo captura esa release y genera DOI.
3. Copiar el DOI en este README (sección de citación).

### Cita sugerida

Hasta tener DOI asignado:

```text
Mazuela, Guillermo Rodrigo (2026). Gravity Rainbow Formulation (GRF 2026 Essay), v17.3.222.
```

### Script de publicación (CLI)

Definir token:

```bash
export ZENODO_TOKEN="tu_token"
```

Publicar en producción (Zenodo real):

```bash
./zenodo_publish.sh PDF/GRF_2026_v17.3.222_EN.pdf TEX/GRF_2026_v17.3.222_EN.tex
```

Probar en sandbox sin publicar (solo borrador):

```bash
./zenodo_publish.sh --sandbox --draft-only PDF/GRF_2026_v17.3.222_EN.pdf
```

---

## Estructura de directorios

```
.
├── TEX/          ← fuentes .tex e imágenes
├── PDF/          ← PDFs finales generados
├── tmp/          ← archivos auxiliares (.aux, .log, .out)
├── GRF2026.sh    ← script de compilación
└── README.md
```

---

## Flujo de trabajo

- Los archivos `.tex` y las imágenes van en `./TEX/`.
- Se compila con `GRF2026.sh` — siempre **2 pasadas** automáticas.
- Método oficial del repositorio: usar `./GRF2026.sh` para compilar (evitar compilación manual salvo depuración puntual).
- Los archivos auxiliares (`.aux`, `.log`, `.out`) quedan en `./tmp/`.
- El PDF final va a `./PDF/`.

---

## Preparación (primera vez)

```bash
mkdir -p ./TEX ./tmp ./PDF
chmod +x GRF2026.sh
```

---

## Compilación con el script

```bash
bash GRF2026.sh TEX/<nombre_sin_extension>
```

Publicar a GitHub solo si la compilación termina bien:

```bash
bash GRF2026.sh TEX/<nombre_sin_extension> --publish
```

Publicar con comentario manual:

```bash
bash GRF2026.sh TEX/<nombre_sin_extension> --publish --msg "tu comentario"
```

### Ejemplos

```bash
bash GRF2026.sh TEX/GRF_2026_<version>_EN
bash GRF2026.sh TEX/GRF_2026_<version>_ES
bash GRF2026.sh TEX/GRF_PORTADA_v<version>_ES
```

El script:
1. Verifica que el archivo `<nombre>.tex` exista en la ruta pasada
2. Crea `./tmp/` y `./PDF/` si no existen
3. Compila 2 pasadas con `pdflatex`
4. Mueve el PDF resultante a `./PDF/`
5. Se detiene en el primer error
6. Si usas `--publish`, hace `git add -A`, `commit` y `push` **solo si todo lo anterior salió bien**

---

## Git rápido (add + commit + push)

Para automatizar publicación con un solo comando:

```bash
./git_publish.sh "comentario del commit"
```

Si no pasas comentario, genera comentario con Codex usando `./.githubprompt`
y lo guarda en `./.githubcoment`:

```bash
./git_publish.sh
```

### Nota importante (GRF2026.sh)

En este repositorio, el flujo activo usa `GRF2026.sh` y espera la ruta con prefijo `TEX/`:

```bash
bash GRF2026.sh TEX/GRF_2026_v<version>_EN
bash GRF2026.sh TEX/GRF_2026_v<version>_ES
bash GRF2026.sh TEX/GRF_PORTADA_v<version>_ES
```

Si se omite `TEX/`, el script falla con: `Error: no se encontró <archivo>.tex`.

---

## Compilación manual (sin script)

### Una versión

```bash
pdflatex -output-directory=./tmp ./TEX/<nombre>.tex && \
pdflatex -output-directory=./tmp ./TEX/<nombre>.tex && \
mv ./tmp/<nombre>.pdf ./PDF/
```

### Dos versiones de una vez

```bash
pdflatex -output-directory=./tmp ./TEX/<nombre>_EN.tex && \
pdflatex -output-directory=./tmp ./TEX/<nombre>_EN.tex && \
mv ./tmp/<nombre>_EN.pdf ./PDF/ && \
pdflatex -output-directory=./tmp ./TEX/<nombre>_ES.tex && \
pdflatex -output-directory=./tmp ./TEX/<nombre>_ES.tex && \
mv ./tmp/<nombre>_ES.pdf ./PDF/
```

---

## Imagen requerida

Las imágenes referenciadas en el `.tex` deben estar en `./TEX/IMG/`:

```
./TEX/IMG/GRF2026_v<version>.png
```

---

## Checklist de requisitos GRF 2026

Basado en el documento oficial `2026+GRF+Awards+for+Essays.pdf`.

### Formato ✓ (verificado en .tex)

- [x] Fuente 12pt — `\documentclass[12pt,...]`
- [x] Márgenes 1 pulgada — `\usepackage[margin=1in]{geometry}`
- [x] ~~Doble espacio en cuerpo — `\doublespacing` tras `\newpage`~~ verificado ✓ (abstract en portada va en simple, cuerpo en doble — correcto)
- [x] ~~Simple espacio en portada — `\singlespacing` en portada~~ verificado ✓
- [x] Una sola columna — clase `article` por defecto
- [x] Páginas numeradas — `\rhead{\thepage}`, contador desde 1 en cuerpo

### Portada — verificar manualmente en PDF

- [ ] Título del ensayo presente
- [ ] Nombre(s) del autor(es) — indicar cuál es el **corresponding author**
- [ ] Email y dirección postal del autor
- [ ] Fecha de envío
- [ ] Abstract ≤ 125 palabras — **actual: 106 palabras** ✓
- [ ] Statement exacto requerido (verificar redacción):
  > *"Essay written for the Gravity Research Foundation 2026 Awards for Essays on Gravitation."*
  > ⚠️ El `.tex` actual tiene el orden incorrecto — corregir

### Contenido — verificar manualmente en PDF

- [ ] Máximo **10 páginas** de cuerpo (portada y referencias NO cuentan)
- [ ] Sin cálculos matemáticos extensos ni descripciones detalladas de setup experimental
- [ ] Ensayo autocontenido — no depende de leer otros documentos
- [ ] Número reducido de diagramas, tablas y ecuaciones

### Envío

- [ ] Un solo PDF en inglés
- [ ] Enviado antes del **31 de marzo de 2026, medianoche**
- [ ] A: `grideoutjr@aol.com`
- [ ] Confirmar recepción dentro de 24 horas

---

## Opciones útiles de pdflatex

| Opción | Descripción |
|--------|-------------|
| `-output-directory=./tmp` | Directorio para todos los archivos de salida (PDF, aux, log, out) |
| `-jobname=<nombre>` | Cambia el nombre del archivo de salida sin renombrar el .tex |
| `-interaction=nonstopmode` | No se detiene ante errores, compila hasta el final |
| `-interaction=batchmode` | Igual que nonstopmode pero sin mensajes en terminal |
| `-interaction=errorstopmode` | Se detiene en cada error (modo por defecto) |
| `-shell-escape` | Permite ejecutar comandos del sistema desde el .tex |
| `-synctex=1` | Genera archivo `.synctex.gz` para sincronización editor↔PDF |
| `-draft` | Compilación rápida: no incrusta imágenes, muestra cajas negras |
| `-draftmode` | No genera PDF (solo auxiliares, útil para la primera pasada) |
| `-halt-on-error` | Se detiene en el primer error sin preguntar |
| `-file-line-error` | Muestra errores con formato `archivo:línea: error` |
| `-quiet` | Reduce mensajes en terminal (alias: `-q`) |
| `-recorder` | Genera archivo `.fls` con lista de todos los archivos usados |
| `-parse-first-line` | Lee la primera línea del .tex para opciones especiales |

---

## Archivos generados por pdflatex

| Extensión | Descripción |
|-----------|-------------|
| `.pdf` | Documento final |
| `.aux` | Referencias internas (usado entre pasadas) |
| `.log` | Log completo de la compilación |
| `.out` | Datos de navegación/links (paquete hyperref) |
| `.synctex.gz` | Sincronización editor↔PDF (si se usa `-synctex=1`) |
| `.fls` | Lista de archivos usados (si se usa `-recorder`) |

---

## PROMPT IA

Instrucciones para el asistente IA al modificar archivos `.tex`:

- **Siempre aumentar la versión** del archivo `.tex` al realizar cualquier cambio de contenido
- El nombre del archivo sigue el patrón: `GRF_2026_v<mayor>.<menor>.<patch>_<idioma>.tex`
- Ejemplo: si se modifica `GRF_2026_v17.3.213_EN.tex`, el archivo resultante debe ser `GRF_2026_v17.3.214_EN.tex`
- Aplicar el mismo incremento de versión a ambos idiomas (EN y ES) cuando el cambio es equivalente en ambos
- No sobreescribir la versión anterior — crear archivo nuevo con versión incrementada
- **Nunca realizar cambios al `.tex` sin VB (visto bueno) explícito del autor** — presentar primero qué se va a cambiar y esperar confirmación
- **SIEMPRE crear archivo nuevo con versión incrementada antes de editar** — nunca editar el archivo existente directamente. Ejemplo: editar `v17.3.215` → crear `v17.3.216` con los cambios

---

## Hallazgos recientes (Febrero 2026)

- Versionado de ecuaciones actualizado:
  - Se mantiene `TEX/GRF_ECUATION_v8_ES.tex` sin cambios
  - Nueva versión creada: `TEX/GRF_ECUATION_v9_ES.tex` (incluye sección 6.4: dispersión cromática y \(\epsilon\) observable)
- Portada standalone unificada con la última versión de contenido disponible:
  - Fuente de verdad: `TEX/GRF_2026_v17.3.214_ES.tex`
  - Nueva portada: `TEX/GRF_PORTADA_v17.3.228_ES.tex`
- Flujo correcto confirmado:
  - Compilar con `GRF2026.sh` (2 pasadas automáticas)
  - Salida PDF en `./PDF/`
  - Auxiliares en `./tmp/`
- Limpieza:
  - No dejar `*.aux`, `*.log`, `*.pdf` sueltos en la raíz del proyecto
  - Si aparecen por compilación manual, mover/eliminar y conservar solo `tmp/` y `PDF/`
- Estado técnico actual:
  - `GRF_PORTADA_v17.3.228_ES` compila correctamente
  - Persisten `LaTeX Font Warning` por sustitución de tamaños en títulos grandes (no bloquea la compilación)

---

## Próximos pasos

- [ ] Corregir statement en portada (orden de palabras incorrecto en `.tex`)
- [ ] Verificar conteo de páginas del cuerpo (máx. 10)
- [ ] Confirmar corresponding author marcado en portada
- [ ] Confirmar fecha de envío en portada
- [ ] Compilar versión EN final y revisar PDF completo
- [ ] Enviar a `grideoutjr@aol.com` antes del 31 de marzo de 2026
