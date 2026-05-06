# Actividad Semana 9 - Base de Datos

En este documento se presenta todo el proceso solicitado en la actividad de la semana 9.

# Gestor Dbeaver

------------------------------------------------------------------------

# 1Para comenzar, se utilizó la herramienta **Mockaroo** para generar registros de prueba.

Al solicitar los datos, se obtiene un archivo SQL con todos los registros necesarios para cada tabla.

![](images/clipboard-2928949879.png){width="600"}

Como se muestra en la imagen.

------------------------------------------------------------------------

# 2. Consultas avanzadas con operadores

Después de generar los datos, se realizan consultas usando operadores lógicos como:

`=`, `>`, `>=`, `<`, `<=`, `LIKE`, `BETWEEN`

------------------------------------------------------------------------

## 2.1 Operador "=" (Igualdad)

``` sql
SELECT * FROM clients WHERE status = 'Activo';
```

Filtra los clientes cuyo estado sea **Activo**.

![](images/clipboard-2968842538.png){width="560"}

------------------------------------------------------------------------

## 2.2 Operador "\>" (Mayor que)

``` sql
SELECT * FROM sales WHERE total > 100;
```

Muestra las ventas cuyo total es mayor a 100.

![](images/clipboard-2593370976.png){width="531"}

------------------------------------------------------------------------

## 2.3 Operador "\>=" (Mayor o igual)

``` sql
SELECT * FROM sales WHERE total >= 50;
```

Obtiene las ventas con total mayor o igual a 50.

![](images/clipboard-3121602612.png){width="543"}

------------------------------------------------------------------------

## 2.4 Operador "\<" (Menor que)

``` sql
SELECT * FROM sales_details WHERE quantity < 9;
```

Muestra los productos con cantidad menor a 9.

![](images/clipboard-2301704793.png){width="492"}

------------------------------------------------------------------------

## 2.5 Operador "\<=" (Menor o igual)

``` sql
SELECT *  FROM sales_details  WHERE quantity <= 8;
```

Muestra los productos con cantidad menor o igual a 8.

![](images/clipboard-1555395350.png)

------------------------------------------------------------------------

## 2.6 Operador LIKE

``` sql
SELECT * FROM clients WHERE email LIKE 'm%';
```

Busca clientes cuyo correo comienza con la letra "m".

![](images/clipboard-2728769682.png)

------------------------------------------------------------------------

## 2.7 Operador BETWEEN

``` sql
SELECT * FROM sales WHERE sale_date BETWEEN '2025-01-01' AND '2025-12-31';
```

Obtiene las ventas realizadas dentro del año 2025.

![](images/clipboard-2862545811.png)

------------------------------------------------------------------------

## 2.8 Consulta combinada

``` sql
SELECT C.name, V.total FROM clients C JOIN sales V ON C.id = V.id_clients WHERE V.total >= 50 AND V.sale_date BETWEEN '2025-01-01' AND '2026-01-01';
```

Muestra los clientes con ventas mayores o iguales a 50 dentro de un rango de fechas.

![](images/clipboard-1655089596.png)

------------------------------------------------------------------------

## 3.1 Agrupamiento con 1 tabla

``` sql
SELECT status, COUNT(*) AS total_clientes
FROM clients
GROUP BY status;
```

Agrupa los clientes por estado y cuenta cuántos hay en cada grupo.

![](images/clipboard-2778139464.png)

------------------------------------------------------------------------

### Creación de la Vista

``` sql
CREATE OR REPLACE VIEW vista_agrupamiento_1tabla AS
SELECT status, COUNT(*) AS total_clientes
FROM clients
GROUP BY status;
```

![](images/clipboard-90441331.png)Verificación en la sección de vistas:

![](images/clipboard-2060383711.png)

Source:

![](images/clipboard-3890585805.png)

Datos:

![](images/clipboard-1750217931.png)

------------------------------------------------------------------------

### Creación de la Función

``` sql
CREATE OR REPLACE FUNCTION proc_agrupamiento_1tabla()
RETURNS TABLE(status TEXT, total_clientes BIGINT)
LANGUAGE sql
AS $$
    SELECT status, COUNT(*) 
    FROM clients
    GROUP BY status;
$$;
```

![](images/clipboard-2556143058.png)

Verificación:

![](images/clipboard-1019158328.png)

Source:

