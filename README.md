# Diagnóstico de marca empleadora con reseñas de Glassdoor

Caso práctico de NLP del Máster de Evolve Academy. Se analizan las reseñas públicas de Glassdoor para identificar los puntos fuertes de **Barclays** como empleador y sus áreas de mejora, comparándolos con los de cuatro competidores directos. Un problema solo se considera prioritario si Barclays está peor que el mercado.

## Datos

- Fuente: *Glassdoor Job Reviews 1 y 2* (D. Gauthier, Kaggle), en un único fichero Parquet facilitado en el curso.
- Periodo: de julio de 2018 a abril de 2023 (2018 y 2023 son años parciales).
- Empresas: Barclays y, como peers, HSBC, Standard-Chartered-Bank, NatWest-Group y Deutsche-Bank (38.724 reseñas disponibles).
- Muestra: 2.000 reseñas por empresa (10.000 en total, semilla 42).
- Se descartó HSBC-Holdings porque su serie termina en junio de 2021 y 6.323 de sus reseñas son idénticas a las de HSBC.

El fichero de datos **no se incluye** en el repositorio. Hay que colocar `glassdoor_reviews_hr.parquet` en la misma carpeta que el notebook.

## Método

1. **Preparación y control de calidad:** variables derivadas (situación actual/antiguo, antigüedad, recomendación), detección de marcadores vacíos y de la ausencia declarada de contras mediante expresiones regulares validadas en tres rondas.
2. **Sentimiento:** `distilbert-base-uncased-finetuned-sst-2-english` como control de coherencia entre pros y cons.
3. **Temas:** BERTopic (embeddings `all-MiniLM-L6-v2`, UMAP y HDBSCAN), con un modelo para pros (50 temas) y otro para cons (39 temas), agrupados en 11 categorías de negocio.
4. **Comparación:** balance neto (pros − cons) de Barclays frente a la media de los peers, con intervalos de confianza del 95 % por bootstrap, pruebas de robustez y una matriz de priorización con tres señales.
5. **Análisis adicionales:** evolución temporal y análisis por antigüedad, recomendación y motivos de salida.

## Resultados principales

- Barclays tiene la valoración media más alta del grupo (3,89), aunque su ventaja se ha estrechado: de unos 0,22 puntos en 2018-19 a 0,07 en 2022-23.
- Puntos fuertes: compensación y beneficios (+4,3 puntos de balance neto frente a los peers) y estabilidad y reestructuraciones (+1,9).
- Prioridad alta de mejora: cultura y ambiente, y tecnología y sistemas. Prioridad media: empresa y marca y naturaleza del puesto y ubicación. Conciliación queda en vigilancia.
- Tecnología es el único motivo de salida significativo entre los antiguos empleados.

## Contenido del repositorio

| Archivo | Descripción |
|---|---|
| `NLP_Marta_Parás_Gutiérrez.ipynb` | Notebook con todo el análisis, explicado paso a paso |
| `Presentacion_Barclays_Marca_Empleadora.pptx` | Presentación de resultados |
| `fig_*.png` | Figuras usadas en la presentación |
| `requirements.txt` | Dependencias |

## Cómo ejecutarlo

Probado con Python 3.12.5, en CPU y sin GPU.

```bash
pip install -r requirements.txt
jupyter notebook NLP_Marta_Parás_Gutiérrez.ipynb
```

Primero se coloca el fichero Parquet junto al notebook y se ejecuta con *Restart & Run All*. La clasificación de sentimiento tarda unos 10 minutos en CPU y se guarda en `sentimiento_resenas.parquet` para no repetirla; los embeddings de BERTopic también se guardan en caché.

## Limitaciones

Muestra de 2.000 reseñas por empresa; las reseñas son voluntarias y pueden sobrerrepresentar opiniones extremas; el modelo de sentimiento es binario; cerca del 27 % de las reseñas queda como outlier en los modelos de temas; 2018 y 2023 son años parciales.

## Autoría

Marta Parás Gutiérrez · Máster de Evolve Academy
