aqui empezaremos a crear un crud para Dos tablas de cada motor teniendo en cuenta que usaremos Create, Update, Delete
empezaremos con oracle

primeramente abrimos un Sql de la coneccion ![alt text](images/image.png)
y agregamos este script
![alt text](images/image-2.png)
elegimos de referencia como primera tabla suplieers

y asi continuamos haciendo procedimientos de eliminar, borrar y actualizar
![alt text](images/image-3.png)

inserto una vista mas general del proceso acabado de realizar
![alt text](images/image-4.png)

en mi caso me toco ajustar las tablas para que pudiesen empezar desde 51 almenos en la insercion
ALTER TABLE employees
![alt text](images/image-5.png)

inserccion en suppliers
![alt text](images/image-6.png)

en la tabla employees
![alt text](images/image-7.png)

los updates en suppliers
![alt text](images/image-8.png)

update en eemployees
![alt text](images/image-9.png)

delete en suppliers
![alt text](images/image-10.png)
delete en employees
![alt text](images/image-11.png)

Aqui ejecutamos los procesos
inserts
![alt text](images/image-12.png)
hay que tener en cuenta que en oracleno se guardan automaticamente los cambios asi que hay que ejecutar los procesos con un commit al final
![alt text](images/image-13.png)
Delete
![alt text](images/image-14.png)
