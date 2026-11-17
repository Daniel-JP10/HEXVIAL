# Catálogo de Esquemas de Artefactos de la API (HEXVIAL)

> Documento generado automáticamente por `src/variables/exportar_artefactos_api.py`. 
> Describe la totalidad de artefactos persistidos en `datos/procesados/api/` para consumo 
> directo por parte de servicios backend, sin requerir recálculos ni acceso a datos crudos.

## `catalogo_localidades.parquet`

**Filas:** 21 | **Columnas:** 4

**Descripción:** Catálogo maestro normalizado de localidades del Distrito Capital para consumo de filtros y navegación en la API. Contiene la clave canónica en mayúsculas ASCII sin tildes ni eñes (localidad_clave), el nombre oficial para visualización con tildes tomado de la fuente de Medicina Legal (nombre_mostrar), la cantidad de hexágonos de resolución 7 que tienen dicha localidad como moda (n_hexagonos_sector) y la bandera de disponibilidad en la serie histórica de mortalidad (tiene_mortalidad). Incluye la categoría SIN DATO.

| Columna | Tipo de Dato | No Nulos | Descripción / Valores de Muestra |
| :--- | :---: | :---: | :--- |
| `localidad_clave` | `object` | 21 / 21 | Ejemplo: `ANTONIO NARINO` |
| `nombre_mostrar` | `object` | 21 / 21 | Ejemplo: `Antonio Nariño` |
| `n_hexagonos_sector` | `int64` | 21 / 21 | Ejemplo: `1` |
| `tiene_mortalidad` | `bool` | 21 / 21 | Ejemplo: `True` |

## `riesgo_sector_mes.parquet`

**Filas:** 9,840 | **Columnas:** 7

**Descripción:** Panel completo balanceado (con ceros reales agregados) de siniestralidad vial en resolución sectorial gruesa (H3 resolución 7). Cada fila representa una celda y un mes de observación continua (2015-01 a 2021-08).

| Columna | Tipo de Dato | No Nulos | Descripción / Valores de Muestra |
| :--- | :---: | :---: | :--- |
| `h3` | `object` | 9,840 / 9,840 | Ejemplo: `8766e002dffffff` |
| `periodo` | `object` | 9,840 / 9,840 | Ejemplo: `2015-01` |
| `solo_danos` | `int64` | 9,840 / 9,840 | Ejemplo: `0` |
| `con_heridos` | `int64` | 9,840 / 9,840 | Ejemplo: `0` |
| `con_muertos` | `int64` | 9,840 / 9,840 | Ejemplo: `0` |
| `total_siniestros` | `int64` | 9,840 / 9,840 | Ejemplo: `0` |
| `indice` | `int64` | 9,840 / 9,840 | Ejemplo: `0` |

## `riesgo_zona_mes.parquet`

**Filas:** 104,735 | **Columnas:** 7

**Descripción:** Panel de siniestralidad vial en resolución barrial fina (H3 resolución 9), acotado exclusivamente a las celdas con actividad histórica de siniestros.

| Columna | Tipo de Dato | No Nulos | Descripción / Valores de Muestra |
| :--- | :---: | :---: | :--- |
| `h3` | `object` | 104,735 / 104,735 | Ejemplo: `8966e082073ffff` |
| `periodo` | `object` | 104,735 / 104,735 | Ejemplo: `2015-01` |
| `solo_danos` | `int64` | 104,735 / 104,735 | Ejemplo: `0` |
| `con_heridos` | `int64` | 104,735 / 104,735 | Ejemplo: `0` |
| `con_muertos` | `int64` | 104,735 / 104,735 | Ejemplo: `1` |
| `total_siniestros` | `int64` | 104,735 / 104,735 | Ejemplo: `1` |
| `indice` | `int64` | 104,735 / 104,735 | Ejemplo: `10` |

## `atributos_sector.parquet`

**Filas:** 123 | **Columnas:** 10

**Descripción:** Atributos espaciales consolidados por hexágono sectorial (res 7): coordenadas de centroide, localidad modal (aproximación geoespacial), efecto relativo respecto al promedio distrital derivado del modelo econométrico binomial negativo (M0), índice mensual esperado, posición ordinal de riesgo (ranking 1 al mayor efecto), indicador de alto riesgo (top 10%) e identificador de clúster espacial DBSCAN.

