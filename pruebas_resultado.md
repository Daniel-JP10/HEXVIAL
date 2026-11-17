# Resultados de pruebas de HEXVIAL

Las pruebas del frontend corren contra una API y un sitio levantados localmente desde el código del repositorio privado; la prueba de humo de la API y la revisión del sitio corren contra producción.

Corrida del 16 de noviembre de 2026 para la versión v1.2.0, que corrige cifras y textos del sitio, de la API y de los reportes. Este documento registra solo lo obtenido en esa corrida.

Contexto común:

- Las pruebas de interfaz con navegador, que son la verificación de cifras del sitio contra la API, el filtro de localidad al alternar el tema, la ponderación dinámica por gravedad y la auditoría de accesibilidad, corrieron con Playwright sobre el código del commit `4d9b7eb` del repositorio privado, padre de `c59c7b8`, contra una compilación local del sitio y una instancia local de la API construidas desde ese código. `c59c7b8` solo modifica README.md, de modo que el código de interfaz probado es idéntico al de `c59c7b8`.
- Los commits posteriores no cambian el comportamiento de la interfaz: `38315bf` retira Playwright del proyecto, `bc0572f` solo agrega anotaciones de tipos en httpAdapters.ts y `5690210` fija la versión de statsmodels en las dependencias del pipeline.
- Sobre `5690210` se repitieron las pruebas automáticas de la API, la verificación de valores de control, la verificación de tipos, el análisis estático, las pruebas unitarias y de integración y la compilación del frontend.
- El árbol fusionado en `main`, commit `6efec63`, es idéntico al de `5690210`. Sobre `6efec63` se repitieron las pruebas automáticas de la API, la verificación de valores de control, la compilación y las pruebas del frontend.
- La prueba de humo de la API y la revisión del sitio se ejecutaron contra producción, con la API y el sitio desplegados desde `6efec63`.
- El código fuente de las pruebas se mantiene en el repositorio privado.

## Resumen

| Prueba | Resultado | Conteo |
| :--- | :--- | :--- |
| Pruebas automáticas de la API | Aprobada | 22 de 22 |
| Prueba de humo contra la API en producción | Aprobada | 10 de 10 endpoints |
| Verificación de valores de control de los reportes | Aprobada | 20 de 20 claves |
| Verificación de tipos del frontend | Aprobada | 0 errores |
| Análisis estático del frontend | Aprobada con advertencias | 0 errores, 60 advertencias |
| Pruebas unitarias y de integración del frontend | Aprobada | 35 de 35 |
| Compilación de producción del frontend | Aprobada con advertencia | 1 advertencia de tamaño |
| Verificación de cifras del sitio contra la API | Aprobada | 120 de 120 cifras |
| Filtro de localidad al alternar el tema | Aprobada | 3 de 3 iteraciones |
| Ponderación dinámica por gravedad | Aprobada | 6 de 6 combinaciones |
| Auditoría automatizada de accesibilidad | Aprobada | 20 de 20 configuraciones |
| Revisión del sitio en producción | Aprobada | 5 rutas en 2 temas |

## 1. Pruebas automáticas de la API

- **Qué valida:** el contrato de la API, con documentación OpenAPI en español, formato de errores 404 y 422, cabeceras de caché y que el servicio no cargue librerías de entrenamiento; la política de orígenes permitidos, con origen exacto, origen por expresión regular y rechazo de un origen ajeno; una prueba de humo interna; la consistencia de los valores servidos; las correcciones de esta versión, que son los commits de código y de artefactos en el estado del servicio, la advertencia sobre el número de clústeres y el diagnóstico de sobredispersión frente a su reporte; y que cada valor p de la sección 7 de regresion_pandemia.md coincida con su tabla. La consistencia de valores incluye que la suma del índice por sector y por zona sea igual a la de la ciudad, que el sector con ranking 1 sea el de mayor efecto, las sumas de las zonas calientes, la consistencia de la serie de mortalidad y su pronóstico, y el percentil 95 de latencia en la resolución de zona.
- **Criterio de aprobación:** todas las pruebas pasan.
- **Contra qué se comparó:** los artefactos analíticos versionados del mismo commit.
- **Resultado:** 22 de 22 aprobadas en `5690210` y de nuevo en `6efec63`, en unos 8,6 s. Por grupo: contrato 6, orígenes permitidos 4, humo 1, valores 7, corrección de cifras 3 y reporte de pandemia 1. Hubo 1 advertencia de obsolescencia que viene de una dependencia de terceros, no del proyecto.

