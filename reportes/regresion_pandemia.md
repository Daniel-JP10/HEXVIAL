# Modelo de Regresión GLM NB2: Evaluación de Indicadores de Pandemia e Inferencia Robusta

Evaluación del impacto de indicadores de pandemia en la tendencia temporal y el ranking espacial de riesgo relativo por celda H3, complementado con un esquema comparativo de inferencia bajo dependencia temporal común: errores agrupados por hexágono, de dos vías por hexágono y mes, y bootstrap por bloques temporales corregido.


## 1. Parámetros Metodológicos de Pandemia

Nota: Las siguientes fechas corresponden a supuestos de trabajo metodológicos del equipo de investigación y no representan delimitaciones normativas oficiales.

- Inicio de pandemia: `2020-03`.
- Fase cierre: `2020-03` a `2020-08`.
- Fase reapertura: `2020-09` a último mes completo observado.

## 2. Ajuste, Criterios de Información y Convergencia de los Modelos

| modelo   |   parametros |   log_verosimilitud |     aic |     bic |     alpha | optimizador   | convergio   |
|:---------|-------------:|--------------------:|--------:|--------:|----------:|:--------------|:------------|
| M0       |          127 |            -29900.1 | 60054.1 | 60967.8 | 0.100351  | bfgs          | True        |
| M1       |          128 |            -29587   | 59429.9 | 60350.8 | 0.0835826 | bfgs          | True        |
| M2       |          129 |            -29327.6 | 58913.3 | 59841.3 | 0.0736386 | bfgs          | True        |


## 3. Coeficientes Temporales y de Pandemia con Errores Estándar Originales

| modelo   | variable        |   coeficiente |   error_std |      p_valor |   exp_coef |
|:---------|:----------------|--------------:|------------:|-------------:|-----------:|
| M0       | mes_sin         |   -0.0485936  | 0.00618362  | 3.88907e-15  |   0.952568 |
| M0       | mes_cos         |   -0.00310643 | 0.00624906  | 0.619116     |   0.996898 |
| M0       | t               |   -0.00102304 | 0.000195169 | 1.5899e-07   |   0.998977 |
| M1       | mes_sin         |   -0.0400148  | 0.00577186  | 4.12767e-12  |   0.960775 |
| M1       | mes_cos         |   -0.0223758  | 0.00587508  | 0.000139755  |   0.977873 |
| M1       | t               |    0.00372654 | 0.000258214 | 3.25571e-47  |   1.00373  |
| M1       | pandemia        |   -0.379112   | 0.0145701   | 2.95312e-149 |   0.684469 |
| M2       | mes_sin         |   -0.0358497  | 0.00550963  | 7.68076e-11  |   0.964785 |
| M2       | mes_cos         |   -0.0453421  | 0.00570286  | 1.85376e-15  |   0.955671 |
| M2       | t               |    0.00313778 | 0.000247321 | 6.97656e-37  |   1.00314  |
| M2       | fase_cierre     |   -0.674715   | 0.0184331   | 2.51418e-293 |   0.509302 |
| M2       | fase_reapertura |   -0.23336    | 0.0153515   | 3.47946e-52  |   0.791869 |


### Verificación del Signo del Coeficiente de Tendencia $t$

El coeficiente de t cambia de signo entre M0 y M1: pasa de negativo en M0, -0.001023 con p=1.590e-07 y p_rob=0.0115, a positivo en M1, 0.003727 con p=3.256e-47 y p_rob=6.863e-24. En M2 se mantiene positivo: 0.003138 con p=6.977e-37 y p_rob=4.877e-18.


## 4. Coeficientes con Errores Estándar Robustos Agrupados por Hexágono, Cluster-Robust 1-Vía

Estimación con matriz de covarianza robusta agrupada por identificador de celda hexagonal H3, con `cov_type='cluster'` y `groups=h3`, empleando el mismo estimador para M0, M1 y M2:

