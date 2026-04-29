---
title: "Postgres-V-P"
output: html_document
---

### Condiciones en las Consultas o filtros en las Consultas

Para las condiciones se utiliza la clausula Where de la siguiente manera:

Quiero realizar la misma consulta anterior de cualquiera de las dos formas, teniendo en cuenta la condición que presente las ventas de una fecha especifica.

Forma 1:

``` {.script style="color: blue"}
CREATE VIEW Buscarusuariosporfecha AS
SELECT C.name, C.email, V.* 
FROM clients C 
JOIN sales V ON C.id = V.id_clients 
WHERE V.sale_date = '2025-10-09';
```

![](images/clipboard-4135295842.png)

y nos da como resultado

![](images/clipboard-1165607689.png)

Forma 2:

```         
CREATE VIEW Buscarusuariosporfecha2 AS
SELECT C.name, C.email, V.* 
FROM clients C 
JOIN sales V ON C.id = V.id_clients 
WHERE V.sale_date = '2025-09-29';
```

1.  ![](images/clipboard-1155723931.png) y nos da como resultado

    ![](images/clipboard-2959419757.png)

``` {.script style="color: blue"}
CREATE VIEW Filtrar_correo_M AS
SELECT * 
FROM clients C 
WHERE C.email LIKE 'm%'
```

![Nos devuelve un resultado tal que asi](images/clipboard-2584511289.png)

![](images/clipboard-4084071243.png)

-   Mostrar todos los correos de los clientes que contengan el dominio gmail

    ```         
        create or replace view Filtrar_dominio_gmail
        as
    SELECT * 
    FROM clients as C 
    where C.email like concat('%','gmail','%'); 
    ```

    ![](images/clipboard-2418600197.png)

-   Nos da el siguiente resultado

![](images/clipboard-2311467767.png)

Mostrar todas las ventas en un rango de fecha, incluyendo sus respectivos clientes y productos. Ordenarla ascendentemente por fecha

Forma 1:

```         
CREATE VIEW Filtrar_rango_fecha AS
SELECT 
    C.name AS nombre, 
    C.status AS estado, 
    V.sale_date AS fecha,
    PV.quantity AS cantidad, 
    P.name AS producto 
FROM clients C
JOIN sales V ON C.id = V.id_clients
JOIN sales_details PV ON V.id = PV.id_sales
JOIN products P ON P.id = PV.id_products
WHERE V.sale_date BETWEEN '2025-09-14' AND '2026-02-20';
```

![](images/clipboard-1232162929.png)y nos devuelve

![](images/clipboard-3055840424.png)

Forma 2:

![](images/clipboard-470977909.png)

Dandonos como resultado

![](images/clipboard-1616832096.png)

### Consultas de Agrupamiento

Se consideran este tipo de consultas cuando tenemos valores que se repiten en los registros.

Si observamos la siguiente consulta:

```         
CREATE VIEW agrupamiento AS
SELECT C.name, C.email, V.* 
FROM sales V 
JOIN clients C ON C.id = V.id_clients;
```

![](images/clipboard-3062265759.png)su resultado es![](images/clipboard-3924148593.png)

Forma: 1

``` {.script style="color: blue"}
CREATE VIEW agrupamiento_clientesid AS
SELECT 
    C.id, 
    C.name,
    SUM(V.total) AS TotalSuma,
    COUNT(V.id_clients) AS CuentaTotal,
    AVG(V.total) AS Promedio
FROM clients C
JOIN sales V ON C.id = V.id_clients
WHERE V.sale_date BETWEEN '2025-03-01' AND '2026-03-30'
GROUP BY C.id, C.name;
```

![](images/clipboard-2414438750.png)su resultado es![](images/clipboard-3968239747.png)

Forma 2:

``` {.script style="color: blue"}
CREATE VIEW agrupamiento_clientesid2 AS
SELECT 
    C.id, 
    C.name,
    SUM(V.total) AS TotalSuma,
    COUNT(V.id_clients) AS CuentaTotal,
    AVG(V.total) AS Promedio
FROM clients C
JOIN sales V ON C.id = V.id_clients
WHERE V.sale_date BETWEEN '2025-03-01' AND '2026-03-30'
GROUP BY C.id, C.name;
```

![![](images/clipboard-970043789.png)](images/clipboard-2656779900.png)

También se le pueden colocar condiciones a las consultas agrupadas. Para este caso utilice la clausula Having., e la siguiente manera:

Forma 1:

``` {.script style="color: blue"}
CREATE VIEW agrupamiento_clientesid_having AS
SELECT 
    C.id, 
    C.name,
    SUM(V.total) AS TotalSuma,
    COUNT(V.id_clients) AS CuentaTotal,
    AVG(V.total) AS Promedio
FROM clients C
JOIN sales V ON C.id = V.id_clients
WHERE V.sale_date BETWEEN '2025-03-01' AND '2026-03-30'
GROUP BY C.id, C.name
HAVING COUNT(V.id_clients) >= 2;
```

![](images/clipboard-3559517704.png)teniendo como resultado![](images/clipboard-2755877348.png)

Forma 2:

``` {.script style="color: blue"}

CREATE VIEW agrupamiento_clientesid_having2 AS
SELECT 
    C.id, 
    C.name,
    SUM(V.total) AS TotalSuma,
    COUNT(V.id_clients) AS CuentaTotal,
    AVG(V.total) AS Promedio
FROM clients C
JOIN sales V ON C.id = V.id_clients
WHERE V.sale_date BETWEEN '2025-03-01' AND '2026-03-30'
GROUP BY C.id, C.name
HAVING COUNT(V.id_clients) >= 2;
```

![](images/clipboard-3559517704.png)

y su resultado

![](images/clipboard-2871938890.png)
