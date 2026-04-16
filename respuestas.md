# Respuestas — Práctica Final: Análisis y Modelado de Datos

> Rellena cada pregunta con tu respuesta. Cuando se pida un valor numérico, incluye también una breve explicación de lo que significa.

---

## Ejercicio 1 — Análisis Estadístico Descriptivo
---
Añade aqui tu descripción y analisis:

--- 

**Pregunta 1.1** — ¿De qué fuente proviene el dataset y cuál es la variable objetivo (target)? ¿Por qué tiene sentido hacer regresión sobre ella?

> El dataset utilizado es de un sistema ERP de gestión de ventas, recoge informacion sobre perdido, clientes, productos y envios.
La variable objetivo es MARGEN_DOLARES, representando el beneficio obtenido en cada venta.

Tiene sentido aplicar regresion a esta variable por que es numerica continmua y depende de multiples factores como el precio del producto, 
lso descuentos aplicados, la cantidad vendidad o el tipo de transaccion. Esto permite analizar que variables influyen en el margen y predecir su valor en futuras transacciones

**Pregunta 1.2** — ¿Qué distribución tienen las principales variables numéricas y has encontrado outliers? Indica en qué variables y qué has decidido hacer con ellos.

> Las variables numericas presentan distribuciones asimetricas, en particular la variable objetivo MARGEN_DOLARES muestra la asimetria fuerte hacia la izquierda(asimetria negativa), 
con una concentracion de valores cercanos a cero y una cola larga hacia valores negativos, que indica numerosas ventas con perdidas significativas.
![alt text](image.png)

> En cuanto a los outliers, se detectaron principalmente en variables relacionadas con el margen y los descuentos
> - MARGEN_DOLARES (~10.5% de outliers)
> - MARGEN_PORCENTAJE (~9.6%)
> - DESCUENTO (~4.38%)
Se identificaron mediante el metodo del rango intercuartilico -> IQR
![alt text](image-1.png)

> No se eliminaron los valores atípicos, ya que representan situaciones reales del negocio (por ejemplo, ventas con grandes descuentos o pérdidas), 
por lo que forman parte de la variabilidad natural del dataset.



**Pregunta 1.3** — ¿Qué tres variables numéricas tienen mayor correlación (en valor absoluto) con la variable objetivo? Indica los coeficientes.

> Las tres variable snuméricas con mayor correlación (en valor absoluto) respecto a la variable objetivo MARGEN_DOLARES son:
> - MARGEN_PORCENTAJE → 0.865
> - PRECIO_TOTAL_CON_DESCUENTO → 0.118
> - PRECIO_TOTAL_SIN_DESCUENTO → 0.116
![alt text](image-2.png)

> La alta correlación con el margen porcentual indica que el beneficio en terminos absolutos esta fuertemente relacionado con el porcentaje de margen aplicado en cada venta.
> Las otras dos variables presentan correlaciones más bajas, lo que sugiere una relación más débil con el margen.

**Pregunta 1.4** — ¿Hay valores nulos en el dataset? ¿Qué porcentaje representan y cómo los has tratado?

> No se han detectado valores nulos en el dataset, ya que todas las variables presentan un 0% de valores faltantes.
Por lo tanto, no ha sido necesario aplicar técnicas de imputación ni eliminación de datos, lo que facilita el análisis y garantiza la integridad de la información.

---

## Ejercicio 2 — Inferencia con Scikit-Learn

---
Añade aqui tu descripción y analisis:

---

**Pregunta 2.1** — Indica los valores de MAE, RMSE y R² de la regresión lineal sobre el test set. ¿El modelo funciona bien? ¿Por qué?

> El modelo de regresion lineal ha obtenido los siguientes resultados:
> - MAE (Error absoluto medio): 24.82
> - RMSE (Raíz del error cuadrático medio): 47.83
> - R² (Coeficiente de determinación): 0.756

> El valor de R² = 0.756 indica que el modelo es capaz de explicar aproximadamente el 75.6% de la variabilidad de la variable objetivo (MARGEN_DOLARES), lo cual sugiere un buen ajuste general al estar cerca de 1.
 
> En cuanto a los errores, el MAE de 24.82 indica que, en promedio, el modelo se equivoca en unos 25 dólares por predicción. Sin embargo, el hecho de que el RMSE (47.83) sea considerablemente mayor que el MAE sugiere la presencia de errores grandes en algunas observaciones, lo cual es coherente con la existencia de outliers detectados previamente en el análisis descriptivo.

> El gráfico de residuos muestra que estos no se distribuyen completamente de forma aleatoria, observándose ciertos patrones y una dispersión no constante, lo que indica posibles problemas de heterocedasticidad y que la relación entre variables no es perfectamente lineal.
![alt text](output/ej2_residuos.png)

> En conjunto, el modelo funciona razonablemente bien como primera aproximación, pero presenta limitaciones que vienen de la naturaleza de los datos, especialmente la presencia de outliers y relaciones no lineales, por lo que podría mejorarse mediante modelos más complejos o técnicas adicionales de preprocesamiento.
Además, se observa que la variable MARGEN_PORCENTAJE es la más influyente en el modelo, lo cual es consistente con el análisis de correlaciones realizado previamente.


---

## Ejercicio 3 — Regresión Lineal Múltiple en NumPy

---
Añade aqui tu descripción y analisis:

---

