------------------------------------------------------------------------

#  Actividad Semana 9 - Base de Datos

En este documento se presenta todo el proceso solicitado en la actividad de la semana 9.

------------------------------------------------------------------------

#  1. Generación de datos con Mockaroo

Para comenzar, se utilizó la herramienta **Mockaroo** para generar registros de prueba.

Al solicitar los datos, se obtiene un archivo SQL con todos los registros necesarios para cada tabla.

![](images/clipboard-2928949879.png){width="600"}

Como se muestra en la imagen.

------------------------------------------------------------------------

#  2. Consultas avanzadas con operadores

Después de generar los datos, se realizan consultas usando operadores lógicos como:

`=`, `>`, `>=`, `<`, `<=`, `LIKE`, `BETWEEN`

------------------------------------------------------------------------

## 2.1 Operador "=" (Igualdad)

``` sql
SELECT *
FROM clients
WHERE status = 'Activo';
```

Filtra los clientes cuyo estado sea **Activo**.

![](images/clipboard-2968842538.png){width="560"}

------------------------------------------------------------------------

## 2.2 Operador "\>" (Mayor que)

``` sql
SELECT *
FROM sales
WHERE total > 100;
```

Muestra las ventas cuyo total es mayor a 100.

![](images/clipboard-2593370976.png){width="531"}

------------------------------------------------------------------------

## 2.3 Operador "\>=" (Mayor o igual)

``` sql
SELECT *
FROM sales
WHERE total >= 50;
```

Obtiene las ventas con total mayor o igual a 50.

![](images/clipboard-3121602612.png){width="543"}

------------------------------------------------------------------------

## 2.4 Operador "\<" (Menor que)

``` sql
SELECT *
FROM sales_details
WHERE quantity < 9;
```

Muestra los productos con cantidad menor a 9.

![](images/clipboard-2301704793.png){width="492"}

------------------------------------------------------------------------

## 2.5 Operador "\<=" (Menor o igual)

``` sql
SELECT * 
FROM sales_details 
WHERE quantity <= 8;
```

Muestra los productos con cantidad menor o igual a 8.

![](images/clipboard-1555395350.png)

------------------------------------------------------------------------

## 2.6 Operador LIKE

``` sql
SELECT *
FROM clients
WHERE email LIKE 'm%';
```

Busca clientes cuyo correo comienza con la letra "m".

![](images/clipboard-2728769682.png)

------------------------------------------------------------------------

## 2.7 Operador BETWEEN

``` sql
SELECT *
FROM sales
WHERE sale_date BETWEEN '2025-01-01' AND '2025-12-31';
```

Obtiene las ventas realizadas dentro del año 2025.

![](images/clipboard-2862545811.png)

------------------------------------------------------------------------

## 2.8 Consulta combinada

``` sql
SELECT C.name, V.total
FROM clients C
JOIN sales V ON C.id = V.id_clients
WHERE V.total >= 50
AND V.sale_date BETWEEN '2025-01-01' AND '2026-01-01';
```

Muestra los clientes con ventas mayores o iguales a 50 dentro de un rango de fechas.

![](images/clipboard-1655089596.png)

------------------------------------------------------------------------

# 3. Consultas de agrupamiento

------------------------------------------------------------------------

## 3.1 Agrupamiento con 1 tabla

``` sql
SELECT status, COUNT(*) AS total_clientes
FROM clients
GROUP BY status;
```

Agrupa los clientes por estado y cuenta cuántos hay en cada grupo.

![](images/clipboard-2537964063.png)

------------------------------------------------------------------------

###  Creación de la Vista

``` sql
CREATE VIEW vista_agrupamiento_1tabla AS
SELECT status, COUNT(*) AS total_clientes
FROM clients
GROUP BY status;
```

![](images/clipboard-2507283680.png)

Verificación en la sección de vistas:

![](images/clipboard-2352191906.png){width="257"}

Source:

![](images/clipboard-680676448.png){width="261"}

Datos:

![](images/clipboard-3670222771.png){width="264"}

------------------------------------------------------------------------

###  Creación del Procedimiento

``` sql
CREATE PROCEDURE proc_agrupamiento_1tabla()
BEGIN
    SELECT status, COUNT(*) AS total_clientes
    FROM clients
    GROUP BY status;
END;
```

![](images/clipboard-3546340718.png)

Verificación:

![](images/clipboard-2326766343.png){width="247"}

Source:

![](images/clipboard-2398801967.png){width="248"}

------------------------------------------------------------------------

## 3.2 Agrupamiento con 2 tablas

``` sql
SELECT C.name, COUNT(V.id) AS total_ventas
FROM clients C
JOIN sales V ON C.id = V.id_clients
GROUP BY C.name;
```

Cuenta cuántas ventas ha realizado cada cliente.

![](images/clipboard-3376786870.png)

------------------------------------------------------------------------

###  Creación de la Vista

``` sql
CREATE VIEW vista_agrupamiento_1tabla AS
SELECT status, COUNT(*) AS total_clientes
FROM clients
GROUP BY status;
```

![](images/clipboard-3016062368.png)

Verificación:

![](images/clipboard-1564425218.png){width="258"}

Source:

![](images/clipboard-1808644884.png){width="259"}

Datos:

![](images/clipboard-662863311.png){width="277"}

------------------------------------------------------------------------

###  Creación del Procedimiento

``` sql
CREATE PROCEDURE proc_agrupamiento_2tablas()
BEGIN
    SELECT C.name, COUNT(V.id) AS total_ventas
    FROM clients C
    JOIN sales V ON C.id = V.id_clients
    GROUP BY C.name;
END;
```

![](images/clipboard-1606761876.png){width="477"}

Verificación:

