------------------------------------------------------------------------

------------------------------------------------------------------------

----Primer paso MySQL por Ubuntu—-

Se elimina la base de datos que ya estaba creada

![](images/clipboard-792183407.png)

Creamos nuevamente la base de datos ferrohogar

![](images/clipboard-1645994990.png)

Usamos la base de datos

![](images/clipboard-71175950.png)

----Acto seguido creamos todas las tablas—-

creamos las tabla categories (categorias)

![](images/clipboard-2377253290.png)

Mostramos la tabla categories

![](images/clipboard-3562442274.png)

creamos la tabla suppliers(-----)

![](images/clipboard-1426729716.png)

Mostramos la tabla suppliers

![](images/clipboard-3007753493.png)

Creamos la tabla clients

![](images/clipboard-1419677849.png)

Mostramos la tabla clients

![](images/clipboard-1292415680.png)

Creamos la tabla employees

![](images/clipboard-802414961.png)

Mostramos la tabla employees

![](images/clipboard-1356390094.png)

Creamos la tabla products

![](images/clipboard-543234620.png)

Mostramos la tabla products

![](images/clipboard-3585232916.png)

Creamos la tabla sales

![](images/clipboard-1897000268.png)

Mostramos la tabla sales

![](images/clipboard-3710808475.png)

Creamos la tabla sales_details

![](images/clipboard-1974063237.png)

Mostramos la tabla sales_details

![](images/clipboard-3639530859.png)

---Acto seguido mostramos todas las tablas—

![](images/clipboard-2898848798.png)

Confirmamos

![](images/clipboard-946309645.png)

-----segundo paso MySQL por workbench—–

Verficiamos que la conexion este abierta

![](images/clipboard-1057156766.png)

Borramos la base de datos

![](images/clipboard-1163601942.png)

Creamos la base de dato

![](images/clipboard-3931119271.png)

Mostramos la base de datos

![](images/clipboard-3781758186.png)

Usamos la base de datos y creamos la tabla categories

![](images/clipboard-4152964261.png)

Mostramos la tabla categories

![](images/clipboard-1567474342.png)

Creamos la tabla suppliers

![](images/clipboard-1005813560.png)

Mostramos la tabla suppliers

![](images/clipboard-1870880224.png)

Creamos la tabla clients

![](images/clipboard-4163394673.png)

mostramos la tabla clients

![](images/clipboard-1086277268.png)

Creamos la tabla employees

![](images/clipboard-947139160.png)

Mostramos la tabla employees

![](images/clipboard-1662612680.png)

Creamos la tabla products

![](images/clipboard-884138450.png)

mostramos la tabla productos

![](images/clipboard-226115674.png)

Creamos la tabla sales

![](images/clipboard-3384529088.png)

Mostramos la tabla sales

![](images/clipboard-1894106849.png)

Creamos la tabla sales_details

![](images/clipboard-2886956880.png)

Mostramos la tabla sales_details

![](images/clipboard-1178311380.png)

Mostramos todas la tablas

![](images/clipboard-756831793.png)

mostramos el resultado final

![](images/clipboard-685394537.png)

Despues haremos el proceso de ingenieria a la inversa para asi poder tener el DDL de la base de datos, primera mente Nos metemos en database en las pestañitas de la parte superior

![](images/clipboard-3587364501.png)

Posteriormente pulsamos donde dice Reverse engineer

![](images/clipboard-287402156.png)

Luego se nos abrira una nueva pestaña, la cueal le daremos next, y nos pedira la contraseña

![](images/clipboard-3850052187.png)

Despues de agregarla exitosamente, Le Pulsaremos "next", elegimos la base de datos, y seguiremos pulsando "next", despues le daremos finish, Cuantas veces se nos permina,  y se nos mostrara nuestro diagrama entidad relacion de la base de datos en este caso "ferrohogar"

![](images/clipboard-4113138852.png)

----pasos de MySQL en Dbeaver ——

vermos que la base de datos se encuentra creada

