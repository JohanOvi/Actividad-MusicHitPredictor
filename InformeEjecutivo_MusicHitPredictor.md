# Informe ejecutivo – Music Hit Predictor

> **Dirigido a:** dirección de la discográfica
> **Fuente de las cifras:** ejecución del notebook del proyecto (semilla 42)

---

## 1. El problema

Queríamos saber si las características musicales de una canción (bailabilidad, energía, positividad, tempo, duración, acústica, género, entre otras) permiten anticipar qué tan alto llegó en el **Billboard Hot 100**. Los niveles de éxito son cuatro:

| Nivel | Puesto máximo alcanzado |
|---|---|
| Entrada simple | 41–100 |
| Top 40 | 11–40 |
| Top 10 | 2–10 |
| Número 1 | 1 |

Usamos un dataset público de Kaggle con **32.503 canciones entre 1958 y 2026**, con audio de Spotify y etiquetas generadas por inteligencia artificial.

---

## 2. Calidad de los datos

Tras una auditoría trabajamos con **24.317 canciones verificables**:

- Eliminamos **24 duplicados exactos**.
- Excluimos **5.159 canciones sin audio** de Spotify.
- Apartamos **3.000 registros de 2021–2026** con señales claras de haber sido generados artificialmente: fechas que no coinciden con las de Billboard, títulos tipo "Version 1057" y una cantidad irreal de números 1.

---

## 3. Hallazgos del análisis exploratorio

- **La música cambió con el tiempo:** hoy el Hot 100 es menos acústico, menos "alegre" y con más partes habladas, y las canciones son más cortas desde los años 90.
- **Las diferencias entre niveles de éxito son mínimas:** los números 1 son apenas un poco más bailables, positivos y largos, y ninguna característica explica más del **1 %** de las diferencias.
- **El género musical** aporta algo más que los rasgos de sonido.

---

## 4. Mejor modelo

Comparamos un modelo de referencia, regresión logística, árbol de decisión, Random Forest y una red neuronal.

| Modelo | Macro F1 en datos nunca vistos |
|---|---|
| **Random Forest optimizado (mejor)** | **0,31** |
| Red neuronal | 0,27 |
| Modelo de referencia | 0,17 |

La red neuronal no superó al Random Forest pese a ser más compleja. El modelo ganador es una mejora real, pero **modesta**: su capacidad para ordenar canciones (**AUC = 0,60**) está poco por encima del azar (0,50).

---

## 5. Errores y su costo

- El modelo detecta solo **13 de cada 100 números 1** y, en **uno de cada tres casos**, los confunde con entradas simples.
- Si forzamos que detecte más números 1, encuentra el **62 %**, pero solo el **6 %** de sus alertas son correctas.

En la práctica, esto significa **invertir promoción en muchas canciones equivocadas** o **dejar pasar éxitos reales**.

---

## 6. Hallazgo sobre la inteligencia artificial

- Las etiquetas de IA que **describen la música** no mejoran el modelo, porque repiten información que ya estaba en los datos.
- Las que sí "mejoran" el resultado lo hacen porque **contienen la respuesta**: describen cuántas semanas duró la canción en la lista o mencionan su posición. Usarlas sería un autoengaño.

---

## 7. Limitaciones

- El dataset solo incluye canciones que **ya entraron** al Hot 100, por lo que no sirve para predecir si una canción nueva entrará.
- Faltan variables decisivas: promoción, fama del artista, radio, redes sociales o fecha de lanzamiento.
- Los datos de Spotify tienen errores de asociación y sesgo hacia canciones disponibles en la plataforma.
- Los resultados son **asociaciones, no causas**.

---

## 8. Recomendaciones

1. Usar el análisis para **entender tendencias**, no para decidir automáticamente qué canciones promocionar.
2. Mantener el **criterio artístico** como decisión principal.
3. **Auditar siempre** los datos enriquecidos con IA antes de comprarlos o usarlos.

---

## 9. Trabajo futuro

- Incorporar datos de promoción, historial del artista, *streaming* y redes sociales.
- Incluir canciones que **no** entraron a la lista.
- Analizar por época y por género.
- Validar con lanzamientos recientes y reales.

---

## 10. Consideraciones éticas

- No usar el modelo para **descartar artistas emergentes**: reforzaría los sesgos históricos de la industria hacia ciertos géneros.
- Ser transparentes sobre sus limitaciones y **no presentarlo como un "predictor de éxitos"**.
