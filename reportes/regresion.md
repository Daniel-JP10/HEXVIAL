# Modelo de regresión, binomial negativa con efecto por hexágono

Observaciones: 9840. Hexágonos: 123. Parámetros estimados: 127.

AIC: 60054.1, frente a 83796.2 sin efecto por hexágono, una mejora de 23742 puntos.
Log-verosimilitud: -29900.1.
Alpha de sobredispersión residual: 0.1004, mucho menor que antes del efecto por hexágono, porque ya se absorbió la mayor parte de la heterogeneidad espacial.

## Efectos temporales

- Estacionalidad, mes_sin: -0.0486, p = 3.889e-15; mes_cos: -0.0031, p = 6.191e-01. Se interpretan en conjunto, un componente no significativo no invalida la estacionalidad si el otro lo es.
- Tendencia en el tiempo, t: -0.0010, p = 1.590e-07. Es la tendencia del modelo base, sin control de la pandemia, y no debe leerse como conclusión. El análisis que controla la pandemia y usa errores estándar robustos está en regresion_pandemia.md.

## Top 10 hexágonos de mayor riesgo relativo

             h3     coef  efecto_vs_referencia  efecto_vs_promedio  indice_esperado_mes
8766e0933ffffff 7.025234           1124.657786           21.028333           140.642949
8766e092effffff 6.927522           1019.963607           19.070810           127.550524
8766e0901ffffff 6.864936            958.084989           17.913832           119.812355
8766e092affffff 6.842684            937.000325           17.519601           117.175634
8766e42d2ffffff 6.820593            916.528643           17.136831           114.615568
8766e0906ffffff 6.804796            902.163677           16.868241           112.819172
8766e428affffff 6.730744            837.769987           15.664237           104.766484
8766e42d6ffffff 6.684508            799.917277           14.956485           100.032851
8766e0905ffffff 6.673464            791.131436           14.792211            98.934146
8766e0904ffffff 6.658905            779.696331           14.578403            97.504141

## Bottom 10 hexágonos de menor riesgo relativo

             h3      coef  efecto_vs_referencia  efecto_vs_promedio  indice_esperado_mes
8766e466dffffff -0.917144              0.399659            0.007473             0.049979
8766e0833ffffff -0.917220              0.399629            0.007472             0.049975
8766e0175ffffff -1.204922              0.299715            0.005604             0.037481
8766e42a0ffffff -1.610249              0.199838            0.003736             0.024991
8766e0802ffffff -2.303274              0.099931            0.001868             0.012497
8766e42d3ffffff -2.303385              0.099920            0.001868             0.012495
8766e080affffff -2.303443              0.099914            0.001868             0.012495
8766e092dffffff -2.303457              0.099913            0.001868             0.012494
8766e0803ffffff -2.303463              0.099912            0.001868             0.012494
8766e0800ffffff -2.303463              0.099912            0.001868             0.012494

efecto_vs_promedio indica cuántas veces el índice del hexágono promedio tiene ese hexágono, manteniendo fijos el mes y la tendencia. indice_esperado_mes es el índice mensual esperado en un mes típico. efecto_vs_referencia mide contra el hexágono que el modelo toma como base, que es uno de muy poca actividad, y se conserva solo por trazabilidad. La tabla completa queda en datos/procesados/variables/efecto_hexagono y alimenta la capa de priorización del mapa junto con los clústeres.