| Columna | Tipo de Dato | No Nulos | Descripción / Valores de Muestra |
| :--- | :---: | :---: | :--- |
| `h3` | `object` | 123 / 123 | Ejemplo: `8766e0933ffffff` |
| `lat` | `float64` | 123 / 123 | Ejemplo: `4.629` |
| `lon` | `float64` | 123 / 123 | Ejemplo: `-74.161` |
| `localidad_moda` | `object` | 123 / 123 | Ejemplo: `KENNEDY` |
| `localidad_clave` | `object` | 123 / 123 | Ejemplo: `KENNEDY` |
| `efecto_vs_promedio` | `float64` | 123 / 123 | Ejemplo: `21.028` |
| `indice_esperado_mes` | `float64` | 123 / 123 | Ejemplo: `140.643` |
| `ranking_riesgo` | `int32` | 123 / 123 | Ejemplo: `1` |
| `en_alto_riesgo` | `bool` | 123 / 123 | Ejemplo: `True` |
| `cluster` | `Int64` | 13 / 123 | Ejemplo: `0` |

## `atributos_zona.parquet`

**Filas:** 3,386 | **Columnas:** 6

**Descripción:** Atributos espaciales consolidados por celda barrial activa (res 9): coordenadas de centroide, celda padre sectorial (res 7) y localidad modal histórica.

| Columna | Tipo de Dato | No Nulos | Descripción / Valores de Muestra |
| :--- | :---: | :---: | :--- |
| `h3` | `object` | 3,386 / 3,386 | Ejemplo: `8966e002d0fffff` |
| `h3_padre_sector` | `object` | 3,386 / 3,386 | Ejemplo: `8766e002dffffff` |
| `lat` | `float64` | 3,386 / 3,386 | Ejemplo: `4.086` |
| `lon` | `float64` | 3,386 / 3,386 | Ejemplo: `-74.229` |
| `localidad_moda` | `object` | 3,386 / 3,386 | Ejemplo: `FONTIBON` |
| `localidad_clave` | `object` | 3,386 / 3,386 | Ejemplo: `FONTIBON` |

## `zonas_calientes.parquet`

**Filas:** 3 | **Columnas:** 9

**Descripción:** Resumen ejecutivo de los clústeres espaciales de concentración crítica de riesgo detectados con DBSCAN métrica Haversine sobre el percentil 90 acumulado.

| Columna | Tipo de Dato | No Nulos | Descripción / Valores de Muestra |
| :--- | :---: | :---: | :--- |
| `cluster` | `int64` | 3 / 3 | Ejemplo: `0` |
| `n_hexagonos` | `int64` | 3 / 3 | Ejemplo: `9` |
| `indice_total` | `int64` | 3 / 3 | Ejemplo: `80217` |
| `lat` | `float64` | 3 / 3 | Ejemplo: `4.604` |
| `lon` | `float64` | 3 / 3 | Ejemplo: `-74.119` |
| `localidades_frecuentes` | `object` | 3 / 3 | Ejemplo: `KENNEDY, PUENTE ARANDA, LOS MARTIRES` |
| `eps_km` | `float64` | 3 / 3 | Ejemplo: `3.000` |
| `silueta` | `float64` | 3 / 3 | Ejemplo: `0.453` |
| `percentil_corte` | `float64` | 3 / 3 | Ejemplo: `90.000` |

## `serie_indice_ciudad.parquet`

**Filas:** 80 | **Columnas:** 4

**Descripción:** Serie temporal mensual agregada para el total de Bogotá D.C. del índice ponderado de gravedad, conteo total de siniestros y siniestros con víctimas mortales.

| Columna | Tipo de Dato | No Nulos | Descripción / Valores de Muestra |
| :--- | :---: | :---: | :--- |
| `periodo` | `object` | 80 / 80 | Ejemplo: `2015-01` |
| `indice` | `int64` | 80 / 80 | Ejemplo: `3511` |
| `total_siniestros` | `int64` | 80 / 80 | Ejemplo: `1913` |
| `con_muertos` | `int64` | 80 / 80 | Ejemplo: `32` |

## `serie_mortalidad.parquet`

**Filas:** 2,263 | **Columnas:** 5

**Descripción:** Serie mensual de víctimas fatales en siniestros de tránsito según registros oficiales del Instituto Nacional de Medicina Legal y Ciencias Forenses (SaluData 2015-2026). Contiene desglose a nivel ciudad y por localidad (registros sin localidad asignada se mantienen únicamente en el total ciudad).

| Columna | Tipo de Dato | No Nulos | Descripción / Valores de Muestra |
| :--- | :---: | :---: | :--- |
| `nivel` | `object` | 2,263 / 2,263 | Ejemplo: `ciudad` |
| `localidad` | `object` | 2,125 / 2,263 | Ejemplo: `ANTONIO NARIÑO` |
| `localidad_clave` | `object` | 2,125 / 2,263 | Ejemplo: `ANTONIO NARINO` |
| `periodo` | `object` | 2,263 / 2,263 | Ejemplo: `2015-01` |
| `muertes` | `int64` | 2,263 / 2,263 | Ejemplo: `33` |

