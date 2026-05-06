# 📊 Actividad Semana 9 - Base de Datos

En este documento se presenta todo el proceso solicitado en la actividad de la semana 9.

# Gestor Dbeaver

------------------------------------------------------------------------

# 

# 1. Generación de datos con Mockaroo

Para comenzar, se utilizó la herramienta **Mockaroo** para generar registros de prueba.

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

------------------------------------------------------------------------

#  3. Consultas de agrupamiento

------------------------------------------------------------------------

## 3.1 Agrupamiento con 1 tabla

``` sql
SELECT status, COUNT(*) AS total_clientes
FROM clients
GROUP BY status;
```

![](images/clipboard-1907306683.png)Agrupa los clientes por estado y cuenta cuántos hay en cada grupo.

------------------------------------------------------------------------

### 🔹 Creación de la Vista

``` sql
IF OBJECT_ID('vista_agrupamiento_1tabla', 'V') IS NOT NULL
DROP VIEW vista_agrupamiento_1tabla;
GO

CREATE VIEW vista_agrupamiento_1tabla AS
SELECT status, COUNT(*) AS total_clientes
FROM clients
GROUP BY status;
GO
```

![](images/clipboard-1183197370.png)Verificación en la sección de vistas:

![](images/clipboard-1063879486.png)

Source:

![](images/clipboard-4286104761.png)

Datos:

![](images/clipboard-3293825421.png)

------------------------------------------------------------------------

### ⚙️ Creación del Procedimiento

``` sql
IF OBJECT_ID('proc_agrupamiento_1tabla', 'P') IS NOT NULL
DROP PROCEDURE proc_agrupamiento_1tabla;
GO

CREATE PROCEDURE proc_agrupamiento_1tabla
AS
BEGIN
    SELECT status, COUNT(*) AS total_clientes
    FROM clients
    GROUP BY status;
END;
GO
```

![](images/clipboard-1080830646.png)Verificación:

![](images/clipboard-3785780487.png)

Source:

![](images/clipboard-2621916471.png)

------------------------------------------------------------------------

## 3.2 Agrupamiento con 2 tablas

``` sql
SELECT C.name, COUNT(V.id) AS total_ventas
FROM clients C
JOIN sales V ON C.id = V.id_clients
GROUP BY C.name;
```

Cuenta cuántas ventas ha realizado cada cliente.

------------------------------------------------------------------------

### 🔹 Creación de la Vista

``` sql
IF OBJECT_ID('vista_agrupamiento_2tablas', 'V') IS NOT NULL
DROP VIEW vista_agrupamiento_2tablas;
GO

CREATE VIEW vista_agrupamiento_2tablas AS
SELECT C.name, COUNT(V.id) AS total_ventas
FROM clients C
JOIN sales V ON C.id = V.id_clients
GROUP BY C.name;
GO
```

![](images/clipboard-564258108.png)Verificación:

![](images/clipboard-1993770150.png)

Source:

![](images/clipboard-284756453.png)

Datos:

![](images/clipboard-4162219890.png)

------------------------------------------------------------------------

### ⚙️ Creación del Procedimiento

``` sql
IF OBJECT_ID('proc_agrupamiento_2tablas', 'P') IS NOT NULL
DROP PROCEDURE proc_agrupamiento_2tablas;
GO

CREATE PROCEDURE proc_agrupamiento_2tablas
AS
BEGIN
    SELECT C.name, COUNT(V.id) AS total_ventas
    FROM clients C
    JOIN sales V ON C.id = V.id_clients
    GROUP BY C.name;
END;
GO
```

![](images/clipboard-3521543379.png)Verificación:

![](images/clipboard-3353072567.png)

Source:

![](images/clipboard-2369570900.png)

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

------------------------------------------------------------------------

### 🔹 Creación de la Vista

``` sql
IF OBJECT_ID('vista_agrupamiento_3tablas', 'V') IS NOT NULL
DROP VIEW vista_agrupamiento_3tablas;
GO

CREATE VIEW vista_agrupamiento_3tablas AS
SELECT C.name AS cliente, P.name AS producto, SUM(PV.quantity) AS total_comprado
FROM clients C
JOIN sales V ON C.id = V.id_clients
JOIN sales_details PV ON V.id = PV.id_sales
JOIN products P ON P.id = PV.id_products
GROUP BY C.name, P.name;
GO
```

