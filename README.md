# Datos del curso MDS102

Programación, SQL y Analítica Reproducible · Maestría en Data Science · Universidad Nacional de Ingeniería

## Fuente

Chen, D. (2015). *Online Retail* [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5BW33. Licencia CC BY 4.0.

Ventas de una tienda en línea del Reino Unido entre el 1 de diciembre de 2010 y el 9 de diciembre de 2011.

## Archivos

| Archivo | Filas | Una fila es… |
|---|---|---|
| `prepared/transactions.csv` | 541 909 | una línea de venta del archivo original |
| `prepared/transactions.parquet` | 541 909 | el mismo contenido, en formato Parquet |
| `prepared/customers.csv` | 4 372 | un cliente identificado |
| `prepared/products.csv` | 4 070 | un código de producto |
| `teaching/retail_small.csv` | 3 108 | una línea de venta del 1 de diciembre de 2010 |
| `teaching/retail_small.xlsx` | 3 108 | el mismo contenido, en Excel |
| `teaching/retail_dirty.csv` | 22 312 | una línea de venta de 1 000 facturas reales, con problemas de calidad |
| `teaching/retail_clean_reference.csv` | 21 990 | el resultado esperado de limpiar `retail_dirty` |
| `teaching/retail_dirty_log.csv` | 3 266 | una alteración pedagógica introducida en `retail_dirty` |

## Columnas

### `transactions`

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

### `customers`

| Columna | Significado |
|---|---|
| `CustomerID` | código del cliente (uno por fila) |
| `Country` | país con más líneas del cliente; 8 clientes compraron desde dos países |
| `Region` | región del país, asignada para el curso |
| `FirstPurchase` | fecha de la primera línea con cantidad positiva (`AAAA-MM-DD`); vacía en los 33 clientes que solo tienen devoluciones |

### `products`

| Columna | Significado |
|---|---|
| `StockCode` | código del producto (uno por fila) |
| `Description` | descripción más frecuente del código; vacía en los 112 códigos sin ninguna descripción |
| `Category` | `producto`; `envío` (`POST`, `DOT`, `C2`); o `ajuste` (`M`, `m`, `D`, `B`, `BANK CHARGES`, `AMAZONFEE`, `CRUK`, `S`) |

## Transformaciones aplicadas

- `transactions` contiene las mismas filas, columnas y valores que el archivo original `Online Retail.xlsx`, sin limpieza ni filtros. `InvoiceDate` se escribe en formato `AAAA-MM-DD hh:mm:ss`, y una descripción numérica (producto `20713`) se guarda como texto.
- `customers` y `products` se derivan de `transactions`, con una fila por clave.
- `retail_small` contiene solo las líneas con fecha 1 de diciembre de 2010.
- Se conservan las facturas canceladas, las cantidades negativas, los precios cero o negativos y los valores faltantes del original.

## `retail_dirty`: datos para la limpieza

`retail_dirty.csv` contiene todas las líneas de 1 000 facturas reales de `transactions` (22 257 líneas), elegidas con la semilla 102. La selección incluye a propósito facturas con los problemas reales poco frecuentes del dataset. La columna `LineID` indica la posición de cada línea en `transactions.csv`.

**Problemas reales del dataset original (no alterados):** líneas sin `CustomerID`, descripciones faltantes o escritas como `?`, duplicados exactos, facturas canceladas, cantidades negativas en facturas no canceladas (ajustes de inventario), precios cero o negativos, códigos que no son productos y códigos que difieren solo en mayúsculas.

**Alteraciones pedagógicas** (registradas fila por fila en `retail_dirty_log.csv`, con su valor original):

| Alteración | Filas |
|---|---|
| fecha en formato `DD/MM/AAAA hh:mm` | 668 |
| precio con coma decimal (`2,55`) | 443 |
| descripción con espacios extra | 443 |
| variante del nombre de un país (`UK`, `france`, `Ireland`…) | 430 |
| cantidad con la unidad escrita (`12 uds`) | 334 |
| código de producto con espacio al final | 223 |
| descripción en minúscula | 217 |
| precio con símbolo de libra (`£3.75`) | 217 |
| cliente faltante disfrazado (`?`, `SIN DATO`, `N/A`), siempre en facturas cuyo cliente se puede recuperar | 211 |
| duplicado oculto: copia de una línea con la descripción alterada | 40 |
| fecha inválida (`31/04/2011`, `sin fecha`…), en facturas con otras líneas de la misma fecha | 25 |
| fila completamente vacía | 15 |
| nombres de columna con espacios o formato distinto | 4 columnas |

`retail_clean_reference.csv` es el resultado de la limpieza documentada en el Colab de la semana 6. Se verificó que recupera los valores originales de `transactions` en todas las líneas conservadas; las únicas diferencias son las estandarizaciones (texto sin espacios y en mayúscula, códigos en mayúscula), las 307 líneas duplicadas eliminadas y las columnas agregadas (`SinCliente`, `Importe` sin redondear, `EsCancelada`, `EsProducto`, `EsDevolucion` —cantidad negativa en factura cancelada—, `EsAjusteInventario` —cantidad negativa en factura no cancelada— y `EsVenta` —cantidad y precio positivos en un código de producto—).

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
| `prepared/customers.csv` | `d93085cf95bf7b74f8c94e88971ecc217f06ebb2b0f7d8b2106ae63f2d566978` |
| `prepared/products.csv` | `198d9bb25a8f5c28b107040678e7d7b9b89ab3937f41f7045c43f9991de12c12` |
| `teaching/retail_small.csv` | `45ca8842daf556b96947109ad92d666391410a2a3e894bab7644773d1ff539b3` |
| `teaching/retail_small.xlsx` | `e85ee1f817e5da1f20c36432f0f3583aa56e81de23852d3676dfb922c3a2974d` |
| `teaching/retail_dirty.csv` | `c0036b4ae2c1da1621cb16cfe5bf29e931888572216465459ca3fda34d829e38` |
| `teaching/retail_clean_reference.csv` | `6fadb3682a1b6b661fb99febf8150b1e5bfe5c92a8fae05ba8d4937195e8a19b` |
| `teaching/retail_dirty_log.csv` | `ba8d138499995dde6a442e375e73e7e3a19851fd65e5ca8d2fcc34e4cd3f5a23` |