| modelo   | variable        |   coeficiente |   error_std_robusto |   p_valor_robusto |   exp_coef |
|:---------|:----------------|--------------:|--------------------:|------------------:|-----------:|
| M0       | mes_sin         |   -0.0485936  |         0.00607327  |      1.23185e-15  |   0.952568 |
| M0       | mes_cos         |   -0.00310643 |         0.00578522  |      0.591296     |   0.996898 |
| M0       | t               |   -0.00102304 |         0.000404868 |      0.0115087    |   0.998977 |
| M1       | mes_sin         |   -0.0400148  |         0.00591762  |      1.36125e-11  |   0.960775 |
| M1       | mes_cos         |   -0.0223758  |         0.00553856  |      5.34492e-05  |   0.977873 |
| M1       | t               |    0.00372654 |         0.000369745 |      6.86342e-24  |   1.00373  |
| M1       | pandemia        |   -0.379112   |         0.0248796   |      1.98279e-52  |   0.684469 |
| M2       | mes_sin         |   -0.0358497  |         0.0058752   |      1.04841e-09  |   0.964785 |
| M2       | mes_cos         |   -0.0453421  |         0.00552241  |      2.20169e-16  |   0.955671 |
| M2       | t               |    0.00313778 |         0.000362488 |      4.877e-18    |   1.00314  |
| M2       | fase_cierre     |   -0.674715   |         0.0271792   |      4.86141e-136 |   0.509302 |
| M2       | fase_reapertura |   -0.23336    |         0.0264075   |      9.84438e-19  |   0.791869 |


### Comparación entre Errores Estándar Originales y Robustos de 1-Vía por Hexágono

| modelo   | variable        |   coeficiente |   error_std |      p_valor |   error_std_robusto |   p_valor_robusto |   ratio_error_rob_orig |   exp_coef |
|:---------|:----------------|--------------:|------------:|-------------:|--------------------:|------------------:|-----------------------:|-----------:|
| M0       | mes_sin         |   -0.0485936  | 0.00618362  | 3.88907e-15  |         0.00607327  |      1.23185e-15  |               0.982156 |   0.952568 |
| M0       | mes_cos         |   -0.00310643 | 0.00624906  | 0.619116     |         0.00578522  |      0.591296     |               0.925774 |   0.996898 |
| M0       | t               |   -0.00102304 | 0.000195169 | 1.5899e-07   |         0.000404868 |      0.0115087    |               2.07445  |   0.998977 |
| M1       | mes_sin         |   -0.0400148  | 0.00577186  | 4.12767e-12  |         0.00591762  |      1.36125e-11  |               1.02525  |   0.960775 |
| M1       | mes_cos         |   -0.0223758  | 0.00587508  | 0.000139755  |         0.00553856  |      5.34492e-05  |               0.942721 |   0.977873 |
| M1       | t               |    0.00372654 | 0.000258214 | 3.25571e-47  |         0.000369745 |      6.86342e-24  |               1.43193  |   1.00373  |
| M1       | pandemia        |   -0.379112   | 0.0145701   | 2.95312e-149 |         0.0248796   |      1.98279e-52  |               1.70758  |   0.684469 |
| M2       | mes_sin         |   -0.0358497  | 0.00550963  | 7.68076e-11  |         0.0058752   |      1.04841e-09  |               1.06635  |   0.964785 |
| M2       | mes_cos         |   -0.0453421  | 0.00570286  | 1.85376e-15  |         0.00552241  |      2.20169e-16  |               0.968358 |   0.955671 |
| M2       | t               |    0.00313778 | 0.000247321 | 6.97656e-37  |         0.000362488 |      4.877e-18    |               1.46566  |   1.00314  |
| M2       | fase_cierre     |   -0.674715   | 0.0184331   | 2.51418e-293 |         0.0271792   |      4.86141e-136 |               1.47448  |   0.509302 |
| M2       | fase_reapertura |   -0.23336    | 0.0153515   | 3.47946e-52  |         0.0264075   |      9.84438e-19  |               1.72019  |   0.791869 |


## 5. Errores Estándar con Agrupamiento de Dos Vías: Hexágono y Mes

