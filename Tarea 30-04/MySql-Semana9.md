---
title: "MySql-Semana9"
output: html_document
---

Aqui en este markdown haremos todo el proceso pedido en la actividad de la semana 9 empezamos con el proceso de agregar registros con MOCKAROO, y al solicitarlos, se nos retorna un archivo sql, con todos los datos para cada tabla

![](images/clipboard-2928949879.png){width="600"}

como se muestra en la imagen

despues de haber hecho el primer punto, realizaremos el segundo que es hacer consultas avanzadas con operadores logicos y beetwen, and Like

Empezamos usando el primer operador logico

1.  "=" o "Igualdad"

Usaremos la siguiente consulta

``` sql
SELECT *
FROM clients
WHERE status = 'Activo';
```

Donde se filtran los clientes donde su estatus este en "Activo", dandonos comoi resultado

![](images/clipboard-2968842538.png){width="560"}

2.  "\>" o "Mayor Que"

Usaremos la siguiente consulta

``` sql
SELECT *
FROM sales
WHERE total > 100;
```

Donde se muestra las ventas cuyo total es mayor a 100.

![](images/clipboard-2593370976.png){width="531"}

3.  "\>=" o "Mayor o Igual"

Usaremos la siguiente consulta

``` sql
SELECT *
FROM sales
WHERE total >= 50;
```

Obtiene las ventas con total mayor o igual a 50.

![](images/clipboard-3121602612.png){width="543"}

4.  "\<" o "menor que"

    Usaremos la siguiente consulta

``` sql
SELECT *
FROM sales_details
WHERE quantity < 9;
```

Muestra los productos con una cantidad menor que 9.

![](images/clipboard-2301704793.png){width="492"}

5.  "\<=" o "menor o igual que"

    Usaremos la siguiente consulta

``` sql
SELECT * 
FROM sales_details 
WHERE quantity <= 8;
```

Muestra los productos con una cantidad menor o igual que 8.

![](images/clipboard-1555395350.png)

6.  "Like"

    Usaremos la siguiente consulta

    ``` sql
    SELECT *
    FROM clients
    WHERE email LIKE 'm%';
    ```

    Busca clientes cuyo correo comienza con la letra "m".

    ![](images/clipboard-2728769682.png)

7.  "between" o "rango"

    -    Usaremos la siguiente consulta

        ``` sql
        SELECT *
        FROM sales
        WHERE sale_date BETWEEN '2025-01-01' AND '2025-12-31';
        ```

        Obtiene las ventas realizadas dentro del año 2025.

    ![](images/clipboard-2862545811.png)

Para ultima consulta, haremos una donde se apliquen todas o la mayoria de las consultas hechas anteriormente en una sola

-    Usaremos la siguiente consulta

    ``` sql
    SELECT C.name, V.total
    FROM clients C
    JOIN sales V ON C.id = V.id_clients
    WHERE V.total >= 50
    AND V.sale_date BETWEEN '2025-01-01' AND '2026-01-01';
    ```

    Donde Nos muestra los clientes con ventas mayores o igual a 50 dentro de un rango de fechas

    ![](images/clipboard-1655089596.png)
