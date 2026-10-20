# Datos del curso MDS102

Programación, SQL y Analítica Reproducible · Maestría en Data Science · Universidad Nacional de Ingeniería

## Fuente

Chen, D. (2015). *Online Retail* [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5BW33. Licencia CC BY 4.0.

Ventas de una tienda en línea del Reino Unido entre el 1 de diciembre de 2010 y el 9 de diciembre de 2011.

## Archivos

| Archivo | Filas | Contenido |
|---|---|---|
| `prepared/transactions.csv` | 541 909 | todas las líneas del archivo original |
| `prepared/transactions.parquet` | 541 909 | el mismo contenido, en formato Parquet |
| `teaching/retail_small.csv` | 3 108 | líneas del 1 de diciembre de 2010 |
| `teaching/retail_small.xlsx` | 3 108 | el mismo contenido, en Excel |

## Columnas

| Columna | Significado |
|---|---|
| `InvoiceNo` | número de factura; si empieza con `C`, la factura está cancelada |
| `StockCode` | código del producto |
| `Description` | nombre del producto |
| `Quantity` | unidades; negativa en una devolución |
| `InvoiceDate` | fecha y hora de la factura (`AAAA-MM-DD hh:mm:ss`) |
| `UnitPrice` | precio por unidad, en libras esterlinas |
| `CustomerID` | código del cliente; vacío si la venta no tiene cliente identificado |
| `Country` | país del cliente |

## Transformaciones aplicadas

- Los archivos de `prepared/` contienen las mismas filas, columnas y valores que el archivo original `Online Retail.xlsx`, sin limpieza ni filtros.
- `InvoiceDate` se escribe en formato `AAAA-MM-DD hh:mm:ss`.
- Una descripción numérica (producto `20713`) se guarda como texto, para que la columna tenga un solo tipo.
- `retail_small` contiene solo las líneas con fecha 1 de diciembre de 2010.
- Se conservan las facturas canceladas, las cantidades negativas, los precios cero o negativos y los valores faltantes del original.

## Lectura

```python
import pandas as pd

URL_DATOS = "https://raw.githubusercontent.com/MCIES-DCIES-FIEECS-UNI/mds102-datos/main/"
transacciones = pd.read_csv(URL_DATOS + "prepared/transactions.csv")
```

## Verificación (SHA-256)

| Archivo | SHA-256 |
|---|---|
| `prepared/transactions.csv` | `c35975c40d0e1e105090fbb11188a409393055f873841bd012915475fc84e69f` |
| `prepared/transactions.parquet` | `fcb003532d9ce3d50d11cc97982125fa39289efbbb5a6a0ebc30ad75e8402b08` |
| `teaching/retail_small.csv` | `45ca8842daf556b96947109ad92d666391410a2a3e894bab7644773d1ff539b3` |
| `teaching/retail_small.xlsx` | `e85ee1f817e5da1f20c36432f0f3583aa56e81de23852d3676dfb922c3a2974d` |
