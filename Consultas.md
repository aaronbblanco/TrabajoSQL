### CONSULTAS


***1. Obtener de que pedido se saca mas dinero : 
Con este indicador obtenemos las ventas para cada pedido separadas por cada representante 
(En este caso usaremos el Representante con el codigo '103'.***
```mysql
SELECT CodPedido as CodigoDelPedido, sum(LineasDePedido.cantidad*LineasDePedido.precioVenta) AS VentasPorPedido 
FROM Lineaspedido AS LineasDePedido
WHERE LineasDePedido.CodPedido IN ( SELECT CodPedido FROM Pedidos AS Pedidos 
                                 WHERE Pedidos.CodRepresentante = '103')
                                 GROUP BY LineasDePedido.CodPedido;
```
>El primer 'SELECT' selecciona el codigo del pedido y la suma de las cantidas por su precio y las nombra como  ventas por pedido.
El segundo 'SELECT' Selecciona el codigo del pedido de pedidos donde el codigo del representante sea igual a 103 y los agrupa por los codigos del pedido.

***2.Empleados que cobran mas que el 60% del objetivo de la oficina :
Este indicador selecciona el codigo de oficina y la ciudad donde el sueldo de todos los empleados supere el 60% del objetivo de la oficina*** 
```mysql
SELECT CodOficina As CodigoDeOficina , ciudad AS Ciudad
FROM Oficinas AS Oficinas
WHERE 0.6*Objetivo < (SELECT min(sueldo)
                                      FROM Empleados AS Empleados
                                      WHERE (Empleados.Oficina = Oficinas.CodOficina)
                                      AND (Empleados.Categoria = 'Representantes')
                                      GROUP BY Empleados.Oficina);
 ```
 > La subconsulta selecciona el sueldo minimo de los empleados y comprueba si superan el 50% de los objetivos de la oficina , agrupandolos por oficinas.
 La consulta mostrara el codigo de Oficina y la ciudad de dicha oficina.

 ***3.Productos que no se vendieron : 
 Esta Indicador selecciona los productos que no se hayan vendido***
 ```mysql
SELECT * FROM Productos
WHERE NOT EXISTS (SELECT *
                        FROM LineasPedido
                        WHERE IdFabricante = Fabricante
                        AND Idproducto = Producto);
 ```
 >La consulta selecciona de la tabla productos los productos que no esten en la lista de pedidos , es decir , que nunca se vendiese ninguna cantidad de dicho producto.
 
 ***3.1 [ CONSULTA 3 CON JOIN]  Esta sería la misma manera de hacer La consulta anterior mediante el uso de left join.***
 ```mysql
 SELECT *
FROM Productos
LEFT JOIN LineasPedido
ON IdFabricante = Fabricante AND Idproducto = Producto
WHERE Codpedido IS NULL;
``` 
>Mostraria los mismos datos que el KPI anterior.

***4. Mostrar los Clientes que su limite de credito sea superior en este caso a 2500 euros***
```mysql
SELECT *
FROM Pedidos
WHERE CodCliente IN (SELECT CodCliente
                      FROM Clientes
                      WHERE LimiteCredito > 2500);
 ```
 >La consulta muestra toda la de tabla de pedidos donde el limite de Credito del cliente es superior a 2500.
 
 ***4.1 [CONSULTA 4 SIMPLE] La misma consulta mediante una union sencilla : 
 La consulta anterior se puede hacer tambien mediante una consulta simple.***
 ```mysql
 SELECT *
 FROM Pedidos Ped, Clientes Clien
 WHERE Ped.CodCliente = Clien.CodCliente
 AND LimiteCredito > 2500;
 ```
 >Mostraria los mismos datos que el KPI anterior.
 
 ***4.2 [CONSULTA 4 SIMPLE] La misma consulta mediante una union con join : 
 La consulta anterior se puede hacer tambien mediante una consulta .***
 ```mysql
 SELECT *
FROM Pedidos INNER JOIN Clientes USING(CodCliente)
WHERE LimiteCredito > 2500;
```
>Mostraria los mismos datos que el KPI anterior.


### RESULTADOS 

***Consulta 1 ->***



<img src="https://github.com/aaronbblanco/TrabajoSQL/blob/master/RESULTADOS/sql1.PNG">
<img src="">
>***Este sería el resultado que obtenemos al hacer la consulta 1.***
