# Rendimiento SPI y plan de medición

## Métricas distintas que no deben mezclarse

- **Frecuencia de adquisición ADC por canal (Sa/s):** número de muestras de cada canal por segundo durante la ventana.
- **Frecuencia SCLK de SPI (Hz):** cantidad de ciclos de reloj por segundo sobre el enlace.
- **Throughput SPI efectivo (bit/s):** bits de trama / tiempo total cronometrado; incluye pausas de software y preparación.
- **Refresco HTTP/UI (Hz):** frecuencia de presentación de nuevas ventanas, independiente de los otros tres.

La trama activa tiene 264 bytes = **2.112 bits de reloj SPI**, full-duplex. Transporta 64 muestras CH1 y 64 muestras CH2 de 12 bits, empaquetadas en palabras de 16 bits.

```text
throughput_efectivo = 2112 bits / T_transferencia
T_ideal_bus = 2112 / f_SCLK
T_ventana_nominal = 64 / f_s   (64 periodos de adquisición)
rango_temporal_del_gráfico = 63 / f_s   (entre primera y última muestra)
```

Por ejemplo, una pantalla con 2,07 s de transferencia corresponde a ~**1.020 bit/s efectivos**, no a 1.000 muestras por segundo medidas ni a f_SCLK constante. El dashboard redondea el valor observado en su propia sesión.

## Estimación de implementación GPIO actual

En `backend/milkv_uart/milkv_stream.py` hay tres pausas solicitadas de 200 µs por bit. Por tanto:

```text
T_software_min_solicitado = 2112 × 3 × 200 µs = 1,2672 s
throughput_max_ideal_solicitado ≈ 2112 / 1,2672 = 1666,7 bit/s
```

Esto es un **límite ideal derivado del código**, no la frecuencia máxima teórica del periférico SPI de la Milk-V ni una medición osciloscópica. El planificador Linux, acceso sysfs, lectura GPIO, cambio de CS y otros costes reducen el throughput real.

Los registros del repositorio incluyen dos transferencias C de **1,783 y 1,791 s** con ~1.185 y ~1.179 bit/s, y una captura de dashboard de **2,07 s** con ~1.018 bit/s. Esas mediciones pertenecen a pruebas distintas, por lo que no se deben promediar como benchmark único.

## Presets y ventanas nominales

| Preset | Periodo esperado entre pares | 64 periodos | Eje temporal entre 64 puntos |
| :-- | --: | --: | --: |
| 100 Sa/s | 10 ms | 640 ms | 630 ms |
| 1.000 Sa/s | 1 ms | 64 ms | 63 ms |
| 10.000 Sa/s | 100 µs | 6,4 ms | 6,3 ms |

**Advertencia:** en este firmware un flag de TIM2 desencadena dos conversiones ADC secuenciales por polling. La tabla es una predicción de configuración; requiere medición física de los instantes de captura.

## Protocolo de benchmark reproducible

1. Anotar `git rev-parse HEAD` de firmware, backend y frontend, identificación de placas y alimentación.
2. Medir 30 a 100 transferencias completas SC264 para cada preset, incluyendo tiempos y secuencias; repetir con carga del SBC y sin ella.
3. Con analizador lógico capturar `CS/SCLK/MISO/MOSI`, extraer f_SCLK efectiva y estimar jitter, pausas y duración CS-bajo.
4. Registrar CRC/estado, capturas consecutivas, errores UART y pérdidas de secuencia.
5. Usar generador de señales y una referencia externa para determinar tasa ADC y error de frecuencia; no deducirla del eje X del dashboard.
6. Medir 20 ciclos de actualización gráfica, separar intervalos de UI de intervalos de captura y declarar el porcentaje de nuevas ventanas.

## Datasheets

La comparación de máxima tasa teórica **queda abierta** hasta identificar modelo físico, controlador SPI, modo de funcionamiento, tensión, revisión de datasheet y límites compatibles en ambos extremos. No usar el máximo nominal de un periférico que el programa actual no utiliza como rendimiento alcanzado del enlace GPIO.

## Plantilla de resultados

```text
Fecha y zona horaria:
Placas y revisiones:
SHA firmware / backend:
Cableado y niveles:
Preset solicitado / reportado:
Instrumentos y tasa de captura:
Nº de ventanas:
Tiempo transferencia min/mediana/max:
Frecuencia SCLK medida:
Periodo ADC por canal (media, desv., mín., máx.):
CRC inválidos / estados error / pérdidas:
Intervalo HTTP/UI (min/mediana/max):
Archivos crudos y hashes:
Conclusión y limitaciones:
```

