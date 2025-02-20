# Ejercicios vistas



## Ejercicio 1
> Esta vista muestra a los clientes ordenados por edad de mayor a menor.
```bash
CREATE OR REPLACE VIEW ClientesPorEdad AS
SELECT cliente_id, nombre, apellido_1, edad
FROM clientes
ORDER BY edad DESC;
```

## Ejercicio 2
> Muestra todos los vehículos agrupados por marca y modelo.
```bash
CREATE OR REPLACE VIEW VehiculosMarcaModelo AS
SELECT marca, modelo, count(modelo) as 'Cantidad'
FROM vehiculos
GROUP BY marca, modelo
ORDER BY marca DESC;
```

## Ejercicio 3
> Esta vista combina la información de los clientes con sus números de teléfono
```bash
CREATE OR REPLACE VIEW TelefonosClientes AS
SELECT c.nombre, c.apellido_1, t.telefono
FROM clientes c join telefonos t
ON c.cliente_id = t.cliente;
```

## Ejercicio 4
> Vista para ver las órdenes de trabajo que aún no han sido entregadas (fecha_entrega es en el futuro respecto a la fecha actual).

```bash
CREATE OR REPLACE VIEW OrdenesTrabajoPendiente AS
SELECT *
FROM ordenesdetrabajo
WHERE fecha_entrega > DATE(now());
```

## Ejercicio 5
> Calcula el coste total de cada orden de trabajo sumando el costo de todos los
servicios asociados

```bash
CREATE OR REPLACE VIEW CostoTotalOrdenTrabajo AS
SELECT odt.orden_id, s.nombre, sum(s.coste)
FROM ordenesdetrabajo odt join serviciosporordendetrabajo sodt
on odt.orden_id = sodt.orden_id join servicios s
on s.servicio_id = sodt.servicio_id
group by odt.orden_id, s.nombre
ORDER BY  odt.orden_id asc;
```

## Ejercicio 6
> Mostrar cuántas horas ha trabajado cada mecánico en total a través de todas las órdenes.
```bash
CREATE OR REPLACE VIEW MecanicosHorasTrabajadas AS
SELECT m.nombre, sum(modt.horas) as 'Horas Totales'
FROM mecanicosporordendetrabajo modt join mecanicos m
on modt.mecanico = m.mecanico_id
group by mecanico;
```

## Ejercicio 7
> Vista que combina información sobre los vehículos, incluyendo la edad del cliente dueño del vehículo y el año de fabricación del coche, agrupado por marca
```bash
CREATE OR REPLACE VIEW AnalisisVehciulosFabricacionCliente AS
SELECT v.marca, v.matricula, v.modelo, c.cliente_id, c.nombre, c.edad
FROM vehiculos v join clientes c
on v.cliente = c.cliente_id
group by v.marca, v.matricula, v.modelo;
```