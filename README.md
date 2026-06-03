# OCASA · Monitor de Sentimiento de Marca
**Customer Intelligence | Gerencia CX**

Dashboard navegable conectado a Google Sheets, publicado como página web estática en GitHub Pages (gratis, sin servidor).

---

## Arquitectura

```
Google Sheets (fuente de datos)
        ↓  CSV público
    index.html (GitHub Pages)
        ↓  fetch al abrir la página
    Browser renderiza todo
```

Cada vez que alguien abre la URL, el HTML descarga el CSV actualizado desde Google Sheets y construye el dashboard en el navegador. Sin backend, sin base de datos, sin costo.

---

## Paso 1 — Publicar el Google Sheet como CSV

1. Abrí tu Google Sheet con la base de reviews
2. `Archivo → Compartir → Publicar en la web`
3. En el primer dropdown seleccioná **la hoja exacta** (ej: "reviews_enriquecidas")
4. En el segundo dropdown seleccioná **Valores separados por comas (.csv)**
5. Hacé click en **Publicar** y confirmá
6. Copiá la URL que te da. Tiene esta forma:
   ```
   https://docs.google.com/spreadsheets/d/XXXX/pub?gid=0&single=true&output=csv
   ```
   > ⚠️ Guardá esta URL, la vas a necesitar en el Paso 3.

---

## Paso 2 — Crear el repositorio en GitHub

1. Entrá a [github.com](https://github.com) y creá una cuenta si no tenés
2. Click en **New repository** (botón verde)
3. Nombre: `ocasa-sentiment` (o el que prefieras)
4. Seleccioná **Public**
5. Click en **Create repository**

---

## Paso 3 — Configurar la URL del Sheet en el HTML

Abrí `index.html` en un editor de texto (Notepad, VS Code, etc.) y buscá esta línea cerca del comienzo del `<script>`:

```javascript
const SHEET_CSV_URL = "REEMPLAZAR_CON_URL_CSV_DE_GOOGLE_SHEETS";
```

Reemplazala con la URL del Paso 1:

```javascript
const SHEET_CSV_URL = "https://docs.google.com/spreadsheets/d/XXXX/pub?gid=0&single=true&output=csv";
```

Guardá el archivo.

---

## Paso 4 — Subir el archivo a GitHub

### Opción A — Desde el navegador (más simple)

1. En tu repositorio recién creado, click en **Add file → Upload files**
2. Arrastrá el archivo `index.html`
3. Click en **Commit changes**

### Opción B — Con Git (si tenés Git instalado)

```bash
git clone https://github.com/TUUSUARIO/ocasa-sentiment.git
cd ocasa-sentiment
cp /ruta/a/index.html .
git add index.html
git commit -m "Monitor de sentimiento OCASA"
git push
```

---

## Paso 5 — Activar GitHub Pages

1. En tu repositorio, click en **Settings** (pestaña superior)
2. En el menú izquierdo, click en **Pages**
3. En "Source" seleccioná **Deploy from a branch**
4. Branch: **main**, carpeta: **/ (root)**
5. Click en **Save**
6. Esperá ~2 minutos. Tu URL pública será:
   ```
   https://TUUSUARIO.github.io/ocasa-sentiment
   ```

---

## Actualización automática de datos

El dashboard **no requiere ninguna acción** para reflejar datos nuevos.

- Cada vez que alguien abre la URL, descarga el CSV actualizado de Google Sheets
- El botón **↻ Actualizar** en la barra superior fuerza una nueva descarga sin recargar la página
- Los datos del Sheet se reflejan instantáneamente (Google puede cachear el CSV hasta ~5 minutos)

Para agregar nuevas reviews: simplemente agregá filas al Google Sheet. La próxima visita al dashboard las incluirá.

---

## Columnas requeridas en el Sheet

El HTML espera exactamente estas columnas (mismas que el CSV original):

| Columna | Descripción |
|---|---|
| `fecha_estimada` | Fecha de la review (YYYY-MM-DD) |
| `autor` | Nombre del autor |
| `review` | Texto original de la review |
| `estrellas` | Calificación (ej: "1 estrella", "3 estrellas") |
| `cliente` | Cliente/marca asociada |
| `servicio` | Tipo de servicio |
| `punto_de_dolor` | Categoría del problema |
| `sentimiento` | Negativo / Positivo / Neutro |
| `sucursal` | Sucursal OCASA |
| `fuente` | google_maps / tuquejasuma |
| `resumen_review` | Resumen generado por IA |

---

## Solución de problemas

**El dashboard muestra "No se pudo cargar el Sheet"**
- Verificá que el Sheet esté publicado (Paso 1)
- Asegurate de copiar la URL correcta (debe terminar en `&output=csv` o `format=csv`)
- El Sheet debe ser **público** para que el HTML pueda leerlo

**Los datos no se actualizan**
- Google cachea el CSV publicado por hasta 5 minutos
- Usá el botón ↻ Actualizar en el dashboard

**GitHub Pages no aparece**
- Verificá que el repo sea **Public** (no Private)
- Esperá hasta 5 minutos después de activar Pages
- Revisá que el archivo se llame exactamente `index.html`

---

*Customer Intelligence | Gerencia CX — OCASA*
