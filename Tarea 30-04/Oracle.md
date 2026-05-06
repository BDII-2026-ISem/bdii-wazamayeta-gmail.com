# Actividad Semana 9 - Base de Datos

En este documento se presenta todo el proceso solicitado en la actividad de la semana 9.

# Gestor Dbeaver

---

# 1Para comenzar, se utilizó la herramienta **Mockaroo** para generar registros de prueba.

Al solicitar los datos, se obtiene un archivo SQL con todos los registros necesarios para cada tabla.

![](images_O/clipboard-2928949879.png){width="600"}

Como se muestra en la imagen.

---

# 2. Consultas avanzadas con operadores

Después de generar los datos, se realizan consultas usando operadores lógicos como:

`=`, `>`, `>=`, `<`, `<=`, `LIKE`, `BETWEEN`

---

## 2.1 Operador "=" (Igualdad)

```sql
SELECT * FROM clients WHERE status = 'Activo';
```

Filtra los clientes cuyo estado sea **Activo**.

![alt text](images_O/image.png)

---

## 2.2 Operador "\>" (Mayor que)

```sql
SELECT * FROM sales WHERE total > 100;
```

Muestra las ventas cuyo total es mayor a 100.

![alt text](images_O/image-1.png)

---

## 2.3 Operador "\>=" (Mayor o igual)

```sql
SELECT * FROM sales WHERE total >= 50;
```

Obtiene las ventas con total mayor o igual a 50.

![alt text](images_O/image-2.png)

---

## 2.4 Operador "\<" (Menor que)

```sql
SELECT * FROM sales_details WHERE quantity < 9;
```

Muestra los productos con cantidad menor a 9.
![alt text](images_O/image-3.png)

---

## 2.5 Operador "\<=" (Menor o igual)

```sql
SELECT *  FROM sales_details  WHERE quantity <= 8;
```

Muestra los productos con cantidad menor o igual a 8.

## ![alt text](images_O/image-4.png)

## 2.6 Operador LIKE

```sql
SELECT * FROM clients WHERE email LIKE 'm%';
```

Busca clientes cuyo correo comienza con la letra "m".

![alt text](images_O/image-5.png)

---

## 2.7 Operador BETWEEN

```sql
SELECT * FROM sales WHERE sale_date BETWEEN '2025-01-01' AND '2025-12-31';
```

Obtiene las ventas realizadas dentro del año 2025.
![alt text](images_O/image-6.png)

---

## 2.8 Consulta combinada

```sql
SELECT C.name, V.total FROM clients C JOIN sales V ON C.id = V.id_clients WHERE V.total >= 50 AND V.sale_date BETWEEN '2025-01-01' AND '2026-01-01';
```

Muestra los clientes con ventas mayores o iguales a 50 dentro de un rango de fechas.
![alt text](images_O/image-7.png)

---

## 3.1 Agrupamiento con 1 tabla

```sql
SELECT status, COUNT(*) AS total_clientes
FROM clients
GROUP BY status;
```

Agrupa los clientes por estado y cuenta cuántos hay en cada grupo.
![alt text](images_O/image-8.png)

---

### Creación de la Vista

```sql
CREATE OR REPLACE VIEW vista_agrupamiento_1tabla AS
SELECT status, COUNT(*) AS total_clientes
FROM clients
GROUP BY status;
```

![alt text](images_O/image-9.png)
verificación en la sección de vistas:

![alt text](images_O/image-10.png)

### Creación de la Función

```sql
CREATE OR REPLACE PROCEDURE proc_agrupamiento_1tabla IS
BEGIN
    FOR r IN (
        SELECT status, COUNT(*) AS total_clientes
        FROM clients
        GROUP BY status
    ) LOOP
        DBMS_OUTPUT.PUT_LINE(r.status || ' - ' || r.total_clientes);
    END LOOP;
END;
/
```

![alt text](images_O/image-11.png)

Verificación:
![alt text](images_O/image-12.png)

## 3.2 Agrupamiento con 2 tablas

```sql
SELECT C.name, COUNT(V.id) AS total_ventas
FROM clients C
JOIN sales V ON C.id = V.id_clients
GROUP BY C.name;
```

![alt text](images_O/image-13.png)
Cuenta cuántas ventas ha realizado cada cliente.

---

### Creación de la Vista

```sql
CREATE OR REPLACE VIEW vista_agrupamiento_2tablas AS
SELECT C.name, COUNT(V.id) AS total_ventas
FROM clients C
JOIN sales V ON C.id = V.id_clients
GROUP BY C.name;
```

![alt text](images_O/image-14.png)

Verificación:

## ![alt text](images_O/image-15.png)

### Creación de la Función

```sql
CREATE OR REPLACE PROCEDURE proc_agrupamiento_2tablas IS
BEGIN
    FOR r IN (
        SELECT C.name, COUNT(V.id) AS total_ventas
        FROM clients C
        JOIN sales V ON C.id = V.id_clients
        GROUP BY C.name
    ) LOOP
        DBMS_OUTPUT.PUT_LINE(r.name || ' - ' || r.total_ventas);
    END LOOP;
END;
```

![alt text](images_O/image-16.png)
Verificación:

