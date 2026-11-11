# Clasificación de cadenas comerciales

Clasificador por reglas de palabras clave para un catálogo de locales comerciales de Chile y
México. Asigna a cada local una categoría principal y las secundarias que correspondan, deja
escrito con qué criterio se tomó cada decisión, y escribe el resultado en una tabla de control
con clave primaria por local.

Escrito en junio de 2024, durante mi trabajo como analista de datos.

**Se publica como registro de un trabajo hecho, no como una herramienta ejecutable.** Los datos y
la conexión eran de la empresa y no se publican — ver *Por qué no corre*.

## El problema

Un catálogo de cientos de miles de locales, con el nombre escrito por quien lo cargó: mayúsculas
irregulares, acentos ausentes, abreviaturas, y el rubro unas veces en el nombre y otras no. La
pregunta del negocio era de qué tipo es cada local. No existía esa etiqueta.

## Cómo funciona

**Normalización primero.** Los nombres bajan a minúscula, pierden acentos y signos, y se
estandarizan los separadores. Sin ese paso, «Panadería San José» y «PANADERIA SAN JOSE» son dos
locales distintos.

**Tres formas de coincidir, no una.** Una palabra clave puede exigirse como palabra entera, como
fracción al inicio del nombre o como fracción en cualquier posición. La distinción es la que hace
utilizable el método: «pan» al inicio de «pan de campo» clasifica, pero «pan» dentro de
«panorama» no.

**Categoría principal y secundarias.** Un local puede caer legítimamente en varias —un sushi que
además vende pescados y mariscos—. Se elige una principal por especificidad y el resto quedan
registradas como secundarias, no descartadas.

**El criterio queda escrito.** Cada fila guarda con qué palabra clave y con qué forma de
coincidencia se clasificó. Sin eso, revisar una clasificación dudosa obliga a reconstruir el
razonamiento a mano, y nadie lo hace.

**Salida.** Conteos por categoría y país para dimensionar la cobertura, y la tabla de control con
clave primaria por local, para poder reprocesar sin duplicar.

## Por qué no corre

El repositorio **no incluye datos ni conexión**:

- Las tablas de origen —el catálogo de locales y la tabla de palabras clave— eran de la empresa
  donde trabajé. No se publican.
- La cadena de conexión se reemplazó por variables de entorno.
- Las planillas de resultados, la presentación de cierre y las capturas de los dataframes se
  retiraron por la misma razón.

Queda el código. Para ejecutarlo haría falta aportar un catálogo propio con las columnas que el
notebook espera y una tabla de palabras clave con su tipo de coincidencia.

## Dependencias

Python con `polars`, `pandas`, `matplotlib`, `seaborn`, `psycopg2`, `unidecode` y `openpyxl`.

Las variables de entorno que espera la conexión son `DB_NAME`, `DB_USER`, `DB_PASSWORD`, `DB_HOST`
y `DB_PORT`.
