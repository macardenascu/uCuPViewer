# Matriz de cumplimiento y verificación

**Referencia:** enunciado académico provisto por el equipo. **Fecha de corte:** 2026-11-06.

Esta matriz no suplanta [la matriz de planificación original](../05_matriz_verificacion_inicial.txt), ni reetiqueta sin pruebas sus resultados. Los estados aquí distinguen implementación, evidencia disponible y criterios faltantes.

**Leyenda:** VERIFICADO = evidencia directa para el alcance concreto indicado; PARCIAL = hay ejecución observada pero no cumple o no valida todo el requisito; PENDIENTE = sin verificación suficiente; NO DEMOSTRADO = no se acredita el objetivo.

| ID | Requisito | Estado | Evidencia / limitación |
| :-- | :-- | :-- | :-- |
| R01 | Sistema microprocesador + microcontrolador | VERIFICADO | STM32 y Milk-V intercambiaron tramas; [log SPI](../debug/milkv_validacion_post_systick.md) |
| R02 | Dos señales analógicas | PARCIAL | Vectores separados en interfaz y trama; faltan dos señales conocidas, generadas y medidas independientemente |
| R03 | ≥64 muestras por ventana | VERIFICADO | Trama SC264, parser y tres transferencias físicas de 64 pares |
| R04 | Periodicidad de muestreo constante | PENDIENTE | Timer polling; faltan mediciones de intervalos y jitter |
| R05 | Preset 100 Sa/s | PARCIAL | Código y selector; sin cadencia física medida |
| R06 | Preset 1.000 Sa/s | PARCIAL | Tramas válidas con código 2; `rate_hz` de API es **nominal**, no medido |
| R07 | Preset 10.000 Sa/s | PARCIAL | Seleccionable, pero hay registro de fallo `0x17` (periodo perdido); no demuestra operación estable |
| R08 | Cambio del preset por SPI | PARCIAL | Primer byte MOSI permite seleccionar preset de la siguiente captura; sin prueba integral de las tres tasas y sin ACK independiente |
| R09 | Comunicación vía SPI | PARCIAL | Enlace SPI modo 0 con GPIO/bit-banging validado en sesiones limitadas; no hardware SPI de alta tasa |
| R10 | SBC sirve dashboard por HTTP usando Apache/Nginx | PENDIENTE | HTTP disponible en PC/VM; Nginx en Milk-V no está demostrado en montaje probado |
| R11 | Refresco entre 0,25 y 1 Hz | PARCIAL | Bucle nominal cada 4 s, UI consulta 4 s; falta registro de tiempos de actualización visible y regularidad |
| R12 | SPI máximo teórico por datasheets de MCU y SBC | PENDIENTE | Faltan fuentes de hoja de datos y límite verificable para cada interfaz física utilizada |
| R13 | Velocidad SPI verificada experimentalmente | PARCIAL | Reportes de ~1.179–1.185 bit/s y ~1.018 bit/s en escenas distintas; no equivale a máximo experimental |
| R14 | Pruebas unitarias / integración | PARCIAL | 12 pruebas offline reportadas y varias transferencias reales; sin ensayos integrales de repetibilidad |
| R15 | Repositorio, instalación, diagramas, aportes, IA, matriz y video | PARCIAL | Elementos existen en rutas diferentes; declaraciones, capturas, instalación SBC y video requieren revisión humana |
| R16 | Bonificación ≥40.000 Sa/s | NO DEMOSTRADO | Sin implementación medida bajo ese objetivo |

## Criterios de ensayo pendientes

1. **Exactitud temporal**: inyectar señal conocida y usar analizador lógico/osciloscopio para medir el tiempo entre pares de muestras; reportar media, desviación estándar, mínimo, máximo y error frente a 10 ms / 1 ms / 100 µs. La tolerancia admisible debe acordarse antes de asignar PASS/FAIL.
2. **Independencia de canales**: aplicar dos fuentes distinguibles dentro del rango seguro, cambiar solo una entrada, verificar que el otro canal no replica el cambio y documentar referencia eléctrica.
3. **SPI**: medir SCLK con analizador lógico, repetir ≥100 ventanas por preset, contar errores de cabecera, CRC, estado, pérdidas y secuencias; conservar el archivo bruto.
4. **HTTP SBC**: confirmar IP del SBC, servicio Apache/Nginx y acceso desde otro dispositivo; guardar `curl -I`, página y datos reales; no sustituir con un túnel del PC.
5. **Refresco**: tomar al menos 20 timestamps de actualizaciones de gráfica, calcular intervalos reales y distinguir ventanas nuevas frente a relecturas de la última captura.
6. **40 kSa/s**: solo reportar si hay ventanas completas, medición temporal y estabilidad demostrada; calcular aparte el ancho de banda SPI requerido y tiempos de servicio.

Un resultado negativo es un resultado. La evidencia de fallo a 10 kSa/s debe conservarse.

## Cómo registrar una prueba nueva

Usar [plantilla de mediciones](measurements.md), indicando fecha local, versión Git del firmware/backend/frontend, placas realmente usadas, instrumento y configuración, señal de entrada, procedimiento, series de datos y resultado observado. Las capturas del dashboard no sustituyen archivos con timestamps.

