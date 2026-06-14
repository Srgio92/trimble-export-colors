# trimble-export-colors

Extensión HTML/JavaScript para Trimble Connect 3D Viewer.

- Repositorio local: `C:\Users\sergi\Documents\trimble-export-colors`
- Repositorio GitHub: https://github.com/Srgio92/trimble-export-colors
- GitHub Pages: https://srgio92.github.io/trimble-export-colors/
- Archivo principal: `index.html` (HTML + CSS + JS en un solo archivo; no asumir módulos separados).

---

## Eficiencia de tokens

Trabaja con eficiencia de tokens. No cargues más contexto del necesario. No hagas auditorías completas salvo que el usuario lo pida expresamente o la gravedad del fallo lo justifique.

### Lectura mínima del repositorio

- Usa `rg` (ripgrep / Grep tool) para localizar ids, clases, constantes, listeners y funciones antes de abrir bloques grandes.
- No uses `cat index.html` completo salvo que sea imprescindible.
- Lee solo los bloques relacionados con la tarea.
- Si el cambio afecta a un botón, revisa solo su HTML, CSS, listener y función asociada.
- Si el cambio afecta a una herramienta, revisa solo esa herramienta y sus helpers directos.
- Para revisar cambios recientes, usa `git diff`, no el archivo entero.
- Para revisar una versión GPT, usa `rg` y `git diff --no-index` sobre archivos concretos; no abras toda la carpeta.
- No leas `gpt_code/`, `old_code/` ni documentación si la tarea no lo requiere.

### Edición mínima

- No reescribas bloques completos si puedes aplicar un parche mínimo.
- No hagas refactors grandes sin permiso.
- No reformatees el archivo completo.
- No cambies nombres de funciones, ids o estructuras si no es necesario.
- Si el cambio toca más de 3 zonas funcionales, detente y pregunta antes de aplicar.
- Prioriza cambios pequeños, localizados y verificables.
- Si hay varias soluciones, elige la que modifique menos código siempre que no sacrifique estabilidad.

### Herramientas y coste

- Agrupa búsquedas relacionadas en pocos comandos `rg`.
- Evita ejecutar comandos exploratorios repetidos sin hipótesis clara.
- No lances subagentes ni flujos multiagente salvo que el usuario haya pedido `ultracode` o una auditoría profunda.
- No generes informes largos si basta una respuesta breve.
- No pegues bloques grandes de código en la respuesta final salvo petición expresa.
- Ejecuta verificaciones estáticas necesarias, pero evita repetirlas si no ha habido cambios desde la última verificación.
- Antes de subir el nivel de esfuerzo, intenta acotar el problema con búsquedas y diff.

### Diagnóstico incremental

- Primero identifica el bloque afectado.
- Después revisa dependencias directas.
- Solo si el problema no se explica con esas zonas, amplía el análisis.
- Si faltan datos, pregunta antes de hacer una auditoría completa.
- Si una tarea parece cara en tokens, propón dividirla en fases:
  1. diagnóstico limitado;
  2. parche mínimo;
  3. verificación;
  4. ampliación solo si falla.

---

## Clasificación de tareas y nivel recomendado

Solo usar **Opus**. No recomendar ni usar Sonnet ni Fable en este proyecto.

Los niveles disponibles se tratan como niveles de dedicación/esfuerzo para Opus:

- `baja` = `low`
- `media` = `medium`
- `alta` = `high`
- `extra` = `xhigh`
- `máxima` = `max`
- `ultracode` = modo de Claude Code, no un nivel estándar de API; equivale a `xhigh` con permiso para flujos multiagente/subagentes.

Regla general:
- No asumir que más nivel siempre es mejor.
- Elegir el nivel mínimo que mantenga fiabilidad.
- Subir de nivel solo si la tarea lo justifica por riesgo, ambigüedad, tamaño, interacción entre herramientas o necesidad de exploración.
- Si el usuario pide ahorrar créditos, priorizar `baja` o `media` y dividir la tarea en fases.
- Si el usuario pide máxima fiabilidad o reparación crítica, usar `extra` o `máxima` según el caso.

### Tabla de decisión