Estimación con matriz de covarianza robusta agrupada en dos dimensiones simultáneas, el espacio por celda H3 y el tiempo por periodo mensual, mediante `cov_cluster_2groups(m, g_h3, g_mes)[0]`, que calcula $V_{\text{dos\_vias}} = V_{\text{hex}} + V_{\text{mes}} - V_{\text{hex}\times\text{mes}}$ con corrección de muestra finita. Se reportan además los errores agrupados solo por mes:

| modelo   | variable        |   coeficiente |   error_dos_vias |   p_dos_vias |   ratio_vs_original |   ratio_vs_unavia |   error_mes |       p_mes |
|:---------|:----------------|--------------:|-----------------:|-------------:|--------------------:|------------------:|------------:|------------:|
| M0       | mes_sin         |   -0.0485936  |      0.0251791   |  0.053617    |             4.07191 |           4.14589 | 0.0251876   | 0.0536976   |
| M0       | mes_cos         |   -0.00310643 |      0.0246805   |  0.899838    |             3.94947 |           4.26613 | 0.0247661   | 0.900183    |
| M0       | t               |   -0.00102304 |      0.000887652 |  0.249104    |             4.54811 |           2.19245 | 0.000816173 | 0.210036    |
| M1       | mes_sin         |   -0.0400148  |      0.0204197   |  0.0500411   |             3.53781 |           3.45067 | 0.0203814   | 0.0496111   |
| M1       | mes_cos         |   -0.0223758  |      0.0186468   |  0.230147    |             3.17389 |           3.36673 | 0.0187343   | 0.232331    |
| M1       | t               |    0.00372654 |      0.000672135 |  2.95071e-08 |             2.60301 |           1.81784 | 0.000611208 | 1.08067e-09 |
| M1       | pandemia        |   -0.379112   |      0.0679553   |  2.42129e-08 |             4.66402 |           2.73137 | 0.06518     | 6.01353e-09 |
| M2       | mes_sin         |   -0.0358497  |      0.0161357   |  0.0262992   |             2.92863 |           2.7464  | 0.0160286   | 0.025312    |
| M2       | mes_cos         |   -0.0453421  |      0.0144715   |  0.00172918  |             2.53759 |           2.62051 | 0.0145006   | 0.00176657  |
| M2       | t               |    0.00313778 |      0.000591759 |  1.14255e-07 |             2.39267 |           1.63249 | 0.000524137 | 2.14326e-09 |
| M2       | fase_cierre     |   -0.674715   |      0.104976    |  1.29866e-10 |             5.69498 |           3.86236 | 0.104025    | 8.81e-11    |
| M2       | fase_reapertura |   -0.23336    |      0.0471815   |  7.57573e-07 |             3.07341 |           1.78667 | 0.0424037   | 3.7279e-08  |


## 6. Bootstrap por Bloques de Meses para M1 y M2

Bootstrap no paramétrico con bloques temporales contiguos de 3 meses, 300 remuestras y semilla `42`. Conserva el valor original de $t$ y de los indicadores en cada mes remuestreado. Se utilizó la especificación GLM con alpha fijo y los efectos espaciales de hexágono ya estimados pasados como offset, de modo que solo se reestiman los parámetros temporales en cada remuestra. **Declaración metodológica**: Al mantener fijos los efectos espaciales de celda H3, los intervalos de confianza resultantes constituyen una aproximación condicionada a la estructura espacial estimada.


### 6.1. Invalidación del Intervalo Previo de Fase de Cierre

> **Dictamen**: El intervalo de confianza previo reportado para `fase_cierre`, cuyo límite superior era exactamente `0.000000`, queda marcado como **INVÁLIDO**.

> **Causa técnica**: La fase de cierre abarca únicamente 6 meses, de marzo a agosto de 2020, de los 80 meses observados. En el muestreo por bloques aleatorios con reemplazo no estratificado, en exactamente 23 de las 300 remuestras, el 7,67 %, no se extrajo ningún bloque perteneciente a la fase de cierre, es decir $N_{\text{cierre}} = 0$. En esas remuestras la variable fue constante igual a cero y el coeficiente no era matemáticamente estimable; al registrarse espuriamente como cero, los ceros artificiales forzaron el percentil 97,5 a 0,000000. La corrección consiste en dos pasos: primero, descartar formalmente las remuestras no estimables y calcular los percentiles únicamente sobre remuestras válidas; segundo, evaluar la variante estratificada por fase.


