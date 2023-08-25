# Practicando-SQL-Server
**Base de datos:** Adventure Works  (Empresa ficticia que se dedica a la fabricación y venta de bicicletas)  

**Motor de base de datos:** SQL Server  
  
Mostrar los empleados que tienen más de 90 horas de vacaciones:  
````
SELECT 
  BusinessEntityID, 
  VacationHours 
FROM 
  HumanResources.Employee 
WHERE 
  VacationHours > 90
````
Mostrar las personas cuyo nombre tenga una C o c como primer carácter,
cualquier otro como segundo carácter, ni d ni e ni f ni g como tercer carácter,
cualquiera entre j y r o entre s y w como cuarto carácter y el resto sin restricciones:  
````
SELECT 
  FirstName 
FROM 
  Person.Person 
WHERE 
  FirstName LIKE '[C-c]_[^d-g][j-w]%'
````
Mostrar el nombre, precio y color de los accesorios para asientos de las bicicletas
cuyo precio sea mayor a 100 pesos:  
````
SELECT 
  Name, 
  ListPrice, 
  Color 
FROM 
  Production.Product 
WHERE 
  Name LIKE '%seat%' 
  AND ListPrice > 100
````
Mostrar el id de los empleados, si tiene salario deberá mostrarse descendente, de lo contrario, ascendente:  
````
SELECT 
  BusinessEntityID, 
  SalariedFlag 
FROM 
  HumanResources.Employee 
ORDER BY 
  CASE SalariedFlag WHEN 1 THEN BusinessEntityID END DESC, 
  CASE WHEN SalariedFlag = 0 THEN BusinessEntityID END
````
Mostrar el código de subcategoría y el producto más barato de cada una de esas subcategorías:   
````
SELECT 
  ProductSubCategoryID, 
  ListPrice, 
  ProductID 
FROM 
  Production.Product pp 
WHERE 
  ListPrice = (
    SELECT 
      MIN (ListPrice) 
    FROM 
      Production.Product pp1 
    WHERE 
      pp.ProductSubcategoryID = pp1.ProductSubcategoryID
  ) 
ORDER BY 
  ProductSubcategoryID
````
Mostrar las subcategorías de los productos que tienen dos o más productos que cuestan menos de $150:  
````
SELECT 
  ProductSubcategoryID, 
  COUNT(ProductSubcategoryID) AS Cantidad 
FROM 
  Production.Product 
WHERE 
  ListPrice < 150 
GROUP BY 
  ProductSubcategoryID 
HAVING 
  COUNT(ProductSubcategoryID) > 2
````
Mostrar todos los códigos de subcategorías existentes junto con la cantidad para los productos cuyo precio de lista sea mayor a $70 y el precio promedio sea mayor a $300:  
````
SELECT 
  ProductSubcategoryID, 
  COUNT(ProductSubcategoryID) AS Cantidad, 
  AVG(ListPrice) AS Promedio 
FROM 
  Production.Product 
WHERE 
  ListPrice > 70 
GROUP BY 
  ProductSubcategoryID 
HAVING 
  AVG(ListPrice) > 300
````
Mostrar los precios de venta de aquellos productos donde el precio de venta sea inferior al precio de lista recomendado para ese producto, ordenados por nombre de producto:  
````
SELECT 
  DISTINCT p.ProductID, 
  p.Name Producto, 
  p.ListPrice 'Precio de lista recomendado', 
  sd.UnitPrice 'Precio de venta' 
FROM 
  Sales.SalesOrderDetail AS sd 
  JOIN Production.Product AS p ON sd.ProductID = p.ProductID 
  AND sd.UnitPrice < p.ListPrice
ORDER BY p.Name
````
Mostrar los empleados ordenados alfabéticamente por apellido y por nombre:  
````
SELECT 
  p.LastName Apellido, 
  p.FirstName Nombre 
FROM 
  HumanResources.Employee AS e 
  JOIN Person.Person AS p ON e.BusinessEntityID = p.BusinessEntityID 
ORDER BY 
  p.LastName, 
  p.FirstName
````
Mostrar todas nombre y apellido de todas las personas y en el caso de que sean empleados también mostrar el login id, sino mostrar null:  
````
SELECT 
  DISTINCT pe.LastName Apellido, 
  pe.FirstName Nombre, 
  he.LoginID 
FROM 
  Person.Person pe 
  LEFT JOIN HumanResources.Employee he ON pe.BusinessEntityID = he.BusinessEntityID
````
Clonar estructura y datos de los campos nombre, color y precio de la lista de la tabla production.product en una tabla llamada #Productos:
````
SELECT 
  Color, 
  Name, 
  ListPrice
INTO #Productos 
FROM 
  Production.Product
````
Crear una Common Table Expression con las órdenes de venta:
````
WITH SALES_CTE (
  SalesPersonID, SalesOrderID, SalesYear
) AS (
  Select 
    SalesPersonID, 
    SalesOrderID, 
    YEAR (OrderDate) AS Año 
  FROM 
    Sales.SalesOrderHeader 
  WHERE 
    SalesPersonID IS NOT NULL
) 
SELECT 
  SalesPersonID, 
  SalesOrderID, 
  SalesYear 
FROM 
  SALES_CTE;
````
Clonar estructura y datos de los campos nombre, color y precio de lista de la tabla Production.Product en una tabla llamada Productos:
````
SELECT 
  ProductID, 
  Name, 
  Color, 
  ListPrice 
INTO Productos 
FROM 
  Production.Product
````
Aumentar un 20% el precio de lista de los productos del proveedor 1540:
````
UPDATE 
  p 
SET 
  ListPrice = ListPrice * 1.2 
FROM 
  Productos p 
  INNER JOIN Purchasing.ProductVendor v ON p.ProductID = v.ProductID 
WHERE 
  v.BusinessEntityID = 1540
````
Eliminar los productos cuyo nombre empiece con la letra m:
````
DELETE FROM 
  Productos 
WHERE 
  Name LIKE 'm%'
````
Borrar todo el contenido de la tabla Productos:
````
TRUNCATE TABLE Productos
````
