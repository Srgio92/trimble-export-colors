# trimble-export-colors

Extensión HTML/JavaScript para Trimble Connect 3D Viewer.

- Repositorio local: `C:\Users\sergi\Documents\trimble-export-colors`
- Repositorio GitHub: https://github.com/Srgio92/trimble-export-colors
- GitHub Pages: https://srgio92.github.io/trimble-export-colors/
- Archivo principal: `index.html` (HTML + CSS + JS en un solo archivo; no asumir módulos separados).

---

## Eficiencia de tokens

Trabaja con eficiencia de tokens. No cargues más contexto del necesario. No hagas auditorías completas salvo que el usuario lo pida expresamente.

- Usa `rg` (ripgrep / Grep tool) para localizar ids, clases y funciones antes de abrir bloques grandes.
- No uses `cat index.html` completo salvo que sea imprescindible.
- Lee solo los bloques relacionados con la tarea.
- Si el cambio afecta a un botón, revisa solo su HTML, CSS, listener y función asociada.
- Si el cambio afecta a una herramienta, revisa solo esa herramienta y sus helpers directos.
- Para revisar cambios recientes, usa `git diff`, no el archivo entero.
- No reescribas bloques completos si puedes aplicar un parche mínimo.
- No hagas refactors grandes sin permiso.
- Si el cambio toca más de 3 zonas funcionales, detente y pregunta antes de aplicar.

---

## Clasificación de tareas y modelo recomendado

**Modelo por defecto: Sonnet 4.6, uso bajo.** No usar Fable como modelo habitual. Fable solo en dos casos: (a) tokens cerca de reiniciarse y hay que cerrar una tarea crítica; (b) consolidar versión estable final antes de un commit + push importante.

1. **UI simple** — Sonnet 4.6, uso bajo.
   Ejemplos: textos, márgenes, alineación, iconos, colores CSS, border-radius, hover, espaciados.

2. **UI con comportamiento** — Sonnet 4.6, uso medio/alto.
   Ejemplos: botones con listener, popovers, sliders, inputs dinámicos, selección/deselección, sticky/floating.

3. **Código funcional localizado** — Sonnet 4.6, uso alto.
   Ejemplos: funciones concretas, listeners, cachés locales, parsing, pequeñas reparaciones JS.

4. **Lógica crítica Trimble** — Opus 4.8, uso alto/máximo.
   Ejemplos: colores Trimble/Tekla/IFC, selección de objetos, API Trimble, etiquetas 3D, Quantity Surveyor, Configurar vista, export/import CSV, exportación masiva, ZIP, cachés globales.

5. **Auditoría completa o depuración profunda** — Opus 4.8, uso máximo.

6. **Versión estable final o cierre antes de reinicio de tokens** — Fable, uso máximo.
   Solo usar si: queda poco para reinicio de tokens; hay que consolidar una versión estable; se va a hacer commit + push relevante; la tarea requiere máxima fiabilidad.

Si la tarea requiere un modelo superior al actual, detente antes de modificar archivos y di:
> "Esta tarea debería hacerse con [modelo] y uso [nivel]. ¿Cambio de modelo antes de continuar?"

No recomendar Fable para UI, cambios localizados o depuración intermedia.

---

## Regla de versionado

El proyecto usa versiones con formato `vX.Y.Z`, donde:

- `X` = versiones con incorporación de nuevas funciones, nuevas herramientas o cambios funcionales relevantes.
- `Y` = versiones publicadas mediante push que corrigen errores, estabilizan funciones existentes o incorporan ajustes funcionales sin crear una herramienta nueva.
- `Z` = versiones internas de trabajo, pruebas o correcciones locales sin push.

### Reglas obligatorias

1. Siempre que se modifique `index.html`, debe actualizarse `SCRIPT_VERSION`.

2. Las versiones `Z` son versiones internas:
   - pueden existir en local;
   - pueden existir en `gpt_code/`;
   - pueden usarse para pruebas previas;
   - no deben publicarse como versión final mediante push;
   - no deben generar entrada en `CHANGELOG.md`.