![](images/clipboard-1729686533.png)

------------------------------------------------------------------------

## 3.2 Agrupamiento con 2 tablas

``` sql
SELECT C.name, COUNT(V.id) AS total_ventas
FROM clients C
JOIN sales V ON C.id = V.id_clients
GROUP BY C.name;
```

![](images/clipboard-2891212611.png)Cuenta cuántas ventas ha realizado cada cliente.

------------------------------------------------------------------------

### Creación de la Vista

``` sql
CREATE OR REPLACE VIEW vista_agrupamiento_2tablas AS
SELECT C.name, COUNT(V.id) AS total_ventas
FROM clients C
JOIN sales V ON C.id = V.id_clients
GROUP BY C.name;
```

![](images/clipboard-3343970709.png)

Verificación:

![](images/clipboard-1707303898.png)

Source:

![](images/clipboard-1367782579.png)

Datos:

![](images/clipboard-2529687017.png)

------------------------------------------------------------------------

### Creación de la Función

``` sql
CREATE OR REPLACE FUNCTION proc_agrupamiento_2tablas()
RETURNS TABLE(name TEXT, total_ventas BIGINT)
LANGUAGE sql
AS $$
    SELECT C.name, COUNT(V.id)
    FROM clients C
    JOIN sales V ON C.id = V.id_clients
    GROUP BY C.name;
$$;
```

![](images/clipboard-1096336802.png)Verificación:

![](images/clipboard-2566265486.png)

Source:

![](images/clipboard-1541692100.png)

------------------------------------------------------------------------

## 3.3 Agrupamiento con 3 tablas

``` sql
SELECT C.name AS cliente, P.name AS producto, SUM(PV.quantity) AS total_comprado
FROM clients C
JOIN sales V ON C.id = V.id_clients
JOIN sales_details PV ON V.id = PV.id_sales
JOIN products P ON P.id = PV.id_products
GROUP BY C.name, P.name;
```

![](images/clipboard-2350007754.png)Muestra cuánto ha comprado cada cliente de cada producto.

------------------------------------------------------------------------

### Creación de la Vista

``` sql
CREATE OR REPLACE VIEW vista_agrupamiento_3tablas AS
SELECT C.name AS cliente, P.name AS producto, SUM(PV.quantity) AS total_comprado
FROM clients C
JOIN sales V ON C.id = V.id_clients
JOIN sales_details PV ON V.id = PV.id_sales
JOIN products P ON P.id = PV.id_products
GROUP BY C.name, P.name;
```

![](images/clipboard-193913543.png)Verificación:

![](images/clipboard-3501893867.png)

Source:

![](images/clipboard-1582056451.png)

Datos:

![](images/clipboard-317315740.png)

------------------------------------------------------------------------

### Creación de la Función

``` sql
CREATE OR REPLACE FUNCTION proc_agrupamiento_3tablas()
RETURNS TABLE(cliente TEXT, producto TEXT, total_comprado BIGINT)
LANGUAGE sql
AS $$
    SELECT C.name, P.name, SUM(PV.quantity)
    FROM clients C
    JOIN sales V ON C.id = V.id_clients
    JOIN sales_details PV ON V.id = PV.id_sales
    JOIN products P ON P.id = PV.id_products
    GROUP BY C.name, P.name;
$$;
```

![](images/clipboard-1416322097.png)Verificación:

![](images/clipboard-3825063654.png)

Source:

![](images/clipboard-149942875.png)

------------------------------------------------------------------------

# 4. Subconsultas

------------------------------------------------------------------------

## 4.1 Subconsulta con 1 tabla

``` sql
SELECT *
FROM clients
WHERE id IN (
    SELECT id_clients
    FROM sales
);
```

![](images/clipboard-1263239432.png)Muestra los clientes que han realizado al menos una venta.

------------------------------------------------------------------------

### Vista

``` sql
CREATE OR REPLACE VIEW vista_subconsulta_1tabla AS
SELECT *
FROM clients
WHERE id IN (
    SELECT id_clients
    FROM sales
);
```

![](images/clipboard-398579269.png)

verificacion:

![](images/clipboard-89241317.png)

source:

![](images/clipboard-568538376.png)

------------------------------------------------------------------------

Datos:

![](images/clipboard-2965680602.png)

### Función