![](images/clipboard-815666929.png)

Borramos la base de datos de nuevo

![](images/clipboard-265595502.png)

Acto seguido necesitamos crear nuestra base de datos de nuevo, para ello, le tenemos que dar click derechoen databases, despues en crear nuevo database

![](images/clipboard-3572644121.png)

se nos abre una ventana donde podemos agregarle el normbre de nuestra base de datos, en este caso "ferrohogar"

![](images/clipboard-3884072125.png)

despues de presionarle aceptar se nos creara la interfaz de nuestra base de datos en la parte izquierda y al ingresar podemmos ver unas pestañas, donde veremos "tables", y al darle click derecho se nos abrira unas opciones donde podremos empezar a crear nuestras tablas

![](images/clipboard-1361341561.png)

despues de darle crear nuevo table, se nos abrira una pestaña donde podremos agregar los datos de nuestra tabla como el nombre de la tabla, al darle click derecho donde esta, o donde se encuentra el espacio vacio en columnas, nos aparecera la opcion de agregar columnas, donde podremos agregar los campos de las tablas

![](images/clipboard-1930449439.png)

al presionarle a crear nueva columna se nos abrira una pestaña nueva donde como dije anteriormente, agregaremos los datos de la columna

![](images/clipboard-2436827942.png)

si seguimos los pasos, y agregamos los datos correspondientes podremos crear nuestra primera tabla facilmente, rellenando datos importante, como primary key, unikey,not null, todo correspondiente a nuestros datos necesarios, en m caso mi primera tabla quedo tal que asi

![](images/clipboard-65929696.png)

despues de terminar de rellenar las colimnas con los campos necesarios, para guardar los datos, tendremos que presionar la "x" que se encuentra en la pestaña que estamos usando, nos pedira confirmacion para guardar la informacion, le tendremos que decir que si, acto seguido aparecera la siguiente ventana

![](images/clipboard-4129617033.png)

confirmandonos el codigo sql, que hemos creado de forma mas flexible, nosotros confirmaremos los datos, y le daremos ejecutar para continuar, despues de eso se nos creara automaticamente nuestra tabla, y se vera reflejada en la seccion de la parte izquierda de dbeaver tal que asi

![asi confirmandonos nuestra operacion exitosa, y asi continuaremos con todas nuestras tablas](images/clipboard-3810725484.png)

----

para la tabla suppliers queda tal que asi

![](images/clipboard-768911424.png)

asi queda la tabla clients

![](images/clipboard-187954890.png)

asi queda la tabla employee

![](images/clipboard-2363009743.png)

en products la tabla tamvien es igual, la diferencia es que tenemos que referenciar las forenkeys , y llo hacemos de la siguiente manera

![](images/clipboard-3775367095.png)

nos desplazamos a el apartado de foreingkeys

![](images/clipboard-524485642.png)

como se ve en la imagen ya cree una llave, pero aun asi falta otra llkave foranea

![presionamos click derecho y creamos foreign key](images/clipboard-2814819909.png)

se nos abrira la siguiente pestaña done tendremos que elegir la tabla a lacual se le referenciara, respecto a donde estamos ![](images/clipboard-796998641.png)

en nuestro caso elegiremos "suppliers" y nos aparecera opciones para elegir que dato de nuestra columna de nuestra tabla elegiremos que se referencie con el id de la tabla "suppliers"

![](images/clipboard-220581535.png)

entonces en actions elegiremos la opcion restrict, en ambos lados

![](images/clipboard-3650662639.png)

nos quedara tal que asi

![](images/clipboard-1840106298.png)

de resto todas las tablas son el mismo proceso, y asi queda la tabla products

![](images/clipboard-4111055295.png)

tal que asi queda la tabla sales

![](images/clipboard-4166020518.png)

tal que asi queda la tabla sales_details

![](images/clipboard-3606981175.png)

y para finalizar asi queda la referencia y/o el diagrama final

![](images/clipboard-1576969452.png)
