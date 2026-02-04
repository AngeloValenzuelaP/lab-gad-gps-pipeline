📄 README.md — lab-gad-gps-pipeline
# Lab GAD — GPS Pipeline 

Pipeline oficial del Laboratorio de Datos Lab GAD para el procesamiento de datos GPS.

Este repositorio **implementa** los estándares declarados en `lab-gad-data-standards` y produce datasets GPS estandarizados, anonimizados y trazables.

---

## 🎯 Alcance

Este pipeline permite:

- Ingestar archivos RAW de posiciones GPS
- Normalizar:
  - fecha-hora a UTC
  - coordenadas (WGS84 / EPSG:4326)
- Generar identificadores determinísticos (`id_registro`)
- Anonimizar patentes vía cruce controlado
- Aplicar reglas de limpieza y validación
- Calcular índice y nivel de calidad (A/B/C)
- Deduplicar eventos
- Generar:
  - datasets CLEAN y MASTER (GPS_STD)
  - reportes estadísticos
  - metadatos de ejecución
  - diccionario de datos

---

## 🧱 Arquitectura



RAW
↓
CLEAN (normalizado y depurado)
↓
MASTER (GPS_STD anonimizados)
↓
REPORTS / METADATA


La lógica del pipeline **no define reglas**.  
Las reglas se leen desde `lab-gad-data-standards`.

---

## 📂 Estructura del proyecto


- src/labgad_gps/
- cli.py # Entrada por línea de comandos
- pipeline.py # Orquestación end-to-end
- mapping.py # Aplicación de mapeos
- normalize.py # Normalización temporal y espacial
- anonymize.py # Hash HMAC de patentes
- rules_engine.py # Limpieza y validación
- quality.py # Índice de calidad
- report.py # Reporte estadístico
- metadata.py # Lineage y trazabilidad
- dictionary.py # Diccionario de datos


---

## ▶️ Ejecución básica

```bash
python -m labgad_gps.cli \
  --raw /ruta/wisetrack_posiciones.csv \
  --standards /ruta/lab-gad-data-standards \
  --out /ruta/salidas

Variable de entorno requerida
export LABGAD_HASH_SECRET="secreto_irreversible"
```

📊 Outputs generados

CLEAN/

  - posiciones normalizadas

MASTER/

  - GPS_STD (anonimizado)

REPORTS/

  - métricas de calidad y descarte

METADATA/

  - lineage de ejecución

  - diccionario de datos

🔐 Seguridad y anonimización

La patente real no se publica

El cruce con MOVILES se realiza solo para generar hash_vehiculo

El hash utiliza HMAC-SHA256 con secreto externo

Cumple con el marco de Seguridad y Accesos del Lab GAD
