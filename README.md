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