![alt text](images_O/image-17.png)

## 3.3 Agrupamiento con 3 tablas

```sql
SELECT C.name AS cliente, P.name AS producto, SUM(PV.quantity) AS total_comprado
FROM clients C
JOIN sales V ON C.id = V.id_clients
JOIN sales_details PV ON V.id = PV.id_sales
JOIN products P ON P.id = PV.id_products
GROUP BY C.name, P.name;
```

![](images_O/clipboard-2350007754.png)Muestra cuánto ha comprado cada cliente de cada producto.

---

### Creación de la Vista

```sql
CREATE OR REPLACE VIEW vista_agrupamiento_3tablas AS
SELECT C.name AS cliente, P.name AS producto, SUM(PV.quantity) AS total_comprado
FROM clients C
JOIN sales V ON C.id = V.id_clients
JOIN sales_details PV ON V.id = PV.id_sales
JOIN products P ON P.id = PV.id_products
GROUP BY C.name, P.name;
```

![alt text](images_O/image-18.png)
![alt text](images_O/image-19.png)
Verificación:

![alt text](images_O/image-20.png)

---

### Creación de la Función

```sql
CREATE OR REPLACE PROCEDURE proc_agrupamiento_3tablas IS
BEGIN
    FOR r IN (
        SELECT C.name AS cliente, P.name AS producto, SUM(PV.quantity) AS total_comprado
        FROM clients C
        JOIN sales V ON C.id = V.id_clients
        JOIN sales_details PV ON V.id = PV.id_sales
        JOIN products P ON P.id = PV.id_products
        GROUP BY C.name, P.name
    ) LOOP
        DBMS_OUTPUT.PUT_LINE(r.cliente || ' - ' || r.producto || ' - ' || r.total_comprado);
    END LOOP;
END;
```

![alt text](images_O/image-21.png)
Verificación:
![alt text](images_O/image-22.png)

---

# 4. Subconsultas

---

## 4.1 Subconsulta con 1 tabla

```sql
SELECT *
FROM clients
WHERE id IN (
    SELECT id_clients
    FROM sales
);
```

![alt text](images_O/image-23.png)
Muestra los clientes que han realizado al menos una venta.

---

### Vista

```sql
CREATE OR REPLACE VIEW vista_subconsulta_1tabla AS
SELECT *
FROM clients
WHERE id IN (
    SELECT id_clients
    FROM sales
);
```

![alt text](images_O/image-24.png)

verificacion:
![alt text](images_O/image-25.png)

### Función

```sql
CREATE OR REPLACE PROCEDURE proc_subconsulta_1tabla IS
BEGIN
    FOR r IN (
        SELECT *
        FROM clients
        WHERE id IN (SELECT id_clients FROM sales)
    ) LOOP
        DBMS_OUTPUT.PUT_LINE(r.id);
    END LOOP;
END;
/
```

![alt text](images_O/image-26.png)

Verificacion:
![alt text](images_O/image-27.png)

---

## 4.2 Subconsulta con 2 tablas

```sql
SELECT *
FROM clients
WHERE id IN (
    SELECT id_clients
    FROM sales
    WHERE total > 100
);
```

![alt text](images_O/image-28.png)

---

### Vista

```sql
CREATE OR REPLACE VIEW vista_subconsulta_2tablas AS
SELECT *
FROM clients
WHERE id IN (
    SELECT id_clients
    FROM sales
    WHERE total > 100
);
```

![alt text](images_O/image-29.png)

---

Verificamos:
![alt text](images_O/image-30.png)

### Función

```sql
CREATE OR REPLACE PROCEDURE proc_subconsulta_2tablas IS
BEGIN
    FOR r IN (
        SELECT *
        FROM clients
        WHERE id IN (
            SELECT id_clients FROM sales WHERE total > 100
        )
    ) LOOP
        DBMS_OUTPUT.PUT_LINE(r.id);
    END LOOP;
END;
```

![](images_O/clipboard-206937751.png)

Verificacion:

![alt text](image-31.png)

---

## 4.3 Subconsulta con 3 tablas

```sql
SELECT *
FROM clients
WHERE id IN (
    SELECT V.id_clients
    FROM sales V
    JOIN sales_details PV ON V.id = PV.id_sales
    WHERE PV.quantity >= 5
);
```

---

### Vista

```sql
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

![alt text](images_O/image-32.png)

---

![alt text](images_O/image-33.png)

Verificacion:

![alt text](images_O/image-34.png)

### Función

```sql
CREATE OR REPLACE PROCEDURE proc_subconsulta_3tablas IS
BEGIN
    FOR r IN (
        SELECT *
        FROM clients
        WHERE id IN (
            SELECT V.id_clients
            FROM sales V
            JOIN sales_details PV ON V.id = PV.id_sales
            WHERE PV.quantity >= 5
        )
    ) LOOP
        DBMS_OUTPUT.PUT_LINE(r.id);
    END LOOP;
END;
```

![alt text](images_O/image-35.png)

---

Verificacion:

![alt text](images_O/image-36.png)

Se completó la actividad utilizando Oracle mediante los gestores DBeaver aplicando consultas avanzadas, agrupamientos, subconsultas, vistas y funciones correctamente.
