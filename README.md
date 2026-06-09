# DEO Tools — Extensión para Trimble Connect 3D Viewer

DEO Tools es una extensión web para el **visor 3D de Trimble Connect** (Trimble Connect for Browser) que añade un panel lateral con herramientas para trabajar con objetos coloreados, exportar e importar colores por GUID, y hacer selecciones por proximidad.

Está pensada para flujos de revisión basados en IFC, donde los objetos se colorean por estado y se necesita volcar esa información a CSV (por ejemplo, para cruzarla con códigos de RFI), o a la inversa, recuperar un coloreado a partir de un CSV.

> Herramienta creada por **Sergi Artigas (DEO)**. Software libre bajo licencia MIT.

---

## Características

- **Exportar colores a CSV** de los objetos coloreados de la vista activa, con el GUID IFC de cada objeto.
- **Selección de columnas** marcables y **reordenables** (el orden de las flechas es el orden del CSV).
- **Detección automática del código de revisión (RFI)** a partir del título de la vista, con modos *Error codes* y *Custom codes*.
- **Columna `revisado_deo`** (TRUE/FALSE) y **columna condicionada `codigo_revision`** según el color del objeto.
- **Extended export**: recorre automáticamente varias vistas del proyecto y genera un único CSV combinado.
- **Importar CSV**: aplica colores a los objetos del modelo cargado a partir de un CSV con GUID + color. Los GUID que no estén en el modelo se omiten.
- **Selección cercanos**: añade a la selección los objetos en contacto (aproximado por *bounding box*) con el objeto seleccionado que cumplan una condición numérica (p. ej. *Net volume < 0.1*).
- **Zona de Desarrollador** con descarga de un informe de diagnóstico (`DEO_debug.txt`).
- Interfaz organizada en menús desplegables, con interruptores estilo Trimble y descripciones flotantes al pasar el ratón.

---

## Estructura del repositorio

```
.
├── index.html        # La extensión completa (HTML + CSS + JS en un solo archivo)
├── manifest.json     # Manifiesto que Trimble Connect lee para cargar la extensión
├── icon.svg          # Icono de la extensión (vectorial)
├── icon.png          # Icono alternativo en PNG (más robusto en algunos entornos)
├── README.md
├── CHANGELOG.md
├── CONTRIBUTING.md
└── LICENSE
```

La extensión es **100 % estática**: no necesita servidor, build ni dependencias más allá de la propia Workspace API de Trimble (que se carga por CDN desde el propio `index.html`). Por eso funciona directamente publicada en **GitHub Pages**.

---

## Instalación

### 1. Publica los archivos

La forma más sencilla es **GitHub Pages**:

1. Haz un *fork* o copia este repositorio en tu cuenta.
2. En el repositorio, ve a **Settings → Pages** y activa GitHub Pages sobre la rama `main` (carpeta raíz `/`).
3. Tu extensión quedará publicada en una URL del tipo:
   `https://TU_USUARIO.github.io/NOMBRE_DEL_REPO/`

Cualquier hosting estático con **HTTPS** sirve igual.

### 2. Añade la extensión en Trimble Connect

1. Abre **Trimble Connect for Browser** e inicia sesión.
2. Entra en la zona de **Extensiones** (Settings / Extensions del visor).
3. Añade una extensión personalizada indicando la **URL del manifiesto**:
   `https://TU_USUARIO.github.io/NOMBRE_DEL_REPO/manifest.json`
4. Abre un proyecto con un modelo cargado. El panel **DEO Tools** aparecerá en la barra lateral.

> La ubicación exacta del menú de extensiones puede variar según la versión de Trimble Connect. Consulta la documentación oficial de extensiones: https://developer.trimble.com/docs/connect/workspace-api/

---

## Uso

### Exportar colores a CSV

1. Sitúate en la vista cuyo coloreado quieras exportar (su título debe contener el código de revisión, p. ej. `(RFI_18861) MÓDULO B...`).
2. Abre **Herramientas del archivo → Exportar colores a CSV**.
3. Marca y ordena las columnas que quieras.
4. Pulsa **Exportar CSV**.

El archivo se descarga con separador `;`, codificación UTF‑8 (con BOM, para que Excel respete los acentos) y todas las celdas entre comillas.

#### Columnas disponibles