![](images/clipboard-2470619872.png)Verificación:

![](images/clipboard-2075525846.png)

Source:

![](images/clipboard-4132526405.png)

Datos:

![](images/clipboard-3575789416.png)

------------------------------------------------------------------------

### ⚙️ Creación del Procedimiento

``` sql
IF OBJECT_ID('proc_agrupamiento_3tablas', 'P') IS NOT NULL
DROP PROCEDURE proc_agrupamiento_3tablas;
GO

CREATE PROCEDURE proc_agrupamiento_3tablas
AS
BEGIN
    SELECT C.name AS cliente, P.name AS producto, SUM(PV.quantity) AS total_comprado
    FROM clients C
    JOIN sales V ON C.id = V.id_clients
    JOIN sales_details PV ON V.id = PV.id_sales
    JOIN products P ON P.id = PV.id_products
    GROUP BY C.name, P.name;
END;
GO
```

![](images/clipboard-641061126.png)Verificación:

![](images/clipboard-3022144368.png)

Source:

![](images/clipboard-3608261677.png)

------------------------------------------------------------------------

# 🔍 4. Subconsultas

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

Muestra los clientes que han realizado al menos una venta.

------------------------------------------------------------------------

### 🔹 Vista

``` sql
IF OBJECT_ID('vista_subconsulta_1tabla', 'V') IS NOT NULL
DROP VIEW vista_subconsulta_1tabla;
GO

CREATE VIEW vista_subconsulta_1tabla AS
SELECT *
FROM clients
WHERE id IN (
    SELECT id_clients FROM sales
);
GO
```

![](images/clipboard-1466824120.png)Verificación:

![](images/clipboard-450114777.png)

Source:

![](images/clipboard-1201146756.png)

Datos:

![](images/clipboard-241023779.png)

------------------------------------------------------------------------

### ⚙️ Procedimiento

``` sql
IF OBJECT_ID('proc_subconsulta_1tabla', 'P') IS NOT NULL
DROP PROCEDURE proc_subconsulta_1tabla;
GO

CREATE PROCEDURE proc_subconsulta_1tabla
AS
BEGIN
    SELECT *
    FROM clients
    WHERE id IN (
        SELECT id_clients FROM sales
    );
END;
GO
```

![](images/clipboard-3210558588.png)Verificación:

![](images/clipboard-855738549.png)

Source:

![](images/clipboard-1684068215.png)

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

Muestra los clientes que han realizado ventas mayores a 100.

------------------------------------------------------------------------

### 🔹 Vista

``` sql
IF OBJECT_ID('vista_subconsulta_2tablas', 'V') IS NOT NULL
DROP VIEW vista_subconsulta_2tablas;
GO

CREATE VIEW vista_subconsulta_2tablas AS
SELECT *
FROM clients
WHERE id IN (
    SELECT id_clients
    FROM sales
    WHERE total > 100
);
GO
```

![](images/clipboard-1390691377.png)Verificación:

![](images/clipboard-3140731610.png)

Source:

![](images/clipboard-3078163405.png)

Datos:

![](images/clipboard-3976762384.png)

------------------------------------------------------------------------

### ⚙️ Procedimiento

``` sql
IF OBJECT_ID('proc_subconsulta_2tablas', 'P') IS NOT NULL
DROP PROCEDURE proc_subconsulta_2tablas;
GO

CREATE PROCEDURE proc_subconsulta_2tablas
AS
BEGIN
    SELECT *
    FROM clients
    WHERE id IN (
        SELECT id_clients
        FROM sales
        WHERE total > 100
    );
END;
GO
```

![](images/clipboard-3720555228.png)

Verificación:

![](images/clipboard-3004742389.png)

Source:

![](images/clipboard-1447341074.png)

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

Muestra los clientes que han comprado productos con cantidad mayor o igual a 5.

------------------------------------------------------------------------

### 🔹 Vista

``` sql
IF OBJECT_ID('vista_subconsulta_3tablas', 'V') IS NOT NULL
DROP VIEW vista_subconsulta_3tablas;
GO

CREATE VIEW vista_subconsulta_3tablas AS
SELECT *
FROM clients
WHERE id IN (
    SELECT V.id_clients
    FROM sales V
    JOIN sales_details PV ON V.id = PV.id_sales
    WHERE PV.quantity >= 5
);
GO
```

