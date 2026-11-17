# API RESTful HEXVIAL - Siniestralidad Vial en Bogotá D.C.

Servicio web analítico de alto rendimiento desarrollado con **FastAPI** para la consulta geoespacial y temporal de los artefactos generados por el pipeline de ciencia de datos del proyecto HEXVIAL.

---

## 1. Arquitectura y Principios de Diseño

- **Cero recálculo estadístico o econométrico:** La API no ejecuta modelos ni remuestreos en tiempo de ejecución. Sirve exclusivamente artefactos precomputados (`.parquet` y `.json`) ubicados en `datos/procesados/api/`.
- **Carga inmutable en memoria:** Los datos se leen una sola vez al arranque (`lifespan`), midiendo y reportando el tiempo de inicialización (~0.15s) y la memoria RSS (~190 MB).
- **Geometrías H3 cacheadas:** Las coordenadas poligonales de las celdas H3 se precalculan al arrancar en orden GeoJSON canónico `[longitud, latitud]`.
- **Compresión y Caché HTTP:** GZip habilitado para cargas útiles superiores a 500 bytes y cabecera `Cache-Control: public, max-age=3600` para optimizar el consumo desde aplicaciones web frontend.
- **Normalización lingüística:** Respuestas, esquemas OpenAPI, códigos y mensajes de error 100% en español.

---

## 2. Variables de Entorno

| Variable | Valor por Defecto | Descripción |
| :--- | :--- | :--- |
| `DIRECTORIO_ARTEFACTOS` | `datos/procesados/api` | Ruta al directorio donde residen los artefactos `.parquet` y `.json`. |
| `ORIGENES_PERMITIDOS` | `http://localhost:5173` | Lista separada por comas de orígenes CORS permitidos para consultas GET. |
| `HOST` | `0.0.0.0` | Dirección IP de enlace del servidor uvicorn. |
| `PUERTO` | `8000` | Puerto TCP de escucha del servidor. |

---

## 3. Instalación de Dependencias

El entorno de ejecución de la API es ligero y no depende de librerías de entrenamiento estadístico (`scikit-learn`, `statsmodels` ni `prophet`).

El código de este módulo se mantiene en un repositorio privado.

---

## 4. Ejecución del Servidor

El código de este módulo se mantiene en un repositorio privado.

---

## 5. Ejecución de la Suite de Pruebas

La API incluye pruebas automáticas de contrato, consistencia matemática, integridad referencial y latencia:

El código de este módulo se mantiene en un repositorio privado.

Consulte el archivo `CONTRATO.md` para ver la tabla detallada de endpoints, parámetros y respuestas recortadas de ejemplo.