## 2. Prueba de humo contra la API en producción

- **Qué valida:** que los 10 endpoints esenciales respondan HTTP 200 con la estructura esperada y con valores iguales a los calculados desde los artefactos versionados.
- **Criterio de aprobación:** los 10 endpoints pasan.
- **Contra qué se comparó:** la API en producción, desplegada desde `6efec63`, frente a los artefactos de ese commit.
- **Resultado:** 10 de 10 aprobados. Valores comprobados:
  - estado disponible, versión 1.1.0, commit del código `6efec63` y commit de los artefactos `415d433`;
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

  Latencias entre 136 y 787 ms.

## 3. Verificación de valores de control de los reportes

- **Qué valida:** que las cifras publicadas en los reportes de análisis coincidan con los valores de control registrados para la línea base vigente.
- **Criterio de aprobación:** las 20 claves quedan dentro de su tolerancia.
- **Contra qué se comparó:** los reportes de `reportes/` frente al archivo de valores de control del mismo commit. Esta prueba no recalcula los modelos desde los datos crudos, que no están versionados. Lo que verifica es que lo reportado coincide con la línea base.
- **Resultado:** 20 de 20 correctas en `5690210` y de nuevo en `6efec63`, por ejemplo:
  - 9.840 filas del panel y 2.747 ceros agregados;
  - razón varianza/media de 44,89 e ICC de 0,874;
  - 5 de 5 pruebas a favor de la binomial negativa;
  - AIC de 60.054,1, alpha de 0,1004 y efecto máximo de 21,03 veces el promedio;
  - 13 hexágonos de alto riesgo, 3 grupos y silueta de 0,453;
  - MAE de Gradient Boosting de 7,801 en el corte A y 9,942 en el corte B;
  - MAE de la binomial negativa de 8,156 en el corte A y 15,735 en el corte B;
  - MAE de mortalidad de 7,395 con 88 orígenes;
  - MAE del índice de 648,948;
  - pronóstico de 56,8 muertes por mes.

## 4. Verificación de tipos del frontend

- **Qué valida:** que el código del frontend no tenga errores de tipos con la configuración estricta del proyecto.
- **Criterio de aprobación:** 0 errores.
- **Resultado:** 0 errores en `5690210`.

## 5. Análisis estático del frontend

- **Qué valida:** las reglas de estilo y de buenas prácticas del proyecto, incluidas las de accesibilidad en componentes.
- **Criterio de aprobación:** 0 errores, y no más advertencias que antes de la corrección.
- **Resultado:** 0 errores y 60 advertencias en `5690210`, las mismas 60 que antes de la corrección: 59 de uso explícito de any y 1 de dependencias de un hook de React.

## 6. Pruebas unitarias y de integración del frontend

- **Qué valida:** traductores de parámetros, formato numérico, cliente HTTP con tiempo de espera, reintentos ante 502, 503, 504 y fallas de red y normalización de errores, respuestas simuladas de error, que son 422, 404, 500, caída de red y demora de 5 s, servicios, el filtro de localidades, e integración contra una API real.
- **Criterio de aprobación:** todas las pruebas pasan, y las de integración verifican contra una API activa.
- **Contra qué se comparó:** respuestas simuladas y, en integración, la API local del mismo commit.
- **Resultado:** 7 de 7 archivos y 35 de 35 pruebas aprobadas. En las ejecuciones sobre `5690210` y sobre `6efec63` sin API local activa, las 5 pruebas de integración terminaron sin verificar nada, aunque figuraron como aprobadas. Se repitieron sobre `6efec63` con la API local activa y las 5 verificaron y aprobaron.

## 7. Compilación de producción del frontend

- **Qué valida:** que el frontend compile para producción con verificación estricta de tipos.
- **Criterio de aprobación:** compilación sin errores.
- **Resultado:** compilación correcta en `5690210` y en `6efec63`, en unos 6 s. Hubo una advertencia: el paquete del motor de mapas pesa 1.043,75 kB sin comprimir y 284,45 kB comprimido, por encima del umbral de aviso de 500 kB.

