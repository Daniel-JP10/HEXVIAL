# Resultados de pruebas de HEXVIAL

Las pruebas del frontend corren contra una API y un sitio levantados localmente desde el mismo commit; la prueba de humo de la API corre contra producción.

Corrida única realizada el 16 de noviembre de 2026, entre las 20:33 y las 20:42 (hora de Colombia, UTC-5), sobre una copia exportada del commit de origen `a8908ab` del repositorio privado. Este documento registra solo lo obtenido en esa corrida.

Contexto común:

- Las pruebas del frontend, las de interfaz con navegador y la auditoría de accesibilidad se ejecutaron contra una compilación local del sitio y una instancia local de la API, ambas construidas desde el mismo commit, porque esas pruebas están diseñadas para entorno local.
- La prueba de humo de la API se ejecutó contra la API en producción.
- El código fuente de las pruebas se mantiene en el repositorio privado.

## Resumen

| Prueba | Resultado | Conteo |
| :--- | :--- | :--- |
| Pruebas automáticas de la API | Aprobada | 18 de 18 |
| Prueba de humo contra la API en producción | Aprobada | 10 de 10 endpoints |
| Verificación de valores de control de los reportes | Aprobada | 20 de 20 claves |
| Verificación de tipos del frontend | Aprobada | 0 errores |
| Análisis estático del frontend | Aprobada con advertencias | 0 errores, 60 advertencias |
| Pruebas unitarias y de integración del frontend | Aprobada | 34 de 34 |
| Compilación de producción del frontend | Aprobada con advertencia | 1 advertencia de tamaño |
| Verificación de cifras del sitio contra la API | Primer intento fallido; reintento aprobado | 47 de 47 cifras en ambos intentos |
| Filtro de localidad al alternar el tema | Aprobada | 3 de 3 iteraciones |
| Ponderación dinámica por gravedad | Aprobada | 6 de 6 combinaciones |
| Auditoría automatizada de accesibilidad | Aprobada | 20 de 20 configuraciones |

## 1. Pruebas automáticas de la API

- **Qué valida:** el contrato de la API (documentación OpenAPI en español, formato de errores 404 y 422, cabeceras de caché y que el servicio no cargue librerías de entrenamiento), la política de orígenes permitidos (origen exacto, origen por expresión regular y rechazo de un origen ajeno), una prueba de humo interna y la consistencia de los valores servidos. Esta última incluye que la suma del índice por sector y por zona sea igual a la de la ciudad, que el sector con ranking 1 sea el de mayor efecto, las sumas de las zonas calientes, la consistencia de la serie de mortalidad y su pronóstico, y el percentil 95 de latencia en la resolución de zona.
- **Criterio de aprobación:** todas las pruebas pasan.
- **Contra qué se comparó:** los artefactos analíticos versionados del mismo commit.
- **Resultado:** 18 de 18 aprobadas en 19,02 s. Por grupo: contrato 6, orígenes permitidos 4, humo 1, valores 7. Hubo 2 advertencias de obsolescencia que vienen de dependencias de terceros, no del proyecto.

## 2. Prueba de humo contra la API en producción

- **Qué valida:** que los 10 endpoints esenciales respondan HTTP 200 con la estructura esperada y con valores iguales a los calculados desde los artefactos versionados.
- **Criterio de aprobación:** los 10 endpoints pasan.
- **Contra qué se comparó:** la API en producción, frente a los artefactos del commit `a8908ab`.
- **Resultado:** 10 de 10 aprobados, a las 20:40. Valores comprobados:
  - ventana de siniestros de 2015-01 a 2021-08;
  - 123 sectores H3 de resolución 7 y 3.386 zonas de resolución 9;
  - 21 localidades en el catálogo;
  - 123 polígonos sectoriales;
  - serie de 80 meses para el sector de mayor riesgo;
  - 3 clústeres con 13 hexágonos;
  - suma del índice de la ciudad de 362.235 en 80 meses;
  - 138 meses de mortalidad y 6 meses de pronóstico;
  - 12 periodos de predicción en el corte A;
  - 4 secciones en el resumen de modelos.

  Latencias entre 157 y 991 ms.

## 3. Verificación de valores de control de los reportes

- **Qué valida:** que las cifras publicadas en los reportes de análisis coincidan con los valores de control registrados para la línea base vigente.
- **Criterio de aprobación:** las 20 claves quedan dentro de su tolerancia.
- **Contra qué se comparó:** los reportes de `reportes/` frente al archivo de valores de control del mismo commit. Esta prueba no recalcula los modelos desde los datos crudos, que no están versionados. Lo que verifica es que lo reportado coincide con la línea base.
- **Resultado:** 20 de 20 correctas, por ejemplo:
  - 9.840 filas del panel y 2.747 ceros agregados;
  - razón varianza/media de 44,89 e ICC de 0,874;
  - 5 de 5 pruebas a favor de la binomial negativa;
  - AIC de 60.054,1, alpha de 0,1004 y efecto máximo de 21,03 veces el promedio;
  - 13 hexágonos de alto riesgo, 3 grupos y silueta de 0,453;
  - MAE de Gradient Boosting de 7,801 (corte A) y 9,942 (corte B);
  - MAE de la binomial negativa de 8,156 (corte A) y 15,735 (corte B);
  - MAE de mortalidad de 7,395 con 88 orígenes;
  - MAE del índice de 648,948;
  - pronóstico de 56,8 muertes por mes.

