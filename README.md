# mds102-datos

Publicaciones de datos del curso **MDS102 · Programación, SQL y Analítica Reproducible**
Maestría en Data Science — FIEECS, Universidad Nacional de Ingeniería.

Este repositorio contiene únicamente **datos**. El código de cada estudiante vive en su propio
repositorio; los datos no se copian ahí, se referencian desde aquí mediante su dirección.

---

## Publicaciones disponibles

| Publicación | Filas | Formato | Tamaño | Uso |
|---|---|---|---|---|
| `FLIGHTS-INTRO-v1.csv` | 5 000 | CSV | 251 KB | Sesión 1 |
| `FLIGHTS-INTRO-v1-10k.csv` | 10 000 | CSV | 503 KB | Sesión 2 |

Las publicaciones de mayor volumen (Parquet particionado, a partir de la semana 4) residen en la
unidad compartida de Drive institucional, no en este repositorio.

---

## Fuente

Bureau of Transportation Statistics (Estados Unidos), programa **On-Time Performance**, que
publica todos los vuelos comerciales programados de aerolíneas con cuota de mercado relevante.

- **Cobertura:** vuelos con origen en Seattle (SEA) y Portland (PDX), año 2014.
- **Universo:** 162 049 vuelos.
- **Muestreo:** selección aleatoria con semilla fija (102), ordenada por fecha, aerolínea y número de vuelo.

## Tratamiento aplicado

1. Se construyó `fl_date` a partir de año, mes y día.
2. Se derivó `cancelled` = 1 cuando no existen hora real de salida ni de llegada.
3. Se derivó `diverted` = 1 cuando existe hora de salida pero no de llegada.
4. **No se corrigió, imputó ni eliminó ningún valor.** Las ausencias del archivo son las del origen.

---

## Diccionario de datos

| Columna | Tipo | Descripción |
|---|---|---|
| `fl_date` | texto (AAAA-MM-DD) | Fecha del vuelo programado |
| `carrier` | texto | Código de la aerolínea |
| `flight` | entero | Número de vuelo |
| `tailnum` | texto | Matrícula de la aeronave |
| `origin` | texto | Aeropuerto de origen |
| `dest` | texto | Aeropuerto de destino |
| `dep_delay` | entero | Minutos de retraso en la salida; negativo indica adelanto |
| `arr_delay` | entero | Minutos de retraso en la llegada |
| `cancelled` | entero (0/1) | 1 si el vuelo se canceló |
| `diverted` | entero (0/1) | 1 si el vuelo se desvió |
| `distance` | entero | Distancia en millas |
| `air_time` | entero | Minutos en el aire |

---

## Resultados de referencia — `FLIGHTS-INTRO-v1.csv`

Valores que debe obtener cualquier ejecución correcta.

| Indicador | Valor |
|---|---|
| Filas / columnas | 5 000 / 12 |
| Vuelos cancelados | 21 (0,42 %) |
| Vuelos desviados | 6 |
| Retraso promedio de salida | 6,6 min |
| Retraso mínimo / máximo | −18 / 713 min |
| Ausentes en `dep_delay` | 21 |
| Ausentes en `arr_delay` | 37 |
| Ausentes en `tailnum` | 6 |
| Aerolíneas distintas | 11 |
| Destinos distintos | 70 |
| Aerolínea con más vuelos | AS (1 923) |

### Nota sobre las ausencias

Los 21 ausentes de `dep_delay` coinciden exactamente con los vuelos cancelados: un vuelo que
nunca despegó no puede tener retraso de salida. Es una **ausencia estructural** y no un error.

`arr_delay` tiene 37 ausentes, diez más que la suma de cancelados y desviados. Esa diferencia
proviene del origen y se conservó deliberadamente: constituye un caso auténtico de ausencia sin
explicación evidente, que se trabaja en la sesión sobre contratos de datos.

---

## Uso desde Colab

```python
URL_DATOS = 'https://raw.githubusercontent.com/<ORGANIZACION>/mds102-datos/main/FLIGHTS-INTRO-v1.csv'
ARCHIVO = 'FLIGHTS-INTRO-v1.csv'

import urllib.request
from pathlib import Path

if not Path(ARCHIVO).exists():
    urllib.request.urlretrieve(URL_DATOS, ARCHIVO)
```

## Verificación de integridad

Las sumas de verificación están en `CHECKSUMS.txt`.

```bash
sha256sum -c CHECKSUMS.txt
```

---

## Versionado

Las publicaciones son **inmutables**. Una corrección genera una versión nueva (`-v2`) y la
anterior se conserva, de modo que cualquier resultado obtenido en clase siga siendo reproducible.

## Licencia

Los datos de origen son de dominio público (obra del gobierno federal de Estados Unidos). Esta
recopilación se distribuye con fines educativos.