3. Si una versión interna `vX.Y.Z` va a publicarse, debe convertirse antes en versión publicable:
   - si incorpora nuevas funciones o herramientas, incrementar `X` y resetear `Y = 0`, `Z = 0`;
   - si corrige errores o estabiliza funciones existentes, incrementar `Y` y resetear `Z = 0`.

   Ejemplos:
   - De `v8.10.43` a push por corrección de errores → `v8.11.0`.
   - De `v8.10.43` a push por nueva función relevante → `v9.0.0`.
   - De `v8.10.43` a nueva prueba interna sin push → `v8.10.44`.

4. `CHANGELOG.md` solo se actualiza cuando la versión publicada modifica `X` o `Y`.
   - Sí actualizar changelog: `v8.10.43` → `v8.11.0`.
   - Sí actualizar changelog: `v8.10.43` → `v9.0.0`.
   - No actualizar changelog: `v8.10.43` → `v8.10.44`.

5. `old_code/` solo debe guardar copias de versiones publicadas o preparadas para publicación.
   - No crear backup histórico para versiones internas `Z` salvo petición expresa.
   - Antes de commit + push, crear copia exacta del `index.html` final en: `old_code/index_vX.Y.0.html`
   - La versión del archivo de backup debe coincidir exactamente con `SCRIPT_VERSION`.

6. La carpeta `gpt_code/` puede contener versiones internas con incremento `Z`.
   - Al ejecutar el flujo `consulta nueva versión GPT`, se puede revisar la versión GPT más alta.
   - Si esa versión solo incrementa `Z`, debe tratarse como versión de trabajo.
   - Si el usuario pide push de esa versión, reclasificarla antes como incremento `X` o `Y`, según el contenido real de los cambios.

7. Antes de decidir si una versión publicable sube `X` o `Y`, analizar la intención del diff:
   - Nueva función, nueva herramienta, nuevo flujo principal o capacidad nueva → subir `X`.
   - Corrección, ajuste, reparación, estabilización, mejora de UI o mejora interna de función existente → subir `Y`.
   - Prueba local, ajuste previo, parche pendiente de validar o versión generada por ChatGPT no publicada → subir `Z`.

8. Si hay duda entre `X` e `Y`, usar `Y` salvo que exista una función nueva clara.

9. Si hay duda sobre publicar o no una versión `Z`, detenerse y preguntar antes de hacer commit + push.

---

## Regla de documentación

- `index.html` sí se modifica siempre que el cambio de código lo requiera.
- `CHANGELOG.md` solo se modifica cuando el trabajo vaya a terminar con commit + push al servidor de GitHub.
- Si no hay push, no actualices `CHANGELOG.md`.
- `README.md`, `CONTRIBUTING.md`, `manifest.json`, `assets` u otros documentos **no** deben modificarse salvo que el usuario lo pida expresamente.
- Antes de tocar cualquier archivo distinto de `index.html` y `CHANGELOG.md`, esperar confirmación explícita.
- No actualices documentación "por si acaso".

---

## Reglas de Git

Antes de modificar:
```
git status
git diff --check
git diff --stat
```

Después de modificar:
```
git diff --check
git diff --stat
git status
```

Si hay commit y push:
- No hacer force push.
- No cambiar de rama automáticamente.
- Si la rama actual es `main`: `git push origin main`.
- Si no es `main`: preguntar o usar `git push origin HEAD` solo si está justificado.
- No añadir archivos que no se hayan modificado.
- No incluir README, CONTRIBUTING, manifest ni assets salvo petición expresa.
- CHANGELOG solo si habrá push.

---

## Reglas de UI

- No añadir estados visuales permanentes que ensucien la interfaz.
- No añadir textos largos de ayuda salvo que el usuario los pida.
- No añadir botones test, legacy o debug visibles.
- Todos los botones pulsables deben tener respuesta visual coherente (hover azul + contracción al pulsar).
- Mantener hover azul en botones pulsables.
- Mantener esquinas redondeadas moderadas.
- Mantener estilo sobrio y compacto.
- Si se pide un icono concreto, integrarlo como SVG inline si es lo más estable.
- No cambiar la apariencia general de herramientas no relacionadas.