![](images/clipboard-432749408.png)

Verificación:

![](images/clipboard-3149375735.png)

Source:

![](images/clipboard-1796511755.png)

Datos:

![](images/clipboard-3836233489.png)

------------------------------------------------------------------------

### ⚙️ Procedimiento

``` sql
IF OBJECT_ID('proc_subconsulta_3tablas', 'P') IS NOT NULL
DROP PROCEDURE proc_subconsulta_3tablas;
GO

CREATE PROCEDURE proc_subconsulta_3tablas
AS
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
GO
```

![](images/clipboard-896796871.png)

Verificación:

![](images/clipboard-4102976579.png)

Source:

![](images/clipboard-881279333.png)

------------------------------------------------------------------------

Aquí concluimos la actividad realizada mediante DBeaver utilizando el motor SQL Server

# Gestor Sql Management

# 1. Generación de datos con Mockaroo

Para comenzar, se utilizó la herramienta **Mockaroo** para generar registros de prueba.

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

![](images/clipboard-734277247.png)

------------------------------------------------------------------------

## 2.2 Operador "\>" (Mayor que)

``` sql
SELECT * FROM sales WHERE total > 100;
```

Muestra las ventas cuyo total es mayor a 100.

![](images/clipboard-1402763983.png)

------------------------------------------------------------------------

## 2.3 Operador "\>=" (Mayor o igual)

``` sql
SELECT * FROM sales WHERE total >= 50;
```

Obtiene las ventas con total mayor o igual a 50.

![](images/clipboard-1070384859.png)

------------------------------------------------------------------------

## 2.4 Operador "\<" (Menor que)

``` sql
SELECT * FROM sales_details WHERE quantity < 9;
```

Muestra los productos con cantidad menor a 9.

![](images/clipboard-1346273899.png)

------------------------------------------------------------------------

## 2.5 Operador "\<=" (Menor o igual)

``` sql
SELECT *  FROM sales_details  WHERE quantity <= 8;
```

Muestra los productos con cantidad menor o igual a 8.

![](images/clipboard-4062036059.png)

------------------------------------------------------------------------

## 2.6 Operador LIKE

``` sql
SELECT * FROM clients WHERE email LIKE 'm%';
```

Busca clientes cuyo correo comienza con la letra "m".

![](images/clipboard-682160669.png)

------------------------------------------------------------------------

## 2.7 Operador BETWEEN

``` sql
SELECT * FROM sales WHERE sale_date BETWEEN '2025-01-01' AND '2025-12-31';
```

Obtiene las ventas realizadas dentro del año 2025.

![](images/clipboard-1035193392.png)

------------------------------------------------------------------------

## 2.8 Consulta combinada

``` sql
SELECT C.name, V.total FROM clients C JOIN sales V ON C.id = V.id_clients WHERE V.total >= 50 AND V.sale_date BETWEEN '2025-01-01' AND '2026-01-01';
```

Muestra los clientes con ventas mayores o iguales a 50 dentro de un rango de fechas.

![](images/clipboard-4025800896.png)

------------------------------------------------------------------------

------------------------------------------------------------------------

#  3. Consultas de agrupamiento

------------------------------------------------------------------------

## 3.1 Agrupamiento con 1 tabla

``` sql
SELECT status, COUNT(*) AS total_clientes FROM clients GROUP BY status;
```

Agrupa los clientes por estado y cuenta cuántos hay en cada grupo.

------------------------------------------------------------------------

### 🔹 Creación de la Vista

``` sql
IF OBJECT_ID('vista_agrupamiento_1tabla', 'V') IS NOT NULL DROP VIEW vista_agrupamiento_1tabla; GO  CREATE VIEW vista_agrupamiento_1tabla AS SELECT status, COUNT(*) AS total_clientes FROM clients GROUP BY status; GO
```

![](images/clipboard-2519168259.png)Verificación en la sección de vistas:

![](images/clipboard-192199630.png)

Source:

![](images/clipboard-1789804899.png)

⚙️ Creación del Procedimiento

``` sql
IF OBJECT_ID('proc_agrupamiento_1tabla', 'P') IS NOT NULL DROP PROCEDURE proc_agrupamiento_1tabla; GO  CREATE PROCEDURE proc_agrupamiento_1tabla AS BEGIN     SELECT status, COUNT(*) AS total_clientes     FROM clients     GROUP BY status; END; GO
```

