---
title: "Postgres-V-P"
output: html_document
---

### Condiciones en las Consultas o filtros en las Consultas

Para las condiciones se utiliza la clausula Where de la siguiente manera:

Quiero realizar la misma consulta anterior de cualquiera de las dos formas, teniendo en cuenta la condición que presente las ventas de una fecha especifica.

Forma 1:

``` {.script style="color: blue"}
    create or replace view Buscarusuariosporfecha
as
SELECT C.name, C.email, V.* 
FROM clients as C, sales as V 
where C.id = V.id_clients and V.sale_date  = '2025-10-09'; 
```

![y nos da como resultado](images/clipboard-3075164101.png)

![](images/clipboard-1786510309.png)

Forma 2:

```         
create or replace view Buscarusuariosporfecha2
    as
    SELECT C.name, C.email, V.* 
    FROM clients as C 
    join sales as V on(C.id = V.id_clients) 
    where  V.sale_date = '2025-09-29'; 
```

![Nos devuelve un resultado![](images/clipboard-4139965908.png)](images/clipboard-2570376807.png)

1.  Mostrar todo los correos de los clientes que comiencen con la letra m.

``` {.script style="color: blue"}
    create or replace view Filtrar_correo_M     as select *  from clients as C  where C.email like 'm%';
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
create or replace view Filtrar_rango_fecha
as

select C.name as nombre,C.status as estado, V.sale_date as fecha,
PV.quantity as cantidad, P.name as producto 
from clients as C, sales as V, sales_details as PV, products as P 
where C.id = V.id_clients and 
V.id = PV.id_sales and 
P.id = PV.id_products and 
V.sale_date between '2025-09-14' and '2026-02-20' 
order by V.sale_date asc; 
```

![Retornandonos los siguiente valores](images/clipboard-976776532.png)

![](images/clipboard-3968426382.png)

Forma 2:

![](images/clipboard-470977909.png)

Dandonos como resultado

![](images/clipboard-1616832096.png)

### Consultas de Agrupamiento

Se consideran este tipo de consultas cuando tenemos valores que se repiten en los registros.

Si observamos la siguiente consulta:

```         
create or replace view agrupamiento
as

SELECT C.name, C.email, V.* 
FROM sales as V 
join clients as C on( C.id = V.id_clients );
```

![](images/clipboard-1192772800.png)

su resultado es

![](images/clipboard-2721025681.png)

Forma: 1

``` {.script style="color: blue"}
create or replace view agrupamiento_clientesid
as

SELECT C.id, C.name, sum(V.total) as TotalSuma, count(V.id_clients) as CuentaTotal, avg(V.total) as Promedio  
FROM clients as C, sales as V  
where  C.id = V.id_clients and 
V.sale_date between '2025-03-01' and '2026-03-30' 
group by C.id 
order by TotalSuma desc; 
```

![](images/clipboard-3758755778.png)

su resultado es\
![](images/clipboard-1989469078.png)

Forma 2:

``` {.script style="color: blue"}
create or replace view agrupamiento_clientesid2
as

SELECT C.id, C.name, sum(V.total) as TotalSuma, count(V.id_clients) as CuentaTotal, avg(V.total) as Promedio  
FROM clients as C 
join sales as V on( C.id = V.id_clients ) 
where V.sale_date between '2025-03-01' and '2026-03-30'
group by C.id 
order by TotalSuma desc; 
```

![su resultado es](images/clipboard-1020090872.png)

![](images/clipboard-2656779900.png)

También se le pueden colocar condiciones a las consultas agrupadas. Para este caso utilice la clausula Having., e la siguiente manera:

Forma 1:

``` {.script style="color: blue"}
create or replace view agrupamiento_clientesid_having
as

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
HAVING COUNT(V.id_clients) >= 2
ORDER BY TotalSuma DESC;
```

![](images/clipboard-498267137.png)

teniendo como resultado

![](images/clipboard-3059221014.png)

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
WHERE V.sale_date BETWEEN '2025-03-01' AND '2026-03-30'
GROUP BY C.id, C.name
HAVING COUNT(V.id_clients) >= 2
ORDER BY SUM(V.total) DESC;
```

![](images/clipboard-1686815353.png)

y su resultado

![](images/clipboard-1263094860.png)
