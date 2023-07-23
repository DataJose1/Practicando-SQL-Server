# Practicando-SQL-Server
**Base de datos: Adventure Works**  
**Motor de base de datos: SQL Server**  
  
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