1. **Consulta, redacción de prompt, comandos Git o explicación breve** — Opus, nivel `baja`.
   Ejemplos: redactar prompt para Claude Code, preparar comandos PowerShell, explicar un flujo, revisar una instrucción corta.

2. **UI simple** — Opus, nivel `baja`.
   Ejemplos: textos, márgenes, alineación, iconos, colores CSS, border-radius, hover, espaciados.

3. **UI con comportamiento localizado** — Opus, nivel `media`.
   Ejemplos: botón con listener, popover simple, slider, input dinámico, pequeña interacción de modal, ajuste de una zona concreta.

4. **Código funcional localizado** — Opus, nivel `media` o `alta`.
   Usar `media` si el bloque está claro y el riesgo es bajo.
   Usar `alta` si hay asincronía, estado compartido, `localStorage`, listeners múltiples, cachés o interacción con el visor.

5. **Lógica crítica Trimble** — Opus, nivel `alta` o `extra`.
   Ejemplos: colores Trimble/Tekla/IFC, selección de objetos, API Trimble, etiquetas 3D, Quantity Surveyor, Configurar vista, export/import CSV, exportación masiva, ZIP, cachés globales.
   Usar `alta` para cambios acotados.
   Usar `extra` si el fallo es ambiguo, afecta a varias funciones o requiere búsqueda/depuración con herramientas.

6. **Depuración profunda o regresión difícil** — Opus, nivel `extra`.
   Ejemplos: errores que aparecen solo en Trimble, estados inconsistentes, bugs intermitentes, interacción entre varias herramientas protegidas, análisis de logs/debug largos.

7. **Auditoría completa, reparación estructural o fallo crítico** — Opus, nivel `máxima`.
   Usar solo cuando:
   - `extra` no sea suficiente;
   - el cambio afecte a muchas zonas;
   - haya riesgo alto de romper herramientas protegidas;
   - se necesite la máxima capacidad aunque consuma más créditos.

8. **Ultracode** — Opus, modo `ultracode`.
   Usar solo con permiso explícito del usuario para:
   - auditoría/reparación multiárea larga;
   - refactorización compleja autorizada;
   - preparación final de versión estable con varias comprobaciones;
   - tareas donde se justifique usar subagentes o exploración amplia.
   No usar para UI, cambios localizados, prompts, comandos Git ni depuración intermedia.

### Regla antes de modificar archivos

Antes de editar, evalúa:
- alcance del cambio;
- número de zonas funcionales;
- riesgo para herramientas protegidas;
- necesidad real de herramientas/subagentes;
- coste estimado de tokens;
- si puede resolverse con parche mínimo.

Si la tarea requiere un nivel superior al que el usuario está usando o al que ha pedido, detente antes de modificar archivos y di:

> "Esta tarea debería hacerse con Opus y nivel [baja/media/alta/extra/máxima/ultracode]. ¿Cambio de nivel antes de continuar?"

Si el usuario ya ha indicado el nivel, respétalo salvo que sea claramente insuficiente o excesivo. Si es excesivo para una tarea simple, puedes sugerir bajar nivel para ahorrar créditos.

### Revisión de documentación Anthropic

La selección de nivel debe basarse en la documentación oficial actual de Anthropic sobre `effort` cuando esté disponible:
- `low` prioriza velocidad y coste, con reducción de capacidad.
- `medium` busca equilibrio entre coste y rendimiento.
- `high` es el comportamiento por defecto de Opus y sirve para razonamiento/coding difícil.
- `xhigh` está orientado a coding/agentes complejos y trabajo largo con más uso de tokens.
- `max` se reserva para máxima capacidad, con coste alto y posibles ganancias marginales respecto a `xhigh`.
- `ultracode` en Claude Code debe tratarse como modo especial, no como nivel de API.

Si la documentación o la UI de Claude cambia, aplicar la regla conservadora:
usar el nivel mínimo que permita resolver la tarea con seguridad y preguntar antes de escalar.

---

## Regla obligatoria de versión

- Siempre que modifiques código, incrementa `SCRIPT_VERSION` en `index.html`.
- Si solo analizas y no modificas código, no cambies versión.
- Si modificas `index.html`, la versión debe subir aunque el cambio sea pequeño.
- Antes de finalizar, confirma la versión resultante.

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