---

## Reglas del botón global de actualización

- Priorizar un único botón global para "Actualizar datos del visor".
- El botón debe ser sticky/floating y mantenerse disponible con scroll.
- No debe tener textos de estado visibles.
- Solo puede animarse mientras trabaja.
- Debe evitar clicks concurrentes.
- Debe registrar errores en debug/recordError, no ensuciar la UI.
- Mantener botones locales solo si son realmente necesarios: Actualizar vistas, Debug, Cancelar (mientras haya proceso activo).
- No mantener refrescos redundantes de colores/modelos/parámetros si quedan cubiertos por el botón global.

---

## Reglas de caché/refresco

- No recalcular todo si no hace falta.
- Datos a dividir: modelos IFC cargados; colores detectables; parámetros; vistas; miembros/usuarios; selección; presentación/vista activa.
- Si cambia la vista o los modelos cargados, invalidar solo lo afectado.
- Si cambia selección, no recalcular colores/modelos/parámetros.
- Si una herramienta necesita datos no cargados, cargarlos bajo demanda.

---

## Reglas de colores

- No convertir colores desconocidos en blanco falso.
- Todo color válido debe normalizarse a `#RRGGBBAA`.
- Soportar: `#RGB`, `#RGBA`, `#RRGGBB`, `#RRGGBBAA`, `rgb()`, `rgba()`, objetos `{r,g,b,a}`, objetos `{red,green,blue,alpha}`, arrays `[r,g,b]`, arrays `[r,g,b,a]`, valores 0–1, valores 0–255, alfa/transparencia.
- Selector por color y Excluir por color deben comparar por color normalizado, no por color visual CSS.
- Si la API de Trimble no expone un color Tekla/IFC nativo, indicarlo como limitación técnica y registrarlo en debug.
- No afirmar que un color funciona si no se puede verificar.

---

## Herramientas protegidas

No romper estas herramientas al hacer cambios:

- Excluir de selección (por color, por Name, por IFC/modelo).
- Selector de items (por Name, por parámetro/valor, por color).
- Selección cercanos.
- Selección ocultos / Ver ocultos.
- Mostrar parámetros / Etiquetas 3D.
- Inspector de selección.
- Quantity Surveyor.
- Configurar vista / Compartir con chips/autocompletado.
- Exportación CSV / Importación CSV.
- Exportación masiva (ZIP).
- Debug.

---

## Reglas de respuesta

Después de cada tarea responder de forma breve con este formato:

```
Versión:
Archivos modificados:
Cambios:
Verificación:
Commit/push:
Pendiente de probar en Trimble:
```

- No explicar el código paso a paso salvo que el usuario lo pida.
- No pegar bloques grandes de código salvo que el usuario lo pida.
- No hacer informes largos para cambios pequeños.

Si no se puede probar algo porque requiere Trimble abierto:
- Hacer verificación estática.
- Indicar "pendiente de prueba manual en Trimble".
- Dar checklist corto de prueba.
- No afirmar funcionamiento real si no se ha probado.

---

## Restricciones heredadas

- No reescribir el proyecto desde cero.
- No reformatear todo el archivo.
- No modificar funciones que ya funcionan salvo necesidad justificada.
- Trabajar por cambios pequeños y verificables.
- Antes de editar, identificar función o bloque afectado.
- No hacer commits sin confirmación explícita del usuario.
- No suponer comportamiento de Trimble Connect si no está visible en el código.
- Marcar incertidumbres en vez de inventar.

---

## Flujo: Consulta nueva versión GPT

**Frases de activación** (ejecutar este flujo automáticamente al detectar cualquiera de ellas, o cualquier instrucción equivalente que haga referencia a revisar una versión GPT desde `gpt_code/`):

- `consulta nueva version gpt`
- `consulta nueva versión gpt`
- `consulta gpt version`
- `consulta gpt versión`
- `revisa nueva version gpt`
- `revisa nueva versión gpt`

**Regla principal:** La carpeta `gpt_code/` es una bandeja de entrada de versiones o fragmentos generados por ChatGPT. Nunca copiar automáticamente un archivo de `gpt_code/` sobre `index.html`. Siempre localizar, comparar, evaluar y aplicar solo los cambios válidos mediante parche controlado.