``` sql
CREATE OR REPLACE FUNCTION proc_subconsulta_1tabla()
RETURNS SETOF clients
LANGUAGE sql
AS $$
    SELECT *
    FROM clients
    WHERE id IN (
        SELECT id_clients
        FROM sales
    );
$$;
```

![](images/clipboard-2347508977.png)

Verificacion:

![](images/clipboard-4057672937.png)

Source:![](images/clipboard-2929864293.png)

------------------------------------------------------------------------

## 4.2 Subconsulta con 2 tablas

``` sql
SELECT *
FROM clients
WHERE id IN (
    SELECT id_clients
    FROM sales
    WHERE total > 100
);
```

![](images/clipboard-2820542095.png)

------------------------------------------------------------------------

### Vista

``` sql
CREATE OR REPLACE VIEW vista_subconsulta_2tablas AS
SELECT *
FROM clients
WHERE id IN (
    SELECT id_clients
    FROM sales
    WHERE total > 100
);
```

![](images/clipboard-2238935789.png)

------------------------------------------------------------------------

Verificamos:

![](images/clipboard-335914057.png)

Source:

![](images/clipboard-2912267530.png)

Datos:

![](images/clipboard-912587090.png)

### Función

``` sql
CREATE OR REPLACE FUNCTION proc_subconsulta_2tablas()
RETURNS SETOF clients
LANGUAGE sql
AS $$
    SELECT *
    FROM clients
    WHERE id IN (
        SELECT id_clients
        FROM sales
        WHERE total > 100
    );
$$;
```

![](images/clipboard-206937751.png)

Verificacion:

![](images/clipboard-935937920.png)

Source:

![](images/clipboard-1643960598.png)

------------------------------------------------------------------------

## 4.3 Subconsulta con 3 tablas

``` sql
SELECT *
FROM clients
WHERE id IN (
    SELECT V.id_clients
    FROM sales V
    JOIN sales_details PV ON V.id = PV.id_sales
    WHERE PV.quantity >= 5
);
```

![](images/clipboard-1809115802.png)

------------------------------------------------------------------------

### Vista

``` sql
CREATE OR REPLACE VIEW vista_subconsulta_3tablas AS
SELECT *
FROM clients
WHERE id IN (
    SELECT V.id_clients
    FROM sales V
    JOIN sales_details PV ON V.id = PV.id_sales
    WHERE PV.quantity >= 5
);
```

![](images/clipboard-1924784438.png)

------------------------------------------------------------------------

Definicion:

![](images/clipboard-1058933399.png)

Source:

![](images/clipboard-2470016570.png)

Datos:

![](images/clipboard-3165056325.png)

### Función

``` sql
CREATE OR REPLACE FUNCTION proc_subconsulta_3tablas()
RETURNS SETOF clients
LANGUAGE sql
AS $$
    SELECT *
    FROM clients
    WHERE id IN (
        SELECT V.id_clients
        FROM sales V
        JOIN sales_details PV ON V.id = PV.id_sales
        WHERE PV.quantity >= 5
    );
$$;
```

![](images/clipboard-2530377179.png)

------------------------------------------------------------------------

Verificacion:

\+![](images/clipboard-1969254300.png)

Source:

![](images/clipboard-852751801.png)

Se completó la actividad utilizando PostgreSQL mediante los gestores DBeaver aplicando consultas avanzadas, agrupamientos, subconsultas, vistas y funciones correctamente.

# Gestor PgAdmin4

------------------------------------------------------------------------

# 1Para comenzar, se utilizó la herramienta **Mockaroo** para generar registros de prueba.

Al solicitar los datos, se obtiene un archivo SQL con todos los registros necesarios para cada tabla.

![](images/clipboard-2928949879.png){width="600"}

Como se muestra en la imagen.

------------------------------------------------------------------------

# 2. Consultas avanzadas con operadores

Después de generar los datos, se realizan consultas usando operadores lógicos como:

`=`, `>`, `>=`, `<`, `<=`, `LIKE`, `BETWEEN`

------------------------------------------------------------------------

## 2.1 Operador "=" (Igualdad)

``` sql
SELECT * FROM clients WHERE status = 'Activo';
```

Filtra los clientes cuyo estado sea **Activo**.

![](images/clipboard-326712240.png)

------------------------------------------------------------------------

## 2.2 Operador "\>" (Mayor que)