![](images/clipboard-3942903105.png){width="265"}

Source:

![](images/clipboard-409484162.png){width="276"}

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

Muestra cuánto ha comprado cada cliente de cada producto.

![](images/clipboard-1814908887.png)

------------------------------------------------------------------------

###  Creación de la Vista

``` sql
CREATE VIEW vista_agrupamiento_3tablas AS
SELECT C.name AS cliente, P.name AS producto, SUM(PV.quantity) AS total_comprado
FROM clients C
JOIN sales V ON C.id = V.id_clients
JOIN sales_details PV ON V.id = PV.id_sales
JOIN products P ON P.id = PV.id_products
GROUP BY C.name, P.name;
```

![](images/clipboard-325409901.png)

Verificación:

![](images/clipboard-2175561479.png){width="249"}

Source:

![](images/clipboard-3504277851.png){width="256"}

Datos:

![](images/clipboard-3877995662.png){width="257"}

------------------------------------------------------------------------

###  Creación del Procedimiento

``` sql
CREATE PROCEDURE proc_agrupamiento_3tablas()
BEGIN
    SELECT C.name AS cliente, P.name AS producto, SUM(PV.quantity) AS total_comprado
    FROM clients C
    JOIN sales V ON C.id = V.id_clients
    JOIN sales_details PV ON V.id = PV.id_sales
    JOIN products P ON P.id = PV.id_products
    GROUP BY C.name, P.name;
END;
```

![](images/clipboard-3532691589.png)

Verificación:

![](images/clipboard-4056144819.png){width="310"}

Source:

![](images/clipboard-3173758032.png){width="330"}

Aqui Empezaremos con las subconsultas

Utilizamos este script

### subconsultas con 1 tabla

``` sql
CREATE PROCEDURE proc_agrupamiento_3tablas() BEGIN     SELECT C.name AS cliente, P.name AS producto, SUM(PV.quantity) AS total_comprado     FROM clients C     JOIN sales V ON C.id = V.id_clients     JOIN sales_details PV ON V.id = PV.id_sales     JOIN products P ON P.id = PV.id_products     GROUP BY C.name, P.name; END;
```

------------------------------------------------------------------------

Muestra los clientes que **han realizado al menos una venta**.

![](images/clipboard-3812621531.png)

### Creamos las vistas

``` sql
CREATE VIEW vista_subconsulta_1tabla AS
SELECT *
FROM clients
WHERE id IN (
    SELECT id_clients
    FROM sales
);
```

------------------------------------------------------------------------

![](images/clipboard-690320178.png)

Verificamos:

![](images/clipboard-424494512.png){width="282"}

Source:

![](images/clipboard-1467082597.png){width="291"}

Datos:

![](images/clipboard-854849806.png){width="338"}

### Creamos los procedimientos

``` sql
CREATE PROCEDURE proc_subconsulta_1tabla()
BEGIN
    SELECT *
    FROM clients
    WHERE id IN (
        SELECT id_clients
        FROM sales
    );
END;
```

![](images/clipboard-411206826.png)

Verificamos:

![](images/clipboard-3174014046.png){width="301"}

Source:

![](images/clipboard-2241894667.png){width="349"}

### subconsultas con 2 tabla

``` sql
SELECT *
FROM clients
WHERE id IN (
    SELECT id_clients
    FROM sales
    WHERE total > 100
);
```

Muestra los clientes que han realizado **ventas mayores a 100**.

------------------------------------------------------------------------

![](images/clipboard-2268236369.png)

### Creamos las vistas

``` sql
CREATE VIEW vista_subconsulta_2tablas AS
SELECT *
FROM clients
WHERE id IN (
    SELECT id_clients
    FROM sales
    WHERE total > 100
);
```

------------------------------------------------------------------------

![](images/clipboard-1258616950.png)

Verificamos:

![](images/clipboard-2544002067.png){width="286"}

Source:

![](images/clipboard-2691658178.png){width="312"}

Datos:

![](images/clipboard-2532632692.png){width="368"}

### Creamos los procedimientos

``` sql
CREATE PROCEDURE proc_subconsulta_2tablas()
BEGIN
    SELECT *
    FROM clients
    WHERE id IN (
        SELECT id_clients
        FROM sales
        WHERE total > 100
    );
END;
```

![Verificamos:](images/clipboard-556676317.png)

![](images/clipboard-144935683.png){width="286"}

Source:

![](images/clipboard-1206127491.png){width="345"}

### subconsultas con 3 tabla

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

Muestra los clientes que han comprado productos con cantidad mayor o igual a 5.

![](images/clipboard-2427047901.png)

### Creamos las vistas

``` sql
CREATE VIEW vista_subconsulta_3tablas AS
SELECT *
FROM clients
WHERE id IN (
    SELECT V.id_clients
    FROM sales V
    JOIN sales_details PV ON V.id = PV.id_sales
    WHERE PV.quantity >= 5
);
```

![](images/clipboard-3570458528.png)

Verificamos:

![](images/clipboard-4220241333.png)

Source:

![](images/clipboard-458618288.png){width="354"}

Datos:

![](images/clipboard-1056124973.png)

### Creamos los procedimientos

``` sql
CREATE PROCEDURE proc_subconsulta_3tablas()
BEGIN
    SELECT *
    FROM clients
    WHERE id IN (
        SELECT V.id_clients
        FROM sales V
        JOIN sales_details PV ON V.id = PV.id_sales
        WHERE PV.quantity >= 5
    );
END;
```

![](images/clipboard-1138720722.png)

Verificamos:

![](images/clipboard-391580128.png){width="321"}

Source:

![](images/clipboard-2843406045.png){width="352"}

Aqui Concluimos la actividad Terminada por medio de Dbeaver con el motor MySQL