## `pronostico_mortalidad.parquet`

**Filas:** 6 | **Columnas:** 7

**Descripción:** Pronóstico univariado final a 6 meses de mortalidad vial en Bogotá D.C., utilizando el mejor modelo seleccionado en validación por origen móvil (Promedio 12 meses) e intervalo de predicción empírico del 80% calibrado sobre errores fuera de muestra.

| Columna | Tipo de Dato | No Nulos | Descripción / Valores de Muestra |
| :--- | :---: | :---: | :--- |
| `periodo` | `object` | 6 / 6 | Ejemplo: `2026-07` |
| `prediccion` | `float64` | 6 / 6 | Ejemplo: `56.800` |
| `inferior` | `float64` | 6 / 6 | Ejemplo: `45.400` |
| `superior` | `float64` | 6 / 6 | Ejemplo: `69.800` |
| `modelo` | `object` | 6 / 6 | Ejemplo: `Promedio 12 meses` |
| `nivel_intervalo` | `int64` | 6 / 6 | Ejemplo: `80` |
| `metodo_intervalo` | `object` | 6 / 6 | Ejemplo: `empírico (percentiles 10-90)` |

## `predicciones_supervisado_api.parquet`

**Filas:** 23,616 | **Columnas:** 8

**Descripción:** Predicciones a un mes hacia adelante de los modelos de Machine Learning (HistGradientBoosting, RandomForest) y baselines estadísticos bajo formato largo normalizado, evaluadas en particiones out-of-time (corte A: 2019, corte B: 2020-2021).

| Columna | Tipo de Dato | No Nulos | Descripción / Valores de Muestra |
| :--- | :---: | :---: | :--- |
| `h3` | `object` | 23,616 / 23,616 | Ejemplo: `8766e002dffffff` |
| `periodo` | `object` | 23,616 / 23,616 | Ejemplo: `2019-01` |
| `corte` | `object` | 23,616 / 23,616 | Ejemplo: `A, prueba 2019` |
| `corte_codigo` | `object` | 23,616 / 23,616 | Ejemplo: `A` |
| `modelo` | `object` | 23,616 / 23,616 | Ejemplo: `Binomial negativa, efecto por hexágono` |
| `modelo_codigo` | `object` | 23,616 / 23,616 | Ejemplo: `binomial_negativa` |
| `real` | `float64` | 23,616 / 23,616 | Ejemplo: `0.000` |
| `prediccion` | `float64` | 23,616 / 23,616 | Ejemplo: `0.000` |

## `resumen_modelos.json`

**Estructura:** JSON (4 bloques raíz)

**Descripción:** Estructura JSON con los parámetros y resultados consolidados de modelado: diagnóstico de sobredispersión, comparación de especificaciones econométricas (M0, M1, M2 con errores de dos vías y bootstrap en bloque), métricas de aprendizaje supervisado y análisis de sensibilidad de pesos.

**Claves de primer nivel:**

- `diagnostico`: `dict` (5)
- `regresion`: `dict` (3)
- `supervisado`: `dict` (2)
- `sensibilidad`: `list` (5)

## `metadatos.json`

**Estructura:** JSON (8 bloques raíz)

**Descripción:** Metadatos de gobernanza del pipeline: ventanas temporales derivadas de los datos crudos, ponderaciones de gravedad oficiales, resoluciones de malla H3, número de celdas, mes parcial excluido, trazabilidad de versión Git y catálogo de advertencias metodológicas.

**Claves de primer nivel:**

- `ventana_datos`: `dict` (2)
- `pesos_gravedad`: `dict` (3)
- `resoluciones_h3`: `dict` (2)
- `numero_hexagonos`: `dict` (2)
- `mes_parcial_excluido`: `dict` (2)
- `git`: `dict` (2)
- `fecha_generacion`: `str` (26)
- `advertencias`: `list` (4)

## Consideraciones Metodológicas y de Uso

1. **Localidad Modal (`localidad_moda`):** Asignada como la moda estadística de siniestros en meses completos. No debe interpretarse como frontera administrativa exacta.
2. **Panel Balanceado:** `riesgo_sector_mes.parquet` garantiza el producto cartesiano exacto (123 hexágonos × 80 periodos = 9,840 filas), permitiendo indexación matricial sin huecos.
3. **Inmutabilidad y Versionado:** Todos los archivos parquet y JSON en este directorio son rastreados por git y coinciden formalmente con los reportes analíticos de `reportes/`.