### 6.2. Diagnóstico de Remuestras por Modelo y Variable: Variante No Estratificada

| modelo   | variable        |   total_remuestras |   remuestras_descartadas |   remuestras_validas |   pct_validas |
|:---------|:----------------|-------------------:|-------------------------:|---------------------:|--------------:|
| M1       | t               |                300 |                        0 |                  300 |      100      |
| M1       | pandemia        |                300 |                        0 |                  300 |      100      |
| M2       | t               |                300 |                        0 |                  300 |      100      |
| M2       | fase_cierre     |                300 |                       23 |                  277 |       92.3333 |
| M2       | fase_reapertura |                300 |                        1 |                  299 |       99.6667 |


### 6.3. Resultados Bootstrap: Variante No Estratificada, Solo Remuestras Válidas

Calculado estrictamente sobre las remuestras válidas, donde cada indicador tuvo al menos un mes activo. Se reporta media, error estándar, intervalo 95% por percentiles, exponencial y porcentaje de cambio $(\exp(\beta)-1)\times 100\%$:

| modelo   | variable        |   coef_original |   media_bootstrap |   se_bootstrap |   ic_95_inf |   ic_95_sup | excluye_cero   |   exp_coef |   exp_ic_inf |   exp_ic_sup |   pct_cambio_orig |   pct_cambio_boot |   pct_cambio_inf |   pct_cambio_sup |
|:---------|:----------------|----------------:|------------------:|---------------:|------------:|------------:|:---------------|-----------:|-------------:|-------------:|------------------:|------------------:|-----------------:|-----------------:|
| M1       | t               |      0.00372654 |        0.00347485 |    0.000835312 |  0.00192594 |  0.00536793 | Sí             |   1.00373  |     1.00193  |     1.00538  |          0.373349 |          0.348089 |         0.19278  |         0.538236 |
| M1       | pandemia        |     -0.379112   |       -0.389843   |    0.102982    | -0.608817   | -0.213931   | Sí             |   0.684469 |     0.543994 |     0.807404 |        -31.5531   |        -32.2837   |       -45.6006   |       -19.2596   |
| M2       | t               |      0.00313778 |        0.00295297 |    0.000557182 |  0.0017975  |  0.00409461 | Sí             |   1.00314  |     1.0018   |     1.0041   |          0.31427  |          0.295734 |         0.179912 |         0.410301 |
| M2       | fase_cierre     |     -0.674715   |       -0.666759   |    0.100565    | -0.864956   | -0.475831   | Sí             |   0.509302 |     0.42107  |     0.621368 |        -49.0698   |        -48.663    |       -57.893    |       -37.8632   |
| M2       | fase_reapertura |     -0.23336    |       -0.235697   |    0.0361564   | -0.312023   | -0.158964   | Sí             |   0.791869 |     0.731965 |     0.853027 |        -20.8131   |        -20.998    |       -26.8035   |       -14.6973   |


### 6.4. Resultados Bootstrap: Variante Estratificada por Fase

**Declaración metodológica**: Esta variante fija la composición por fase repartiendo los bloques de 3 meses dentro de cada fase: prepandemia, 62 meses; cierre, 6 meses; reapertura, 12 meses. Toda remuestra contiene siempre meses de las tres fases, garantizando 100% de remuestras válidas, 300 de 300:

