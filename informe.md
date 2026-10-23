# Análisis estático de `raze.dll`

## Estado y alcance

REA abrió `C:\Users\Usuario\Downloads\Rz2\Mimiv3\Dlls\raze.dll` como PE x86 usando Ghidra 12.1.4. SHA-256 observado: `dfa8632e85c89c318577ef619f20bae2f4db5136042d680ee4dae4109234691a`. El análisis fue estático; no se ejecutó ni inyectó la DLL y el archivo original no se modificó.

La importación de Ghidra y el inventario inicial de cadenas/imports finalizaron. La investigación de funciones no se completó: las consultas de descompilación y referencias quedaron activas durante varios minutos y se interrumpieron. Por tanto, no se confirma ninguna función de glow por comportamiento ni se asignan los sitios de referencia a funciones. No se guardó un proyecto Ghidra persistente: la sesión REA era temporal y se cerró sin snapshot (`close_binary` devolvió `result: null`).

## Evidencia relacionada con glow

Se encontraron estas cadenas en la imagen analizada:

| Dirección | Cadena observada | Alcance de la evidencia |
|---|---|---|
| `0x102830d9` | `Enable_glow` | Etiqueta de configuración relacionada con glow. |
| `0x10283146` | `bKeyEnableGlow` | Nombre de opción/variable; no prueba por sí solo el manejo de tecla. |
| `0x10196f30` | `Enable glow all` | Texto de interfaz/configuración. |
| `0x10285069` | `bGlowSurvivor` | Nombre relacionado con glow de supervivientes. |
| `0x1028503e` | `fColorGlowSurvivor` | Nombre relacionado con color de glow. |
| `0x10284071` | `Glow colors by health status` | Etiqueta de color por estado de salud. |
| `0x10284826` | `Glow y chams` | Etiqueta de menú. |
| `0x102853c8` | `dev/glow_color` | Nombre de material/textura encontrado como cadena. |

Ghidra registró referencias de datos hacia `bKeyEnableGlow` desde `0x1000b56f`, `0x1000b586` y `0x1010ecbb`; hacia `Enable glow all` desde `0x10116974`. Son referencias estáticas observadas. No se confirmó que esos sitios sean parte de funciones concretas ni que ejecuten la activación del glow.

## Colores, visibilidad y jugadores

También se observaron cadenas `Glow colors by health status` (`0x10284071`), `Survivors Color` (`0x10285564`), `Infected Color` (`0x102855de`), `Glow team Color` (`0x1028559a`), `fColorGlowSurvivor` (`0x1028503e`), `bGlowSurvivor` (`0x10285069`) y `bNoDepthPlayers` (`0x10284384`). Son nombres/textos en el binario; no confirman por sí mismos cálculo de color, visibilidad u oclusión.

## Hooks y DirectX

Se encontraron `Left 4 Dead 2 - Direct3D 9` (`0x1028cc99`), `D3DXCreateTextureFromFileInMemoryEx` (`0x10294cfc`) y `d3dx9_43.dll` (`0x102952cf`). En un bloque de script embebido se listan nombres de hooks `PostScreenEffects`, `EndScene`, `DrawModelExecute`, `FrameStageNotify` y `Paint`. La presencia de esos textos no demuestra que la DLL instale los hooks ni permite identificar sus funciones nativas.

## Limitaciones y error observado

El listado de nombres completo excedió el presupuesto de respuesta MCP. Error literal: `resource_constraint` — `The operation completed, but its complete MCP response exceeds the stdio response budget. The analysis result was not truncated.` REA recomendó consultar evidencia retenida o exportar resultados completos.

Las consultas de dossier de funciones quedaron pendientes durante varios minutos y se interrumpieron; no se obtuvo error específico del proveedor para esas consultas. La búsqueda de cadenas cubre datos definidos por Ghidra, no garantiza que todas las cadenas presentes se hayan reconocido. Las referencias estáticas no prueban acceso en tiempo de ejecución. No hay funciones confirmadas con dirección de entrada y pseudocódigo en este informe.