**Pregunta 3.1** — Explica en tus propias palabras qué hace la fórmula β = (XᵀX)⁻¹ Xᵀy y por qué es necesario añadir una columna de unos a la matriz X.

> Esta formula nos sirve para saber los coeficientes que no esten en la primera columna  que se utilizan para minimizar el error cuadrático del MSE entre los valores (y) y los valores que predice el modelo.
Por dentro lo que hace es comparar como hemos dicho los valores que teniamos y los predichos.

> La columna añadida es para poder estimar un primer valor que no cambia (β sub 0) que también se llama intercepto. Sin esta columna el modelo siemp0re tendria que pasar por el origen (putno 0,0 del plano) , lo que limitaria su cpacidad de ajustar.

**Pregunta 3.2** — Copia aquí los cuatro coeficientes ajustados por tu función y compáralos con los valores de referencia del enunciado.

| Parametro | Valor real| Valor ajustado |
|-----------|-----------|----------------|
| β₀        | 5.0       |    4.864995    |
| β₁        | 2.0       |    2.063618    |
| β₂        | -1.0      |   -10117038    |
| β₃        | 0.5       |    0.438517    |

> Los coeficientes ajustados son muy cercanos a los valores reales, olo que indica que el modelo ha sido caapz de aproxcimar correctamente la relacion entre variables.
Las pequeñas diferencias se deben al ruido en los dato sy al proceso de esitmacion, lo cual es esperrable en un contexto real.

**Pregunta 3.3** — ¿Qué valores de MAE, RMSE y R² has obtenido? ¿Se aproximan a los de referencia?

> Los valores en test son:
> MAE = 1.1665
> RMSE = 1.4612
> R² = 0.6897

>Estos resultados son coherentes con los valores de referencia, mostrando un buen ajuste del modelo.
El error es relativamente bajo, lo que indica que las predicciones están cerca de los valores reales.
El valor de R² (~0.69) indica que el modelo explica aproximadamente el 69% de la variabilidad de los datos, lo cual es razonable teniendo en cuenta la presencia de ruido.

**Pregunta 3.4* — Compara los resultados con la reacción logística anterior para tu dataset y comprueba si el resultado es parecido. Explica qué ha sucedido. 

> Al compararlo con el resultado sacado de sklearn , se observa que sacan resultados similares.
Ambos se impelmentan con el mismo algoritmo: regresión lineal por mínimos cuadrados.
Las dieferencias se pueden deber al preprocseamiento, la normalizacion de daatos o la precision numérica en lo calculos.
En eun ejercicio lo hemops hecho manualmente con numpy y en el otro lo hemos hecho usando sklearn


---

## Ejercicio 4 — Series Temporales
---
Añade aqui tu descripción y analisis:

---

**Pregunta 4.1** — ¿La serie presenta tendencia? Descríbela brevemente (tipo, dirección, magnitud aproximada).

> La serie presenta una clara tendencia creciente a lo largo del tiempo.
Se trata de una tendencia de tipo lineal con suavidad, donde los valores aumentan progresivamente desde aproximadamente 60 hasta valores cercanos a 160–170 al final del periodo.
Esto indica un crecimiento sostenido en el tiempo, posiblemente asociado a un incremento estructural en la variable analizada.
![alt text](images/tendencia.png)

**Pregunta 4.2** — ¿Hay estacionalidad? Indica el periodo aproximado en días y la amplitud del patrón estacional.

> Sí, la serie presenta un patrón estacional claramente visible.
El periodo aproximado de la estacionalidad es de 365 días (anual), ya que se repiten patrones similares cada año.
La amplitud del componente estacional es aproximadamente de ±15–20 unidades, observándose oscilaciones regulares alrededor de la tendencia.
![alt text](output/ej4_serie_original.png)

**Pregunta 4.3** — ¿Se aprecian ciclos de largo plazo en la serie? ¿Cómo los diferencias de la tendencia?

> No se aprecian ciclos de largo plazo diferenciados más allá de la tendencia general creciente.
La diferencia entre ciclos y tendencia es que:

> - La tendencia representa un cambio estructural sostenido en el tiempo (en este caso, crecimiento).
> - Los ciclos implican fluctuaciones de largo plazo no periódicas alrededor de la tendencia.

> En este caso, las variaciones observadas se explican principalmente por la combinación de tendencia y estacionalidad, sin evidencia clara de ciclos adicionales.

**Pregunta 4.4** — ¿El residuo se ajusta a un ruido ideal? Indica la media, la desviación típica y el resultado del test de normalidad (p-value) para justificar tu respuesta.

> Datos:
> - Media ≈ 0.127
> - Std ≈ 3.22
> - p-value = 0.5766

> El residuo se aproxima bastante a un ruido blanco ideal.
La media del residuo es cercana a cero (≈ 0.127), lo que indica que no existe sesgo sistemático en el modelo.
La desviación típica es moderada (≈ 3.22), reflejando la variabilidad del error.
Además, el test de normalidad de Jarque-Bera arroja un p-valor de 0.5766 , lo que implica que no se rechaza la hipótesis de normalidad.

> Por otro lado, los gráficos ACF y PACF muestran que no existe autocorrelación significativa en los residuos, ya que la mayoría de los valores se encuentran dentro de los intervalos de confianza.
En conjunto, estos resultados indican que el residuo se comporta de forma cercana a un ruido blanco, cumpliendo los supuestos del modelo.

![alt text](output/ej4_acf_pacf.png)
---

*Fin del documento de respuestas*