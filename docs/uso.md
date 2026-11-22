# Uso de MinuteCutter

Cómo cortar un clip, cómo se escriben los tiempos y qué hacer cuando algo falla. Para instalar el panel, ve a [instalacion.md](instalacion.md).

## Antes de cortar

- **Haz un respaldo del `.prproj`.** La herramienta modifica el timeline.
- **Prueba primero en una copia del proyecto** con el rango `0:10` → `0:20`.
- **Evita probar sobre el único original** o sobre proyectos duplicados con el mismo material: pueden resolverse mal.

## Cómo cortar

1. Selecciona el clip en el timeline, o en el proyecto si ya tiene una instancia en el timeline. El panel debe mostrar `Clip: <nombre>  —  Duración: <mm:ss>`.
2. En cada fila, escribe `Inicio` y `Fin` en formato `mm:ss`. Los tiempos son de **fuente**.
3. Usa **+ Agregar fila** para añadir rangos. Usa el botón `x` de una fila para borrarla.
4. Pulsa **Cortar**.
5. Lee el mensaje de estado. Si el corte fue bien, dice `Corte realizado en N pedazo(s) sin mover material posterior`.

## Formato de tiempos

- Formato `mm:ss`, por ejemplo `1:20` o `2:05`. También acepta más segmentos, por ejemplo `1:20:30` = 1 h 20 min 30 s.
- Los tiempos son de **fuente**: se miden sobre el material del clip, no sobre su posición en el timeline.
- `Fin` debe ser mayor que `Inicio` y no puede superar la duración del material fuente.

| Ejemplo | Rangos | Resultado |
|---|---|---|
| Simple | `0:10` → `0:20` | Un pedazo de 10 s (segundos 10 a 20 de la fuente) |
| Varios rangos | `0:10`–`0:20` y `0:40`–`0:55` | Dos pedazos contiguos de 10 s y 15 s, insertados en la posición original |
| Todo el clip | `0:00` → duración total | Un pedazo igual al clip original completo |

## Mensajes del panel

| Situación | Mensaje |
|---|---|
| Clip detectado y listo | `Clip: <nombre>  —  Duración: <mm:ss>` y botón **Cortar** habilitado |
| Sin selección | `Sin clip seleccionado. Elegí un clip en el timeline o en el proyecto.` |
| Clip solo en el proyecto | `Clip de Proyecto detectado; para cortar, primero debe existir en timeline` |
| Corte exitoso | `Corte realizado en N pedazo(s) sin mover material posterior` |

Los mensajes del panel están en español, como la interfaz.

## Solución de problemas

| Síntoma | Causa probable | Solución |
|---|---|---|
| El panel no aparece en Window > Extensions | Doble carpeta `extension\extension` | Revisa la estructura instalada y vuelve a copiar el contenido |
| `Test-Path` devuelve `False` | Copia incompleta o destino equivocado | Repite la copia desde la raíz del proyecto |
| El panel no carga el host | Falta `host\main.jsx` en la instalación | Reinstala y reinicia Premiere |
| `PlayerDebugMode` no surte efecto | Clave bajo otra versión de CSXS | Verifica que esté bajo **`CSXS.11`** (Premiere 2022) |
| `Error: timeout del host` | El corte tardó más de lo esperado | Revisa el timeline; el corte pudo no completarse |
| `Este build corta solo clips a 1x...` | Clip con velocidad distinta de 100 % | Cambia la velocidad a 100 % y vuelve a intentar |
| `No se admiten clips en reversa para cortar` | Clip en reversa | No se admite reversa en esta versión |
| `El rango supera la duracion de la fuente` | Rango fuera del material fuente | Ajusta el rango a la duración real |
| `Hay audio sincronizado no seleccionado...` | Falta seleccionar el audio vinculado | Selecciona video y audio juntos y vuelve a intentar |
| `Este build no expone ProjectItem.createSubClip` | La API de Premiere no está disponible en ese build | El panel aborta sin tocar el timeline; prueba en otro build |
| `...usa Ctrl+Z` | Un paso del corte no se pudo revertir | Pulsa Ctrl+Z en Premiere y revisa el timeline |
| Hueco tras el corte | Comportamiento esperado en modo sin ripple | Une los clips a mano, o usa `v1.0.0` (con ripple) |

## Limitaciones detalladas

- **Plataforma:** Windows, Premiere Pro **2022 (22.x)** y CEP **11**. El manifest declara `[22.0, 22.9]`; no se verifica en otras versiones.
- **Velocidad:** solo clips a **1x (100 %)**. No se admiten velocidades distintas de 1x, ni 0, ni clips en **reversa**.
- **Proyecto (BIN):** sin instancia en el timeline no se puede cortar. El panel lo avisa.
- **Audio sincronizado:** si hay audio sincronizado sin seleccionar en la posición del clip, el corte se rechaza y pide seleccionar video y audio juntos.
- **Sin ripple:** el corte no mueve clips posteriores. Puede quedar un hueco. Si esperabas que el material posterior se corriera, esta versión no lo hace.
- **APIs del host:** el corte depende de APIs de ExtendScript (crear subclips, quitar e insertar clips). Si el build no las expone, el panel aborta con un mensaje claro.
- **Build no oficial:** Premiere Pro es software cerrado. Las APIs pueden variar entre builds; si una no se comporta como se espera, el panel informa el error.

## Respaldos

Antes de cortar, guarda una copia del `.prproj`. Si un corte deja el timeline a medias y no puedes deshacerlo con Ctrl+Z, la copia es tu punto de regreso.