## 4. Verificación de tipos del frontend

- **Qué valida:** que el código del frontend no tenga errores de tipos con la configuración estricta del proyecto.
- **Criterio de aprobación:** 0 errores.
- **Resultado:** 0 errores.

## 5. Análisis estático del frontend

- **Qué valida:** las reglas de estilo y de buenas prácticas del proyecto, incluidas las de accesibilidad en componentes.
- **Criterio de aprobación:** 0 errores.
- **Resultado:** 0 errores y 60 advertencias, todas del tipo "uso explícito de any".

## 6. Pruebas unitarias y de integración del frontend

- **Qué valida:** traductores de parámetros, formato numérico, cliente HTTP (tiempo de espera, reintentos ante 502, 503, 504 y fallas de red, normalización de errores), respuestas simuladas de error (422, 404, 500, caída de red y demora de 5 s), servicios, e integración contra una API real.
- **Criterio de aprobación:** todas las pruebas pasan, y las de integración verifican contra una API activa.
- **Contra qué se comparó:** respuestas simuladas y, en integración, la API local del mismo commit.
- **Resultado:** 6 de 6 archivos y 34 de 34 pruebas aprobadas. En la primera ejecución no había API local activa, y las 5 pruebas de integración terminaron sin verificar nada, aunque figuraron como aprobadas. Se repitieron con la API local activa y las 5 verificaron y aprobaron: metadatos y catálogo, 123 sectores y 3.386 zonas, 3 clústeres, métricas de diagnóstico, y 138 meses de mortalidad con 6 periodos de pronóstico.

## 7. Compilación de producción del frontend

- **Qué valida:** que el frontend compile para producción con verificación estricta de tipos.
- **Criterio de aprobación:** compilación sin errores.
- **Resultado:** compilación correcta en 14,29 s. Hubo una advertencia: el paquete del motor de mapas pesa 1.043,75 kB sin comprimir y 284,45 kB comprimido, por encima del umbral de aviso de 500 kB.

## 8. Verificación de cifras del sitio contra la API

- **Qué valida:** que las cifras que muestran las 5 rutas del sitio coincidan exactamente con las calculadas desde la API, entre ellas los indicadores de Inicio, el diagnóstico y la tabla de modelos, la reconciliación de mortalidad y la referencia cruzada por año, el panel del mapa y los datos de Metodología.
- **Criterio de aprobación:** todas las cifras coinciden y el proceso termina sin errores.
- **Contra qué se comparó:** el sitio compilado localmente frente a la API local del mismo commit.
- **Resultado:**
  - **Primer intento (20:38): fallido.** Las 47 de 47 cifras coincidieron, pero el proceso terminó con error porque no encontró la carpeta donde guarda su informe. Esa carpeta está excluida del control de versiones y por eso no venía en la copia exportada.
  - **Reintento (20:41), con la carpeta creada: aprobado.** 47 de 47 cifras coincidentes y finalización sin errores.
- **Alcance:** esta prueba compara valores, no etiquetas. No detecta, por ejemplo, que una cifra esté rotulada con un periodo que no le corresponde.

## 9. Filtro de localidad al alternar el tema

- **Qué valida:** que en el mapa un filtro de localidad no reaparezca al volver a "Todas" y cambiar de tema, y que se conserven el número de hexágonos, el tema del documento y la paleta de riesgo del tema activo.
- **Criterio de aprobación:** las 3 iteraciones pasan.
- **Resultado:** 3 de 3 aprobadas. Conteos: 123 hexágonos con "Todas", 6 con Kennedy y 16 con Suba. Tras cada cambio de tema se conservaron los 123 hexágonos.

## 10. Ponderación dinámica por gravedad

- **Qué valida:** que el índice que recalcula el cliente, con la fórmula 1 por solo daños, 3 por heridos y 10 por muertos, coincida con la API en cada combinación de categorías. Valida además el primer lugar del ranking en cada combinación, la coherencia del nivel de riesgo entre mapa, leyenda y panel, la nota de que los modelos usan las tres categorías y la columna de índice recalculado en la vista tabular.
- **Criterio de aprobación:** coincidencia en todos los hexágonos y en todas las combinaciones.
- **Resultado:** aprobada.
  - Las 3 categorías activas coinciden en 123 de 123 hexágonos.
  - Las 6 combinaciones parciales coinciden en 123 de 123 hexágonos cada una, con el primer lugar verificado.
  - La nota del panel aparece 2 veces y la columna de índice recalculado está presente.

## 11. Auditoría automatizada de accesibilidad

- **Qué valida:** reglas automáticas de accesibilidad de axe-core con las etiquetas WCAG 2.0 y 2.1, niveles A y AA, junto con los errores de consola y de política de seguridad de contenido.
- **Criterio de aprobación:** 0 violaciones críticas y 0 serias en todas las configuraciones.
- **Contra qué se comparó:** el sitio compilado localmente, en 5 rutas × 2 anchos (1440 y 390 px) × 2 temas, es decir, 20 configuraciones.
- **Resultado:** 20 de 20 aprobadas, con 0 violaciones críticas, 0 serias, 0 moderadas, 0 menores y 0 errores de consola.
- **Alcance:** la revisión automatizada cubre solo una parte de los criterios WCAG. No equivale a una certificación de conformidad.
