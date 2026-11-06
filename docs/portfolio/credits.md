# Contribuciones, atribución y uso de IA

## Procedencia

Este proyecto se desarrolló en equipo y este repositorio es un **fork** del trabajo alojado en [anhenaobe/uCuPViewer](https://github.com/anhenaobe/uCuPViewer). La existencia del fork no otorga autoría individual sobre cada archivo ni debe usarse para sugerir que todas las partes fueron creadas por la persona que lo mantiene.

Se conserva la lista de responsabilidades inicialmente acordadas en [`docs/02_roles_responsabilidades.txt`](../02_roles_responsabilidades.txt) y las guías específicas en [`docs/por_integrante/`](../por_integrante/). Son asignaciones, **no certificaciones automáticas de aportes realmente completados**. La contribución efectiva debe verificarse mediante commits, PR, revisiones, experimentos y registros de cada participante.

Roles documentados en el proyecto original:

| Integrante | Área de trabajo inicialmente asignada |
| :-- | :-- |
| Andrew | Coordinación, integración y firmware STM32 |
| Valeria | Montaje, alimentación y entradas analógicas |
| Checho | Diseño y ejecución de pruebas, evidencias |
| Mateo | Dashboard y frontend |
| Pipe | Backend Linux, protocolo y comunicación |
| Ferxxo | Infraestructura del SBC y apoyo al SPI |

El fork de macardenascu se publica para portafolio, manteniendo atribución de equipo y vínculo al proyecto matriz. No se declara aquí una lista de aportes individuales verificados que no esté en el historial.

## Material externo y licencias

El directorio [`reference/profesor/`](../../reference/profesor/) se conserva como material de referencia atribuido al docente, tal como consta en [`ORIGEN.txt`](../../reference/profesor/ORIGEN.txt). Allí se señala que antes de redistribuir en público se debe confirmar el permiso del docente. Es un **pendiente de publicación**, no un permiso concedido.

La raíz contiene una licencia [GPL-3.0](../../LICENSE). Las obligaciones y derechos de materiales externos deben evaluarse por separado. No se cambia la licencia ni se inventan permisos.

## IA y verificación

El proyecto tiene un [registro histórico de aportes de IA](../07_aportes_ia.txt), incluido el uso de ChatGPT y Codex para planificación y documentación. Las interacciones requeridas por el docente deben conservarse por sus responsables, con fecha, herramienta/modelo, tarea, archivos generados o modificados y cómo fueron comprobados.

**Refinamiento posterior a entrega, 2026-11-06:** se usó ChatGPT (GPT-6) como apoyo en la revisión de archivos del fork, lectura de documentación y pruebas del Drive, redacción de la portada/diagramas/matriz y preparación de un flujo CI de tests offline. Verificación: lectura cruzada de código de adquisición, parser y servidor, logs ya versionados, estructura de ramas y visualización de tres imágenes y un video. **No se ejecutaron pruebas físicas nuevas ni se midió ninguna tasa ADC.** La revisión humana de esta documentación y de la atribución individual sigue siendo necesaria. El contenido completo de la interacción queda en el chat de trabajo del mantenedor y no se publica automáticamente sin una exportación explícita.

La asistencia automática no sustituye contribuciones humanas ni otorga licencia sobre material externo.

