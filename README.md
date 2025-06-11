# Bia Energy Case – Solucion Airflow 

**Autor:** Jhonatan Andres Saldarriaga I.  
**GitHub:** [JhonatanS93-DE](https://github.com/JhonatanS93-DE)

Este repositorio contiene una solución escalable del caso técnico de Bia Energy usando **Apache Airflow**, contenedores Docker y PostgreSQL. El pipeline orquesta un proceso ETL completo que:

- Valida datos geográficos desde un CSV.
- Enriquece con información de código postal vía API.
- Almacena resultados en una base de datos.
- Genera reportes de calidad y análisis.

---

## Estructura del proyecto

```
bia-energy-case-airflow/
├── dags/                   # DAG de Airflow con tareas ETL
│   └── bia_pipeline_dag.py
├── data/                   # Carpeta mapeada para almacenar archivos CSV originales
│   └── postcodesgeo.csv
├── reports/                # Carpeta para reportes generados por el DAG
│   └── (top_postcodes.csv, quality_stats.csv)
├── logs/                   # Carpeta de logs de Airflow
├── docker-compose.yml      # Orquestación de servicios con Docker
├── .gitignore
└── README.md
```

---

## Cómo ejecutar el proyecto

### 1. Clonar el repositorio

```bash
git clone https://github.com/tu_usuario/bia-energy-case-airflow.git
cd bia-energy-case-airflow
```

### 2. Crear las carpetas necesarias

```bash
mkdir -p dags data reports logs
```

> Copia tu archivo `postcodesgeo.csv` dentro de `data/` si quieres correr el pipeline.

### 3. Levantar los servicios

```bash
docker-compose up --build
```

Esto levantará:

- `airflow-webserver`: Interfaz web de Airflow en http://localhost:8080
- `airflow-scheduler`: Programa las tareas del DAG.
- `postgres`: Base de datos PostgreSQL para almacenamiento.

---

## Acceso a Airflow

- URL: [http://localhost:8080](http://localhost:8080)
- Usuario: `admin`
- Contraseña: `admin`

---

## Reportes generados

Después de ejecutar el DAG manualmente, encontrarás:

- `/reports/top_postcodes.csv`: Top 10 códigos postales más frecuentes.
- `/reports/quality_stats.csv`: Métricas de calidad de datos como porcentaje de `postcode` nulos.

---

## Acceso a la base de datos en DBeaver

- **Host**: `localhost`
- **Puerto**: `5432`
- **Database**: `bia_db`
- **Usuario**: `bia_user`
- **Contraseña**: `bia_password`

---

## Variables de conexión (Airflow)

Ya están configuradas por defecto en el `docker-compose.yml`:

```env
AIRFLOW__CORE__SQL_ALCHEMY_CONN=postgresql+psycopg2://bia_user:bia_password@postgres:5432/bia_db
```

---

## DAG: `bia_pipeline_dag`

Este DAG ejecuta:

1. `ingest_data`: Limpieza, validación y renombre de columnas.
2. `enrich_data`: Consulta a la API `https://api.postcodes.io`.
3. `store_data`: Inserción en PostgreSQL y exportación CSV.
4. `generate_reports`: Generación de reportes analíticos y de calidad.

---

Contacto autor: **jhonatan1393@gmail.com**  
También puedes dejar comentarios en el repositorio de GitHub.

---