![](images/clipboard-1875186231.png)Verificación:

![](images/clipboard-3155038694.png)

Source:

![](images/clipboard-2391931510.png)

------------------------------------------------------------------------

## 3.2 Agrupamiento con 2 tablas

``` sql
SELECT C.name, COUNT(V.id) AS total_ventas FROM clients C JOIN sales V ON C.id = V.id_clients GROUP BY C.name;
```

Cuenta cuántas ventas ha realizado cada cliente.

------------------------------------------------------------------------

### 🔹 Creación de la Vista

``` sql
IF OBJECT_ID('vista_agrupamiento_2tablas', 'V') IS NOT NULL DROP VIEW vista_agrupamiento_2tablas; GO  CREATE VIEW vista_agrupamiento_2tablas AS SELECT C.name, COUNT(V.id) AS total_ventas FROM clients C JOIN sales V ON C.id = V.id_clients GROUP BY C.name; GO
```

![](images/clipboard-1041364211.png)Verificación:

![](images/clipboard-1107631070.png)

Source:

![](images/clipboard-3133947165.png)⚙️ Creación del Procedimiento

``` sql
IF OBJECT_ID('proc_agrupamiento_2tablas', 'P') IS NOT NULL DROP PROCEDURE proc_agrupamiento_2tablas; GO  CREATE PROCEDURE proc_agrupamiento_2tablas AS BEGIN     SELECT C.name, COUNT(V.id) AS total_ventas     FROM clients C     JOIN sales V ON C.id = V.id_clients     GROUP BY C.name; END; GO
```

![](images/clipboard-1458582151.png)Verificación:

![](images/clipboard-352494976.png)

Source:

![](images/clipboard-852505376.png)

------------------------------------------------------------------------

## 3.3 Agrupamiento con 3 tablas

``` sql
SELECT C.name AS cliente, P.name AS producto, SUM(PV.quantity) AS total_comprado FROM clients C JOIN sales V ON C.id = V.id_clients JOIN sales_details PV ON V.id = PV.id_sales JOIN products P ON P.id = PV.id_products GROUP BY C.name, P.name;
```

Muestra cuánto ha comprado cada cliente de cada producto.

------------------------------------------------------------------------

### 🔹 Creación de la Vista

``` sql
IF OBJECT_ID('vista_agrupamiento_3tablas', 'V') IS NOT NULL DROP VIEW vista_agrupamiento_3tablas; GO  CREATE VIEW vista_agrupamiento_3tablas AS SELECT C.name AS cliente, P.name AS producto, SUM(PV.quantity) AS total_comprado FROM clients C JOIN sales V ON C.id = V.id_clients JOIN sales_details PV ON V.id = PV.id_sales JOIN products P ON P.id = PV.id_products GROUP BY C.name, P.name; GO
```

![](images/clipboard-4229431628.png)Verificación:

![](images/clipboard-2512099434.png)

Source:

![](images/clipboard-500265498.png)

------------------------------------------------------------------------

### ⚙️ Creación del Procedimiento

``` sql
IF OBJECT_ID('proc_agrupamiento_3tablas', 'P') IS NOT NULL DROP PROCEDURE proc_agrupamiento_3tablas; GO  CREATE PROCEDURE proc_agrupamiento_3tablas AS BEGIN     SELECT C.name AS cliente, P.name AS producto, SUM(PV.quantity) AS total_comprado     FROM clients C     JOIN sales V ON C.id = V.id_clients     JOIN sales_details PV ON V.id = PV.id_sales     JOIN products P ON P.id = PV.id_products     GROUP BY C.name, P.name; END; GO
```

![](images/clipboard-2507539215.png)Verificación:

![](images/clipboard-1382206642.png)

Source:

![](images/clipboard-3873973238.png)

------------------------------------------------------------------------

# 🔍 4. Subconsultas

------------------------------------------------------------------------

## 4.1 Subconsulta con 1 tabla

``` sql
SELECT * FROM clients WHERE id IN (     SELECT id_clients     FROM sales );
```

Muestra los clientes que han realizado al menos una venta.

------------------------------------------------------------------------

### 🔹 Vista