``` sql
SELECT * FROM sales WHERE total > 100;
```

Muestra las ventas cuyo total es mayor a 100.

![](images/clipboard-3151700683.png)

------------------------------------------------------------------------

## 2.3 Operador "\>=" (Mayor o igual)

``` sql
SELECT * FROM sales WHERE total >= 50;
```

Obtiene las ventas con total mayor o igual a 50.

![](images/clipboard-3151700683.png)

------------------------------------------------------------------------

## 2.4 Operador "\<" (Menor que)

``` sql
SELECT * FROM sales_details WHERE quantity < 9;
```

Muestra los productos con cantidad menor a 9.

![](images/clipboard-2551917366.png)

------------------------------------------------------------------------

## 2.5 Operador "\<=" (Menor o igual)

``` sql
SELECT *  FROM sales_details  WHERE quantity <= 8;
```

Muestra los productos con cantidad menor o igual a 8.![](images/clipboard-3542394955.png)

------------------------------------------------------------------------

## 2.6 Operador LIKE

``` sql
SELECT * FROM clients WHERE email LIKE 'm%';
```

Busca clientes cuyo correo comienza con la letra "m".![](images/clipboard-1633197351.png)

------------------------------------------------------------------------

## 2.7 Operador BETWEEN

``` sql
SELECT * FROM sales WHERE sale_date BETWEEN '2025-01-01' AND '2025-12-31';
```

Obtiene las ventas realizadas dentro del año 2025.

![](images/clipboard-2211223513.png)

------------------------------------------------------------------------

## 2.8 Consulta combinada

``` sql
SELECT C.name, V.total FROM clients C JOIN sales V ON C.id = V.id_clients WHERE V.total >= 50 AND V.sale_date BETWEEN '2025-01-01' AND '2026-01-01';
```

Muestra los clientes con ventas mayores o iguales a 50 dentro de un rango de fechas.![](images/clipboard-1370498044.png)

------------------------------------------------------------------------

## 3.1 Agrupamiento con 1 tabla

``` sql
SELECT status, COUNT(*) AS total_clientes FROM clients GROUP BY status;
```

Agrupa los clientes por estado y cuenta cuántos hay en cada grupo.![](images/clipboard-2940625333.png)

------------------------------------------------------------------------

### Creación de la Vista

``` sql
CREATE OR REPLACE VIEW vista_agrupamiento_1tabla AS SELECT status, COUNT(*) AS total_clientes FROM clients GROUP BY status;
```

![](images/clipboard-4108451195.png)Verificación en la sección de vistas:

------------------------------------------------------------------------

### Creación de la Función

``` sql
CREATE OR REPLACE FUNCTION proc_agrupamiento_1tabla() RETURNS TABLE(status TEXT, total_clientes BIGINT) LANGUAGE sql AS $$     SELECT status, COUNT(*)      FROM clients     GROUP BY status; $$;
```

![](images/clipboard-145047990.png)

------------------------------------------------------------------------

## 3.2 Agrupamiento con 2 tablas

``` sql
SELECT C.name, COUNT(V.id) AS total_ventas FROM clients C JOIN sales V ON C.id = V.id_clients GROUP BY C.name;
```

![](images/clipboard-4029838434.png)Cuenta cuántas ventas ha realizado cada cliente.

------------------------------------------------------------------------

### Creación de la Vista

``` sql
CREATE OR REPLACE VIEW vista_agrupamiento_2tablas AS SELECT C.name, COUNT(V.id) AS total_ventas FROM clients C JOIN sales V ON C.id = V.id_clients GROUP BY C.name;
```

![](images/clipboard-1640706020.png)

------------------------------------------------------------------------

### Creación de la Función

``` sql
CREATE OR REPLACE FUNCTION proc_agrupamiento_2tablas() RETURNS TABLE(name TEXT, total_ventas BIGINT) LANGUAGE sql AS $$     SELECT C.name, COUNT(V.id)     FROM clients C     JOIN sales V ON C.id = V.id_clients     GROUP BY C.name; $$;
```

![](images/clipboard-1226412031.png)

------------------------------------------------------------------------

## 3.3 Agrupamiento con 3 tablas

``` sql
SELECT C.name AS cliente, P.name AS producto, SUM(PV.quantity) AS total_comprado FROM clients C JOIN sales V ON C.id = V.id_clients JOIN sales_details PV ON V.id = PV.id_sales JOIN products P ON P.id = PV.id_products GROUP BY C.name, P.name;
```