## 8. Verificación de cifras del sitio contra la API

- **Qué valida:** que las cifras que muestran las 5 rutas del sitio coincidan exactamente con las calculadas desde la API, entre ellas los indicadores de Inicio, el diagnóstico y la tabla de modelos, la reconciliación de mortalidad y la referencia cruzada por año, el panel del mapa y los datos de Metodología.
- **Criterio de aprobación:** todas las cifras coinciden y el proceso termina sin errores.
- **Contra qué se comparó:** el sitio compilado localmente frente a la API local, ambos desde `4d9b7eb`.
- **Resultado:** 120 de 120 cifras coincidentes y finalización sin errores. Por ruta: Inicio 22, Mapa 29, Modelo 30, Mortalidad 25 y Metodología 14.
- **Alcance:** esta prueba compara valores, no etiquetas. No detecta, por ejemplo, que una cifra esté rotulada con un periodo que no le corresponde.

## 9. Filtro de localidad al alternar el tema

- **Qué valida:** que en el mapa un filtro de localidad no reaparezca al volver a "Todas" y cambiar de tema, y que se conserven el número de hexágonos, el tema del documento y la paleta de riesgo del tema activo.
- **Criterio de aprobación:** las 3 iteraciones pasan.
- **Contra qué se comparó:** el sitio compilado localmente desde `4d9b7eb`.
- **Resultado:** 3 de 3 aprobadas, con las localidades Kennedy, Suba y Kennedy.

## 10. Ponderación dinámica por gravedad

- **Qué valida:** que el índice que recalcula el cliente, con la fórmula 1 por solo daños, 3 por heridos y 10 por muertos, coincida con la API en cada combinación de categorías. Valida además el primer lugar del ranking en cada combinación, la coherencia del nivel de riesgo entre mapa, leyenda y panel, la nota de que los modelos usan las tres categorías y la columna de índice recalculado en la vista tabular.
- **Criterio de aprobación:** coincidencia en todos los hexágonos y en todas las combinaciones.
- **Contra qué se comparó:** el sitio compilado localmente frente a la API local, ambos desde `4d9b7eb`.
- **Resultado:** aprobada. Las 3 categorías activas y las 6 combinaciones parciales coinciden en los 123 hexágonos, con el primer lugar verificado en cada combinación.

## 11. Auditoría automatizada de accesibilidad

- **Qué valida:** reglas automáticas de accesibilidad de axe-core con las etiquetas WCAG 2.0 y 2.1, niveles A y AA, junto con los errores de consola y de política de seguridad de contenido.
- **Criterio de aprobación:** 0 violaciones críticas y 0 serias en todas las configuraciones.
- **Contra qué se comparó:** el sitio compilado localmente desde `4d9b7eb`, en 5 rutas × 2 anchos, 1440 y 390 px, × 2 temas, es decir, 20 configuraciones.
- **Resultado:** 20 de 20 aprobadas, con 0 violaciones críticas, 0 serias, 0 moderadas, 0 menores y 0 errores de consola.
- **Alcance:** la revisión automatizada cubre solo una parte de los criterios WCAG. No equivale a una certificación de conformidad.

## 12. Revisión del sitio en producción

- **Qué valida:** que el sitio en producción muestre las cifras y los textos corregidos en esta versión.
- **Criterio de aprobación:** todos los textos corregidos presentes, ninguno de los anteriores, ningún emoji y 0 errores de consola.
- **Contra qué se comparó:** el sitio en producción, desplegado desde `6efec63`, revisado con Chrome DevTools en las 5 rutas, a 1440 px y en los dos temas.
- **Resultado:** aprobada en las 10 combinaciones de ruta y tema:
  - 198.392 siniestros analizados y el aviso de los 754 registros excluidos en Inicio y en Metodología;
  - "efecto fijo por sector" y alpha del modelo base de 0,1004 en Modelo;
  - las 5 pruebas que votan, con "5 de 5": razón varianza/media, Phi de Pearson, Dean y Lawless, Cameron y Trivedi y Vuong;
  - "Los Mártires" y "Antonio Nariño" en la lista de sectores prioritarios del mapa;
  - el texto nuevo de "Efecto por Hexágono" y el de la silueta en Metodología;
  - ningún emoji y 0 errores de consola. El mapa registró avisos del estilo del mapa base, que no son errores.
