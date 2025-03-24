# Match_DataFrames_V1 - Script de Python

Este script implementa un proceso automatizado para machear o buscar correlación de columnas entre dos DataFrames (`df1` y `df2`) distintos con datos comunes y diferentes nombres de columnas, aplicando múltiples criterios de comparación.

---

## Objetivo de Match_DataFrames_V1

Fusionar información de manera óptima, identificando relaciones entre columnas de diferentes fuentes de datos, como SAP y sistemas externos.

---

##  1. Función Principal: `macheo_columnas_df(df1, df2)`

Para encontrar la mejor correlación entre columnas, el script presenta **4 criterios de macheo**, en el siguiente orden (de mayor a menor calidad):

### 1. Coincidencia entre las modas y similitud en el nombre de la columna
- **Nombre de columna**:
  - Traduce el nombre si hay distintos idiomas.
  - Requiere una similitud ≥ 75%.
- **Modas**:
  - Compara las 20 modas de ambas columnas.
  - Requiere al menos 3 coincidencias exactas.
  - Selecciona la columna con más coincidencias si no hay 20 iguales.

### 2. Coincidencia por modas
- Compara las 20 modas de cada columna.
- Busca al menos 3 coincidencias exactas.
- Devuelve la columna con mayor coincidencia si no hay coincidencia total.

### 3. Coincidencia por valores
- Solo se aplica a columnas de tipo string.
- Compara todos los valores, no solo las modas.
- Si hay al menos 2 valores idénticos, se considera macheo.

### 4. Coincidencia por similitud en el nombre de la columna
- Traduce el nombre si hay distintos idiomas.
- Requiere similitud ≥ 75%.
- Es el criterio menos fiable, requiere validación manual.

---

##  2. Configuración del entorno

- Se establecen rutas dinámicas para trabajar en local o en Microsoft Fabric.
- Se cargan los archivos en `df_original` (CSV) y `df_sap` (Excel).

---

##  3. Preprocesamiento de Datos

- `convertir_a_float_si_posible(columna)`: Convierte strings numéricos a `float`.
- `preprocesar_dataframe(columna, nombre_columna)`: Intenta convertir a `int64`, luego `float64`, y finalmente `string`.
- `estandarizar_texto(texto)`: Elimina acentos, caracteres especiales y reemplaza espacios por guiones bajos.

---

##  4. Creación de Identificadores Únicos

- Se genera la columna `ID_unico_calculado` concatenando valores clave.
- Se reordenan las columnas para colocar el ID al inicio.

---

##  5. Funciones auxiliares

###  Modas
- `similitud(a, b)`
- `obtener_modas(columna, num_modas=20, min_repeticiones_moda=5)`
- `comparar_modas(...)`: Busca coincidencias de modas entre columnas.

###  Valores (columnas string)
- `similitud_valores(a, b)`
- `obtener_modas_valores(columna, num_modas=10, tipo='string')`
- `macheo_por_valores(columna_df1, df2, num_valores=10)`

###  Nombres de columnas
- `estandariza_texto(texto)`
- `similitud_cadenas(a, b)`
- `traducir_nombres_columnas(nombres_columnas)`
- `buscar_similitud_nombres(nombre_columna_df1, df2, traducciones, umbral=0.75)`

---

##  6. Proceso de Macheo

- Se analiza cada columna de `df1` buscando la mejor coincidencia en `df2`.
- Se aplican los 4 criterios en orden.
- Se evita reutilizar columnas de `df2`, salvo que `utilizar_columnas_ya_utilizadas=True`.
- Se genera un reporte detallado.

---

##  7. Resultados Devueltos

1. `df_fusionado`: columnas macheadas.
2. `df_no_correlacionado`: columnas de `df1` sin macheo.
3. `columnas_no_utilizadas`: columnas de `df2` no usadas.
4. `columnas_utilizadas`: columnas de `df2` utilizadas.
5. `columnas_utilizadas_multiples_veces`: columnas de `df2` macheadas varias veces.
6. `macheo_exitoso`: nº de columnas macheadas con éxito.
7. `porcentaje_exito`: ratio de columnas encontradas.
8. `mode_col_name_matching`: coincidencias por moda + nombre.
9. `mode_matching`: coincidencias por moda.
10. `values_matching`: coincidencias por valores.
11. `similarity_matching`: coincidencias por nombres similares.
12. `df_mapeo_fusion`: DataFrame con el mapeo de columnas (`df1` vs `df2`).

---

##  8. Conclusión y Aplicaciones

Este script es una solución robusta para machear columnas entre dos fuentes de datos distintas:

- Identificación de relaciones mediante modas y nombres.
- Preprocesamiento flexible y adaptado.
- Informe completo con métricas de éxito.
- Optimización del proceso de comparación para eficiencia.

 Ideal para proyectos de integración de datos con estructuras heterogéneas.

---
