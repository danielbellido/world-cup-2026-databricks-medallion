# World Cup 2026 Data Hub - Medallion Architecture

Pipeline de Ingeniería de Datos desarrollado en **Databricks (Free Edition)** utilizando **PySpark** y **Unity Catalog** para procesar datos históricos y actuales de la Copa del Mundo de la FIFA.

## 📐 Arquitectura del Proyecto
Se implementó una arquitectura Medallion dividida en tres capas funcionales:
- **Bronze:** Ingesta del dataset crudo de Kaggle en formato Delta Lake con control de evolución de esquemas.
- **Silver:** Limpieza de datos, casteo de tipos de tiempo/goles, tratamiento de nulos en penales y modelado relacional (Hechos/Dimensiones).
- **Gold:** Agregaciones de negocio orientadas al rendimiento histórico de países y métricas avanzadas como Expected Goals (xG).

## 🛠️ Tecnologías Utilizadas
- Databricks Serverless
- Apache Spark (PySpark)
- Delta Lake (Optimización con Z-Order)
- Unity Catalog (Gobernanza de Datos)

## 🔄 Orquestación y Automatización (Databricks Workflows)

Para garantizar que el pipeline sea agnóstico, automatizado y eficiente, el proceso de datos no se ejecuta de forma manual. Se implementó un flujo de trabajo utilizando **Databricks Workflows (Jobs)**, estructurando las tareas de manera secuencial y respetando las dependencias lógicas de la arquitectura Medallion.

El flujo de ejecución del Job se compone de las siguientes tareas:

1. **`Ingest_Bronze_Raw`**: 
   - **Propósito:** Descarga la data fuente e inicializa las tablas crudas de manera transaccional.
   - **Notebook:** `notebooks/01_bronze_ingestion.py`
   - **Almacenamiento:** Escribe en `world_cup_2026.bronze.matches_raw`.

2. **`Transform_Silver_Clean`** (Depende de `Ingest_Bronze_Raw`):
   - **Propósito:** Lee los datos de la capa Bronze solo si la ingesta previa fue exitosa. Ejecuta transformaciones de tipado, limpieza, normalización de strings y aserciones de calidad de datos (*Data Quality Gates*).
   - **Notebook:** `notebooks/02_silver_transformation.py`
   - **Almacenamiento:** Escribe en `world_cup_2026.silver.fact_matches`.

3. **`Aggregate_Gold_Analytics`** (Depende de `Transform_Silver_Clean`):
   - **Propósito:** Construye las vistas agregadas y los modelos dimensionales analíticos optimizados con `Z-ORDER` para su consumo inmediato.
   - **Notebook:** `notebooks/03_gold_analytics.py`
   - **Almacenamiento:** Escribe en `world_cup_2026.gold.team_historical_performance`.

> 💡 **Tip de Arquitectura:** Al utilizar la orquestación nativa de Databricks, se garantiza el aislamiento de fallos: si la validación de calidad de datos en la capa Silver detecta una anomalía, el flujo se detiene inmediatamente, evitando la corrupción de métricas analíticas en la capa Gold.

---<img width="1173" height="596" alt="image" src="https://github.com/user-attachments/assets/f3b6d781-d89b-481b-8069-28b53b6d764f" />

<img width="1237" height="573" alt="image" src="https://github.com/user-attachments/assets/a7802c23-83ed-4ddb-9c0b-024e3a687cf8" />