| modelo   | variable        |   coef_original |   media_bootstrap |   se_bootstrap |   ic_95_inf |   ic_95_sup | excluye_cero   |   exp_coef |   exp_ic_inf |   exp_ic_sup |   pct_cambio_orig |   pct_cambio_boot |   pct_cambio_inf |   pct_cambio_sup |
|:---------|:----------------|----------------:|------------------:|---------------:|------------:|------------:|:---------------|-----------:|-------------:|-------------:|------------------:|------------------:|-----------------:|-----------------:|
| M1       | t               |      0.00372654 |        0.0037546  |    0.000783026 |  0.0021676  |  0.00539018 | Sí             |   1.00373  |     1.00217  |     1.0054   |          0.373349 |          0.376165 |         0.216995 |         0.540474 |
| M1       | pandemia        |     -0.379112   |       -0.400355   |    0.0462375   | -0.49039    | -0.31968    | Sí             |   0.684469 |     0.612387 |     0.726381 |        -31.5531   |        -32.9918   |       -38.7613   |       -27.3619   |
| M2       | t               |      0.00313778 |        0.00300027 |    0.000568225 |  0.00194034 |  0.00421604 | Sí             |   1.00314  |     1.00194  |     1.00422  |          0.31427  |          0.300477 |         0.194223 |         0.422494 |
| M2       | fase_cierre     |     -0.674715   |       -0.727038   |    0.0855598   | -0.901475   | -0.573003   | Sí             |   0.509302 |     0.405971 |     0.56383  |        -49.0698   |        -51.6662   |       -59.4029   |       -43.617    |
| M2       | fase_reapertura |     -0.23336    |       -0.231563   |    0.0345421   | -0.306464   | -0.164879   | Sí             |   0.791869 |     0.736045 |     0.847996 |        -20.8131   |        -20.6707   |       -26.3955   |       -15.2004   |


### 6.5. Verificación de Exclusión de Cero para Fase de Cierre

- En la **Variante No Estratificada, solo válidas**: El intervalo del 95% para `fase_cierre` es `[-0.864956, -0.475831]`. **Excluye el cero**: `Sí`.

- En la **Variante Estratificada por Fase**: El intervalo del 95% para `fase_cierre` es `[-0.901475, -0.573003]`. **Excluye el cero**: `Sí`.


## 7. Comparación de Significancia Estadística al 5% entre Métodos

Comparación de significancia al umbral $\alpha = 0,05$ entre los métodos de inferencia evaluados: Original MLE, Cluster 1-Vía H3, Cluster 2-Vías H3+Mes y Bootstrap por Bloques de 3 meses.

- **Modelo M0, base**:
  - Coeficiente $t$ = -0,001023: p=1,59e-07 original, p=0,0115 cluster hexágono, p=0,249 dos vías. Significativo al 5 % en original, cluster hexágono; **no significativo al 5 %** en dos vías.
  - Coeficiente `mes_sin` = -0,048594: p=3,89e-15 original, p=1,23e-15 cluster hexágono, p=0,0536 dos vías. Significativo al 5 % en original, cluster hexágono; **no significativo al 5 %** en dos vías.
  - Coeficiente `mes_cos` = -0,003106: p=0,619 original, p=0,591 cluster hexágono, p=0,900 dos vías. **No significativo al 5 %** en ninguno de los tres métodos.

- **Modelo M1, pandemia**:
  - Coeficiente $t$ = +0,003727: p=3,26e-47 original, p=6,86e-24 cluster hexágono, p=2,95e-08 dos vías. **Significativo al 5 % en los tres métodos**; intervalo bootstrap 95 %: excluye el cero en la variante no estratificada, excluye el cero en la variante estratificada.
  - Coeficiente `pandemia` = -0,379112: p=2,95e-149 original, p=1,98e-52 cluster hexágono, p=2,42e-08 dos vías. **Significativo al 5 % en los tres métodos**; intervalo bootstrap 95 %: excluye el cero en la variante no estratificada, excluye el cero en la variante estratificada.
  - Coeficiente `mes_sin` = -0,040015: p=4,13e-12 original, p=1,36e-11 cluster hexágono, p=0,0500 dos vías. Significativo al 5 % en original, cluster hexágono; **no significativo al 5 %** en dos vías.
  - Coeficiente `mes_cos` = -0,022376: p=1,40e-04 original, p=5,34e-05 cluster hexágono, p=0,230 dos vías. Significativo al 5 % en original, cluster hexágono; **no significativo al 5 %** en dos vías.

