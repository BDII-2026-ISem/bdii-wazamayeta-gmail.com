### Condiciones en las Consultas o filtros en las Consultas

Para las condiciones se utiliza la clausula Where de la siguiente manera:

Quiero realizar la misma consulta anterior de cualquiera de las dos formas, teniendo en cuenta la condición que presente las ventas de una fecha especifica.

Forma 1:

``` {.script style="color: blue"}
CREATE OR REPLACE VIEW Buscarusuariosporfecha AS
SELECT C.name, C.email, V.*
FROM clients C
JOIN sales V ON C.id = V.id_clients
WHERE V.sale_date = DATE '2025-10-09';
```

![](images/clipboard-4013955281.png)![](images/clipboard-2809902581.png)Forma 2:

```         
CREATE OR REPLACE VIEW Buscarusuariosporfecha2 AS
SELECT C.name, C.email, V.*
FROM clients C
JOIN sales V ON C.id = V.id_clients
WHERE V.sale_date = DATE '2025-09-29';
```

![](images/clipboard-2703196559.png)

1.  ![](images/clipboard-690691974.png)Mostrar todo los correos de los clientes que comiencen con la letra m.

``` {.script style="color: blue"}
CREATE OR REPLACE VIEW Filtrar_correo_M AS
SELECT *
FROM clients C
WHERE C.email LIKE 'm%';
```

![](images/clipboard-3766357226.png)

![](images/clipboard-4007568468.png)

-   Mostrar todos los correos de los clientes que contengan el dominio gmail

    ```         
    CREATE OR REPLACE VIEW Filtrar_dominio_gmail AS
    SELECT *
    FROM clients C
    WHERE C.email LIKE '%' || 'gmail' || '%';
    ```

    ![](images/clipboard-1006830651.png)

-   Nos da el siguiente resultado

![](images/clipboard-2311467767.png)

Mostrar todas las ventas en un rango de fecha, incluyendo sus respectivos clientes y productos. Ordenarla ascendentemente por fecha

Forma 1:

```         
CREATE OR REPLACE VIEW Filtrar_rango_fecha AS
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
WHERE V.sale_date BETWEEN DATE '2025-09-14' AND DATE '2026-02-20';
```

![](images/clipboard-2021600052.png)

![](images/clipboard-2871021799.png)Forma 2:![](images/clipboard-2835439898.png)

Dandonos como resultado![](images/clipboard-4105745608.png)

### Consultas de Agrupamiento

Se consideran este tipo de consultas cuando tenemos valores que se repiten en los registros.

Si observamos la siguiente consulta:

```         
CREATE OR REPLACE VIEW agrupamiento AS
SELECT C.name, C.email, V.*
FROM sales V
JOIN clients C ON C.id = V.id_clients;
```

![](images/clipboard-3997368288.png)

![](images/clipboard-1801301367.png)su resultado es

Forma: 1

``` {.script style="color: blue"}
CREATE OR REPLACE VIEW agrupamiento_clientesid AS
SELECT 
    C.id,
    C.name,
    SUM(V.total) AS TotalSuma,
    COUNT(V.id_clients) AS CuentaTotal,
    AVG(V.total) AS Promedio
FROM clients C
JOIN sales V ON C.id = V.id_clients
WHERE V.sale_date BETWEEN DATE '2025-03-01' AND DATE '2026-03-30'
GROUP BY C.id, C.name;
```

![](images/clipboard-3332972774.png)su resultado es![](images/clipboard-1774098570.png)

Forma 2:

``` {.script style="color: blue"}
CREATE OR REPLACE VIEW agrupamiento_clientesid2 AS
SELECT 
    C.id,
    C.name,
    SUM(V.total) AS TotalSuma,
    COUNT(V.id_clients) AS CuentaTotal,
    AVG(V.total) AS Promedio
FROM clients C
JOIN sales V ON C.id = V.id_clients
WHERE V.sale_date BETWEEN DATE '2025-03-01' AND DATE '2026-03-30'
GROUP BY C.id, C.name;
```

![![](images/clipboard-3261298589.png)](images/clipboard-2656779900.png)

También se le pueden colocar condiciones a las consultas agrupadas. Para este caso utilice la clausula Having., e la siguiente manera:

Forma 1:

``` {.script style="color: blue"}
CREATE OR REPLACE VIEW agrupamiento_clientesid_having AS
SELECT 
    C.id,
    C.name,
    SUM(V.total) AS TotalSuma,
    COUNT(V.id_clients) AS CuentaTotal,
    AVG(V.total) AS Promedio
FROM clients C
JOIN sales V ON C.id = V.id_clients
WHERE V.sale_date BETWEEN DATE '2025-03-01' AND DATE '2026-03-30'
GROUP BY C.id, C.name
HAVING COUNT(V.id_clients) >= 2;
```

![](images/clipboard-1357975169.png)teniendo como resultado![](images/clipboard-3693569936.png)

Forma 2:

``` {.script style="color: blue"}
CREATE OR REPLACE VIEW agrupamiento_clientesid_having2 AS
SELECT 
    C.id,
    C.name,
    SUM(V.total) AS TotalSuma,
    COUNT(V.id_clients) AS CuentaTotal,
    AVG(V.total) AS Promedio
FROM clients C
JOIN sales V ON C.id = V.id_clients
WHERE V.sale_date BETWEEN DATE '2025-03-01' AND DATE '2026-03-30'
GROUP BY C.id, C.name
HAVING COUNT(V.id_clients) >= 2;
```

![](images/clipboard-3817079502.png)

y su resultado![](images/clipboard-917499887.png)
