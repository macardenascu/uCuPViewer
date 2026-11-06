# Hoja de ruta para completar el sistema

El objetivo es convertir el prototipo demostrable en un sistema reproducible, sin borrar versiones que explican cómo llegó hasta aquí.

| Prioridad | Trabajo | Criterio de aceptación | Riesgo |
| :-- | :-- | :-- | :-- |
| P0 | Documentar pinout, preparación GPIO y montaje exacto | Foto anotada, comando de pinmux, niveles y GND común verificados | Cableado/voltaje |
| P0 | Medir ADC a 100/1.000/10.000 Sa/s | Archivos de medición temporal, error y jitter por canal; 10k sin error 0x17 bajo criterio acordado | Polling y bloqueo |
| P0 | Corregir sincronización y recuperación CS | Sin espera indefinida tras errores; ventana TX lista y prueba de pérdidas | Firmware SPI |
| P0 | Robustecer UART y estados API | Distinguir preset solicitado/reportado; métricas de fallos y recuperación | JSON fragmentado |
| P1 | Servir HTTP directamente desde Milk-V | Nginx/Apache y backend iniciados en SBC; acceso desde otra máquina con IP del SBC | Imagen Linux/USB networking |
| P1 | Verificar refresco 0,25–1 Hz | Registro de intervalos reales de nuevas ventanas, no de repintados | Transferencia lenta |
| P1 | Documentar SPI teórico y medir SCLK | Datasheets de ambas plataformas y ensayo con analizador lógico | Restricciones de hardware |
| P1 | CI y pruebas adicionales | Tests del protocolo, errores, frontend y parser en cada PR | Regressiones |
| P2 | Adquisición TIM trigger + DMA | Dos canales y 64 muestras con jitter caracterizado y tramas compatibles | Reescritura parcial |
| P2 | ≥40 kSa/s (opcional) | Medición repetible, sin pérdida y con señal externa conocida | Ancho de banda y ADC |
| P2 | Release de portafolio | Video con permiso, diagrama, guía limpia, changelog y tabla de mediciones | Transparencia/autoria |

## Política de ramas

- `main`: representación del desarrollo que tiene evidencia, no una mezcla automática de todo lo experimental.
- `docs/portfolio-refresh-2026`: mejora de presentación, matriz, capturas, CI y trazabilidad, sin cambiar firmware.
- Ramas `feature/*`, `firmware/*`, `debug/*`: conservar como historial, decidir cherry-picks con revisión y pruebas.
- No fusionar `feature/unificacion-completa-sistema` de forma indiscriminada: su comparación con `main` incluye archivos `.venv/` versionados además de cambios funcionales. Excluir entornos e investigar el diff antes de reutilizar cambios.

## Comprobaciones previas a publicación

El repositorio contiene `reference/profesor/`, identificado por el propio equipo como material entregado por el docente con permiso de redistribución pendiente de confirmar. No confundirlo con trabajo original ni asumir que la GPL de la raíz resuelve automáticamente los derechos sobre todo contenido tercero.

Conservar las bitácoras de error y pruebas negativas. Usar releases/tags fechados después de verificar qué commit corresponde a la demostración real. Fijar el fork (pin) en el perfil de GitHub desde la página del perfil para destacarlo.

