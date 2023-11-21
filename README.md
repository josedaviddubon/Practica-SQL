# Practica-SQL


---2.1
SELECT 
		dep.Nombre 'Departamento',
		min(emp.Salario) 'Mínimo',
		max(emp.Salario) 'Máximo',
		convert(decimal(10,2), (AVG(emp.Salario)))'Promedio'
		FROM Empleados emp
inner join Departamentos dep
on emp.DepartamentoId = dep.id
group by dep.Nombre
------2.2
SELECT 
		dep.Nombre 'Departamento',
		COUNT(emp.DepartamentoId) 'No. Empleados'
		FROM Empleados emp
inner join Departamentos dep
on emp.DepartamentoId = dep.id
group by dep.Nombre
having COUNT(emp.DepartamentoId) >= 3

---2.3
SELECT 
    emp.Nombre,
    DATEDIFF(MONTH, emp.FechaIngreso, GETDATE()) AS Meses
FROM Empleados emp
WHERE DATEDIFF(MONTH, emp.FechaIngreso, GETDATE()) >= 12;

-----2.4
SELECT 
    emp.Nombre 'Colaborador',
	dep.Nombre 'Departamento',
	    CASE 
        WHEN dep.Nombre = 'Mercadeo' THEN 1
        ELSE 2
    END Orden
    --DATEDIFF(MONTH, emp.FechaIngreso, GETDATE()) AS Meses
FROM Empleados emp
inner join Departamentos dep
on emp.DepartamentoId = dep.id
order by Orden

---2.5
---Subconsulta
  SELECT
        emp.Nombre AS Colaborador,
        emp.Salario,
        dep.Nombre AS Departamento,
        ROW_NUMBER() OVER (PARTITION BY emp.DepartamentoId ORDER BY emp.Salario DESC) AS PosicionSalario
    FROM Empleados emp
    INNER JOIN Departamentos dep ON emp.DepartamentoId = dep.id

SELECT
    Colaborador,
    Salario,
    Departamento
FROM (
    SELECT
        emp.Nombre AS Colaborador,
        emp.Salario,
        dep.Nombre AS Departamento,
        ROW_NUMBER() OVER (PARTITION BY emp.DepartamentoId ORDER BY emp.Salario DESC) AS PosicionSalario
    FROM Empleados emp
    INNER JOIN Departamentos dep ON emp.DepartamentoId = dep.id
) AS DosEmp
WHERE PosicionSalario <= 2;

---2.5

SELECT
	ROW_NUMBER() OVER (PARTITION BY emp.DepartamentoId ORDER BY emp.Nombre) AS Numero,
    emp.Nombre 'Colaborador',
    dep.Nombre 'Departamento'
FROM Empleados emp
INNER JOIN Departamentos dep ON emp.DepartamentoId = dep.id;