| Columna            | Descripción                                                        | Por defecto |
|--------------------|--------------------------------------------------------------------|:-----------:|
| `GUID_IFC`         | GUID IFC comprimido de 22 caracteres (el GlobalId del IFC)         | ✓           |
| `color`            | Color aplicado al objeto (`#RRGGBBAA`)                             | ✓           |
| `codigo_revision`  | Código RFI (solo en filas cuyo color esté en la lista de revisión) | ✓           |
| `revisado_deo`     | `TRUE`/`FALSE` según el color                                      | ✓           |
| `View_name`        | Nombre completo de la vista de origen                              | ✓           |
| `modelId`          | Identificador del modelo                                           | —           |
| `objectRuntimeId`  | Identificador en tiempo de ejecución del objeto                    | —           |

> El `GUID_IFC` se obtiene convirtiendo el runtime id al GUID externo con la API y **comprimiéndolo** al formato IFC de 22 caracteres (el mismo que ves en el panel de propiedades como *GUID (IFC)*).

### Lógica del código de revisión

El valor base del código se determina en este orden:

1. **Custom codes ON** y con texto en el cuadro → se usa ese texto.
2. En otro caso, se intenta **extraer del título** de la vista el patrón `RFI_XXXXX`.
3. Si no se puede extraer:
   - **Error codes ON** → se escribe `ERROR (NOMBRE_DE_LA_VISTA)` (o `ERROR (VIEW_NAME_NOT_FOUND)`).
   - **Error codes OFF** → `codigo_revision` queda vacío (con aviso en el log).

> Importante: aunque exista un código base, **solo se escribe en `codigo_revision` cuando el color del objeto está en la lista de colores de revisión**. En el resto de colores la celda queda vacía.

### Convención de colores

Estos colores están configurados como constantes al inicio de `index.html` y puedes adaptarlos a tu propio criterio:

**Colores que activan `codigo_revision`** (`REVISION_CODE_COLORS`):

`#EC00B6FF`, `#AD1457FF`, `#B85AD0FF`, `#DCA1DCFF`, `#FC0D1BFF`, `#D50002FF`, `#E67C73FF`

**Colores que marcan `revisado_deo = TRUE`** (`REVIEWED_DEO_COLORS`):

`#FC0D1BFF`, `#D50002FF`, `#E67C73FF`

> La comparación es exacta sobre el valor `#RRGGBBAA` en mayúsculas. Si tu modelo aplica los colores con un canal alfa distinto a `FF`, ajusta las constantes o modifica la comparación para que ignore el alfa.

### Extended export (varias vistas)

Activa **Extended export** y elige el alcance:

- **Todo el modelo**: recorre todas las vistas del proyecto.
- **Carpeta** (si la API expone carpetas de vistas): solo las de esa carpeta.

La extensión carga cada vista, lee sus objetos coloreados y acumula todo en **un único CSV** (`DEO_extended_export_..._trimble_colores_guid_ifc.csv`). Se permiten GUID duplicados (cada fila indica su `View_name`).

### Importar CSV

En **Herramientas del archivo → Importar colores desde CSV**, pulsa **Importar CSV** y elige un archivo con al menos una columna de **GUID** y otra **`color`**. La extensión:

1. Convierte cada GUID (comprimido o UUID) a runtime id en los modelos cargados.
2. Aplica el color a cada objeto encontrado.
3. **Omite** los GUID que no estén presentes en el modelo activo.

### Selección cercanos

En **Herramientas del modelo**:

1. Selecciona un objeto en el visor.
2. Configura **parámetro**, **condición** (menos / igual / más) y **valor**.
3. Activa el interruptor **Selección cercanos**.

Añade a la selección los objetos en contacto con el seleccionado que cumplan la condición (por ejemplo, tornillos y soldaduras adyacentes a un perfil con *Net volume < 0.1 m³*).

> El contacto es **aproximado mediante cajas envolventes (*bounding boxes*)**, no contacto geométrico exacto.

---

## Limitaciones conocidas

- **Selección cercanos** es una aproximación por *bounding box* y usa la jerarquía del ensamblaje como conjunto de candidatos; puede requerir ajuste según el modelo.
- La **detección de carpetas de vistas** depende de que la API las exponga; si no, solo se ofrece "Todo el modelo".
- **Extended export** se ejecuta en el navegador: para volúmenes muy grandes (miles de vistas) el proceso puede ser largo y el CSV resultante muy pesado.
- La comparación de colores es **exacta** (incluyendo el canal alfa).

---

## Privacidad

La extensión se ejecuta **íntegramente en tu navegador** a través de la Workspace API de Trimble. No envía datos a ningún servidor externo: las exportaciones se descargan localmente y las importaciones se leen localmente.

---

## Licencia

Distribuido bajo licencia **MIT**. Consulta el archivo [LICENSE](LICENSE).