``` sql
IF OBJECT_ID('vista_subconsulta_1tabla', 'V') IS NOT NULL DROP VIEW vista_subconsulta_1tabla; GO  CREATE VIEW vista_subconsulta_1tabla AS SELECT * FROM clients WHERE id IN (     SELECT id_clients FROM sales ); GO
```

![](images/clipboard-2377691573.png)Verificación:

![](images/clipboard-2840374233.png)

Source:

![](images/clipboard-995938235.png)

⚙️ Procedimiento

``` sql
IF OBJECT_ID('proc_subconsulta_1tabla', 'P') IS NOT NULL DROP PROCEDURE proc_subconsulta_1tabla; GO  CREATE PROCEDURE proc_subconsulta_1tabla AS BEGIN     SELECT *     FROM clients     WHERE id IN (         SELECT id_clients FROM sales     ); END; GO
```

![](images/clipboard-2860663272.png)Verificación:

![](images/clipboard-123639110.png)

Source:

![](images/clipboard-2426120751.png)

------------------------------------------------------------------------

## 4.2 Subconsulta con 2 tablas

``` sql
SELECT * FROM clients WHERE id IN (     SELECT id_clients     FROM sales     WHERE total > 100 );
```

Muestra los clientes que han realizado ventas mayores a 100.

------------------------------------------------------------------------

### 🔹 Vista

``` sql
IF OBJECT_ID('vista_subconsulta_2tablas', 'V') IS NOT NULL DROP VIEW vista_subconsulta_2tablas; GO  CREATE VIEW vista_subconsulta_2tablas AS SELECT * FROM clients WHERE id IN (     SELECT id_clients     FROM sales     WHERE total > 100 ); GO
```

![](images/clipboard-3811589686.png)Verificación:

![](images/clipboard-95536102.png)

Source:

![](images/clipboard-2298719775.png)

Datos:

![](images/clipboard-3052904083.png)

------------------------------------------------------------------------

### ⚙️ Procedimiento

``` sql
IF OBJECT_ID('proc_subconsulta_2tablas', 'P') IS NOT NULL DROP PROCEDURE proc_subconsulta_2tablas; GO  CREATE PROCEDURE proc_subconsulta_2tablas AS BEGIN     SELECT *     FROM clients     WHERE id IN (         SELECT id_clients         FROM sales         WHERE total > 100     ); END; GO
```

![](images/clipboard-3271449960.png)

Verificación:

![](images/clipboard-199833527.png)

Source:

![](images/clipboard-200647548.png)

------------------------------------------------------------------------

## 4.3 Subconsulta con 3 tablas

``` sql
SELECT * FROM clients WHERE id IN (     SELECT V.id_clients     FROM sales V     JOIN sales_details PV ON V.id = PV.id_sales     WHERE PV.quantity >= 5 );
```

Muestra los clientes que han comprado productos con cantidad mayor o igual a 5.

------------------------------------------------------------------------

### 🔹 Vista

``` sql
IF OBJECT_ID('vista_subconsulta_3tablas', 'V') IS NOT NULL DROP VIEW vista_subconsulta_3tablas; GO  CREATE VIEW vista_subconsulta_3tablas AS SELECT * FROM clients WHERE id IN (     SELECT V.id_clients     FROM sales V     JOIN sales_details PV ON V.id = PV.id_sales     WHERE PV.quantity >= 5 ); GO
```

![](images/clipboard-83220632.png)

Verificación:

![](images/clipboard-810776734.png)

Source:

![](images/clipboard-160512801.png)

Datos:

![](images/clipboard-2550574299.png)

------------------------------------------------------------------------

### ⚙️ Procedimiento

``` sql
IF OBJECT_ID('proc_subconsulta_3tablas', 'P') IS NOT NULL DROP PROCEDURE proc_subconsulta_3tablas; GO  CREATE PROCEDURE proc_subconsulta_3tablas AS BEGIN     SELECT *     FROM clients     WHERE id IN (         SELECT V.id_clients         FROM sales V         JOIN sales_details PV ON V.id = PV.id_sales         WHERE PV.quantity >= 5     ); END; GO
```

![](images/clipboard-2706251748.png)

Verificación:

![](images/clipboard-3689087636.png)

Source:

![](images/clipboard-263090770.png)

------------------------------------------------------------------------

Aquí concluimos la actividad realizada mediante Sql Management utilizando el motor SQL Server
