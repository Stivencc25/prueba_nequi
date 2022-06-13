# Científico de datos en NEQUI: Prueba Técnica

Objetivo: El objetivo de la prueba es idear una solución para identificar transacciones que evidencian un
comportamiento de Mala Practica Transaccional, empleando un producto de datos.
Adicional, describir la solución y detallar cómo incorporar el producto de datos en un marco
operativo.

## Desarrollo

Se procede a realizar en análisis exploratorio de datos, buscando la integralidad de la base de datos y que concuerde con lo presente en el enunciado. En dicho análsis preliminar se hallo:

1) El identificador único contenia duplicados
2) Se encontraban mismas cuentas de origen y destino para tipos de transacciones crédito y Debito, se infiere que es un error en la Bd, por tanto dichos casos se descartan para evitar ruido.
3) Sólo se trabaja con dos archivos que representan una cantidad de 19639397 registros.
4) Se realiza estádistica descriptiva (Pie plots, tablas de frecuencia, histogramas, pruebas de independenicia estádistica, series de tiempo, correlaciones,boxplot, comparación grafica de distribuciones) con el fin de identificar comportamientos diferentes entre las variables.
5) Heat map de frecuencia horaria de cada una de las transacciones
6) Selección de la muestra (se realiza el procedimiento con varias muestras y se valida consistencia de los coeficientes)
## Definición del modelo análitico y solución
* Como se dió a entender un fraccionamiento está compuesto por multiples transacciones que se realizan en un lapso de 24 horas y que se dividen para aúmentar la comisión.
* Dada la indicación se creó una variable (output) con el fin de indentificar dicho suceso pro medio de una agrupación usando el aliado, la sucursal y las cuentas de origen, destino. Se plantearon tres formas de realizarlo una fue realizando una agrupación dadas las siguiente 24h y la siguiente que sirvió para validación dado que computacionalmene es costosa y por último crear una secuencia dado el groupby y de dicha forma se contabiliza cuantos fraccionamiento se realizaron.

## Una solución viable
* A través de la manipulación del DataFrame es posible identificar la mala practica (llamada fraccionamiento), por medio de la base de datos del core financiero, por medio de un proceso automatizado y generando el groupby on streaming se podria generar un disparador que alerte a la compañia de dicha mala practica (en algunos casos el fraccionamiento en un sólo día fue de 200 transacciones).
* Correr un modelo por baches e informar al dia siguiente(en una ventana de 24 horas) la cantidad de fraccionamientos que se presentaron por entidad. Validar con un backtesting que un modelo se esté compartando correctamente(análisis preescriptivo).

## Modelado
- Se tratan de crear variables historicas a partir de los montos transaccionales medios y si habia estado alguna vez en la base de datos.
- Se eliminan variables por alta cardinalidad.
- Se planea hacer un SMOTE.
- Se separa la data en conjunto de validación, train & test.
- Se eliminan variables altamente correlacionadas dada su relación linea y con el fin de no sobreajustar el modelo.
1) Se dicidió usar de manera inicial un modelo de statsmodel (regresión logistica) con el fin de hallar las variables significativas sobre la variable dependiente (dicotomica), se interpretan los coeficientes y se tiene cuidado para no caer en la trampa de la Dummy  (multicolinealidad).
2) Se procede a realizar un modelo Xgboost con una grilla para optimizar los hiperparametros y lograr máximizar el recall y encontrar un punto estable entre sesgo varianza usando el random search con kfold cross validation.
3) Se procede a realizar un modelo LightGBM con una grilla para optimizar los hiperparametros y lograr máximizar el recall y encontrar un punto estable entre sesgo varianza usando el random search con kfold cross validation.
4) Se puede llevar un modelo a producción facilmente logrando concatenar y complementar variables con el historico.
5) Shap values
## Metricas
1) Matriz de confusión, recall, AUC, curvas de validación para validar el tradeoff entre overfiting y underfitting.
2) 

## Propuestas a futuro
1) Evaluar los costos de cada fraccionamiento de las transacciones... si es alto hallar la forma de rechazar dichas fracciones con el fin de desincentivar dicha practica.
2) Generar un accionable con base a un forecasting de esas malas practicas y entregar un informe con el dinero que está costando está practica.
3) Subsanar la data, dado que el credito y el debito tienen la misma cuenta de origen (dueño de cuenta) y destino (cuenta a consignar).



Stiven Cadavid Cataño.
¡Muchas gracias!