### 1. Comandos iniciales obligatorios

Antes de analizar o modificar, ejecutar:

```bash
git status
git diff --check
git diff --stat
```

### 2. Selección de versión GPT

Localizar versiones disponibles con:

```bash
rg "SCRIPT_VERSION|v[0-9]+\.[0-9]+\.[0-9]+" gpt_code
```

- Si el usuario indica una versión concreta (p. ej. `v8.10.43`), usar exclusivamente esa.
- Si no indica versión, seleccionar automáticamente la versión semánticamente más alta.
- Ignorar versiones anteriores salvo que el usuario las mencione expresamente.
- Si hay varios archivos con la misma versión más alta, detenerse y mostrar la lista antes de modificar nada.
- Si no se encuentra ninguna versión GPT válida, detenerse e indicarlo.

### 3. Validación del archivo GPT seleccionado

1. Verificar que contiene una declaración clara: `const SCRIPT_VERSION = "vX.Y.Z";`
2. Verificar que esa versión es superior a la declarada en el `index.html` del repositorio.
3. Si la versión GPT es igual o inferior, no aplicar sin confirmación explícita del usuario.

### 4. Comparación

- No sustituir el archivo completo sin evaluar el diff.
- Identificar qué bloques cambia: HTML, CSS, JS, listeners, funciones, constantes o textos.
- Evaluar si el cambio es coherente con la petición del usuario.
- Descartar cambios colaterales no justificados.
- No aceptar reformateos masivos, cambios de saltos de línea globales, cambios de indentación global ni reordenaciones no necesarias.
- Si el cambio afecta a más de 3 zonas funcionales, detenerse y resumir riesgos antes de aplicar.

### 5. Herramientas protegidas — verificar antes de aplicar

Si el diff toca alguna de estas herramientas sin relación clara con la petición, no incorporar ese cambio:

- Excluir de selección · Selector de items · Selección cercanos · Selección ocultos / Ver ocultos
- Mostrar parámetros / Etiquetas 3D · Inspector de selección · Quantity Surveyor
- Configurar vista / Compartir con chips/autocompletado
- Exportación CSV / Importación CSV · Exportación masiva ZIP · Debug

### 6. Aplicación

- Aplicar al `index.html` real solo los cambios necesarios.
- La versión final de `index.html` debe coincidir con la versión GPT incorporada.
- Confirmar que `SCRIPT_VERSION` queda actualizado.
- No modificar `README.md`, `CONTRIBUTING.md`, `manifest.json`, `assets` ni otros archivos salvo petición expresa.
- No modificar `CHANGELOG.md` salvo que el trabajo vaya a terminar con commit + push.

### 7. Carpeta `old_code/`

La carpeta `old_code/` es el histórico de versiones publicadas. Solo crear copia cuando el usuario pida commit + push o indique expresamente que la versión se va a publicar:

```
old_code/index_vX.Y.Z.html
```

La copia debe coincidir exactamente con el `index.html` que se va a commitear. No crear copias de versiones descartadas.

### 8. Carpeta `gpt_code/` — regla de commit

La carpeta `gpt_code/` no se commitea por defecto. Solo sirve como entrada de revisión. No incluir archivos de `gpt_code/` en commits salvo orden expresa del usuario.

### 9. Verificación posterior

```bash
git diff --check
git diff --stat
git status
```

### 10. Commit y push (solo si el usuario lo pide expresamente)

- No hacer force push. No cambiar de rama automáticamente.
- Rama `main`: `git push origin main`.
- Incluir en el commit únicamente: `index.html`, `old_code/index_vX.Y.Z.html`, `CHANGELOG.md` (solo si procede), y otros archivos solo si el usuario lo ha pedido expresamente.

### 11. Respuesta obligatoria tras ejecutar el flujo

```
Versión detectada en repositorio:
Versión GPT consultada:
Archivo GPT usado:
Archivos modificados:
Cambios incorporados:
Cambios descartados:
Verificación:
Commit/push:
Pendiente de probar en Trimble:
```