- **Modelo M2, fases de cierre y reapertura**:
  - Coeficiente $t$ = +0,003138: p=6,98e-37 original, p=4,88e-18 cluster hexágono, p=1,14e-07 dos vías. **Significativo al 5 % en los tres métodos**; intervalo bootstrap 95 %: excluye el cero en la variante no estratificada, excluye el cero en la variante estratificada.
  - Coeficiente `fase_cierre` = -0,674715: p=2,51e-293 original, p=4,86e-136 cluster hexágono, p=1,30e-10 dos vías. **Significativo al 5 % en los tres métodos**; intervalo bootstrap 95 %: excluye el cero en la variante no estratificada, excluye el cero en la variante estratificada.
  - Coeficiente `fase_reapertura` = -0,233360: p=3,48e-52 original, p=9,84e-19 cluster hexágono, p=7,58e-07 dos vías. **Significativo al 5 % en los tres métodos**; intervalo bootstrap 95 %: excluye el cero en la variante no estratificada, excluye el cero en la variante estratificada.
  - Coeficiente `mes_sin` = -0,035850: p=7,68e-11 original, p=1,05e-09 cluster hexágono, p=0,0263 dos vías. **Significativo al 5 % en los tres métodos**.
  - Coeficiente `mes_cos` = -0,045342: p=1,85e-15 original, p=2,20e-16 cluster hexágono, p=0,00173 dos vías. **Significativo al 5 % en los tres métodos**.


## 8. Pruebas de Razón de Verosimilitud, Likelihood-Ratio Tests

| comparacion   |   estadistico_lr |   gl |      p_valor |
|:--------------|-----------------:|-----:|-------------:|
| M0 vs M1      |          626.197 |    1 | 3.35662e-138 |
| M1 vs M2      |          518.643 |    1 | 8.35017e-115 |


## 9. Ranking de Efectos por Hexágono Frente a M0

- Correlación de Spearman de efectos espaciales M0 vs M1: `0.999997`.
- Correlación de Spearman de efectos espaciales M0 vs M2: `1.000000`.
- Coincidencia de los 13 hexágonos del top M0 vs M1: `13/13`, Jaccard `1.0000`.
- Coincidencia de los 13 hexágonos del top M0 vs M2: `13/13`, Jaccard `1.0000`.

### Top 10 Hexágonos de Mayor Riesgo Relativo por Modelo

|   puesto | M0_h3           |   M0_efecto | M1_h3           |   M1_efecto | M2_h3           |   M2_efecto |
|---------:|:----------------|------------:|:----------------|------------:|:----------------|------------:|
|        1 | 8766e0933ffffff |       21.03 | 8766e0933ffffff |       21.12 | 8766e0933ffffff |       21.15 |
|        2 | 8766e092effffff |       19.07 | 8766e092effffff |       19.02 | 8766e092effffff |       19.01 |
|        3 | 8766e0901ffffff |       17.91 | 8766e0901ffffff |       18.03 | 8766e0901ffffff |       18    |
|        4 | 8766e092affffff |       17.52 | 8766e092affffff |       17.5  | 8766e092affffff |       17.49 |
|        5 | 8766e42d2ffffff |       17.14 | 8766e42d2ffffff |       17.01 | 8766e42d2ffffff |       17.06 |
|        6 | 8766e0906ffffff |       16.87 | 8766e0906ffffff |       16.95 | 8766e0906ffffff |       16.98 |
|        7 | 8766e428affffff |       15.66 | 8766e428affffff |       15.65 | 8766e428affffff |       15.64 |
|        8 | 8766e42d6ffffff |       14.96 | 8766e0905ffffff |       14.82 | 8766e42d6ffffff |       14.84 |
|        9 | 8766e0905ffffff |       14.79 | 8766e42d6ffffff |       14.79 | 8766e0905ffffff |       14.78 |
|       10 | 8766e0904ffffff |       14.58 | 8766e0904ffffff |       14.56 | 8766e0904ffffff |       14.62 |


## 10. Integridad de Datos

- El archivo original `datos/procesados/variables/efecto_hexagono.parquet` no fue modificado ni sobrescrito.
- Los resultados combinados de efectos se guardaron en `datos/procesados/variables/efecto_hexagono_pandemia`, en formato parquet y csv.