![](images/clipboard-2834276960.png)Muestra cuánto ha comprado cada cliente de cada producto.

------------------------------------------------------------------------

### Creación de la Vista

``` sql
CREATE OR REPLACE VIEW vista_agrupamiento_3tablas AS SELECT C.name AS cliente, P.name AS producto, SUM(PV.quantity) AS total_comprado FROM clients C JOIN sales V ON C.id = V.id_clients JOIN sales_details PV ON V.id = PV.id_sales JOIN products P ON P.id = PV.id_products GROUP BY C.name, P.name;
```

![](images/clipboard-1744704724.png)

------------------------------------------------------------------------

### Creación de la Función

``` sql
CREATE OR REPLACE FUNCTION proc_agrupamiento_3tablas() RETURNS TABLE(cliente TEXT, producto TEXT, total_comprado BIGINT) LANGUAGE sql AS $$     SELECT C.name, P.name, SUM(PV.quantity)     FROM clients C     JOIN sales V ON C.id = V.id_clients     JOIN sales_details PV ON V.id = PV.id_sales     JOIN products P ON P.id = PV.id_products     GROUP BY C.name, P.name; $$;
```

![](images/clipboard-3823485530.png)

------------------------------------------------------------------------

# 4. Subconsultas

------------------------------------------------------------------------

## 4.1 Subconsulta con 1 tabla

``` sql
SELECT * FROM clients WHERE id IN (     SELECT id_clients     FROM sales );
```

![](images/clipboard-3354151249.png)Muestra los clientes que han realizado al menos una venta.

------------------------------------------------------------------------

### Vista

``` sql
CREATE OR REPLACE VIEW vista_subconsulta_1tabla AS SELECT * FROM clients WHERE id IN (     SELECT id_clients     FROM sales );
```

![](images/clipboard-3775144351.png)

### Función

``` sql
CREATE OR REPLACE FUNCTION proc_subconsulta_1tabla() RETURNS SETOF clients LANGUAGE sql AS $$     SELECT *     FROM clients     WHERE id IN (         SELECT id_clients         FROM sales     ); $$;
```

![](images/clipboard-3409149884.png)

## 4.2 Subconsulta con 2 tablas

``` sql
SELECT * FROM clients WHERE id IN (     SELECT id_clients     FROM sales     WHERE total > 100 );
```

![](images/clipboard-2616337128.png)

------------------------------------------------------------------------

### Vista

``` sql
CREATE OR REPLACE VIEW vista_subconsulta_2tablas AS SELECT * FROM clients WHERE id IN (     SELECT id_clients     FROM sales     WHERE total > 100 );
```

![](images/clipboard-1642923806.png)

### Función

``` sql
CREATE OR REPLACE FUNCTION proc_subconsulta_2tablas() RETURNS SETOF clients LANGUAGE sql AS $$     SELECT *     FROM clients     WHERE id IN (         SELECT id_clients         FROM sales         WHERE total > 100     ); $$;
```

![](images/clipboard-1058176365.png)

## 4.3 Subconsulta con 3 tablas

``` sql
SELECT * FROM clients WHERE id IN (     SELECT V.id_clients     FROM sales V     JOIN sales_details PV ON V.id = PV.id_sales     WHERE PV.quantity >= 5 );
```

![](images/clipboard-3271535953.png)

------------------------------------------------------------------------

### Vista

``` sql
CREATE OR REPLACE VIEW vista_subconsulta_3tablas AS SELECT * FROM clients WHERE id IN (     SELECT V.id_clients     FROM sales V     JOIN sales_details PV ON V.id = PV.id_sales     WHERE PV.quantity >= 5 );
```

![](images/clipboard-628507180.png)

### Función

``` sql
CREATE OR REPLACE FUNCTION proc_subconsulta_3tablas() RETURNS SETOF clients LANGUAGE sql AS $$     SELECT *     FROM clients     WHERE id IN (         SELECT V.id_clients         FROM sales V         JOIN sales_details PV ON V.id = PV.id_sales         WHERE PV.quantity >= 5     ); $$;
```

![](images/clipboard-3983832029.png)

Se completó la actividad utilizando PostgreSQL mediante El Gestor pg admin
