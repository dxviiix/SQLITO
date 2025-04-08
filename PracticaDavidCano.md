# Practica David Cano Nieto

## Ejercicio 1
> Actualiza el estado de todas las bicicletas adquiridas antes de
2020 a "mantenimiento" si no están ya en ese estado. Asegúrate de que
solo se actualicen las bicicletas con estado "disponible" o "en uso".
```bash
UPDATE bicicletas
SET estado = 'mantenimiento'
WHERE anio_adquisicion < 2020
AND estado IN ('disponible', 'en uso');
```

## Ejercicio 2
> Elimina de la tabla Usuarios a todos los usuarios que no han
realizado ningún viaje desde el 1 de enero de 2025. Usa una subconsulta
para identificarlos.
```bash
DELETE FROM usuarios
WHERE usuario_ID NOT IN (SELECT DISTINCT usuario
						 FROM viajes
						 WHERE hora_ini >= '2025-01-01 00:00:01');
```

## Ejercicio 3
> Incrementa en un 10% el precio de las tarifas cuya duración
promedio de viajes supera las 2 horas.

```bash
UPDATE tarifas t
JOIN (SELECT tarifa_ID
	  FROM viajes
      GROUP BY tarifa_ID
      HAVING AVG(TIMESTAMPDIFF(MINUTE, hora_ini, hora_fin)) > 120) AS t_usadas ON t.tarifa_ID = t_usadas.tarifa_ID
SET precio = precio * 1.10;
```

## Ejercicio 4
> Elimina de la tabla Turista a todos los usuarios cuyo país de
origen sea "USA" y que no tengan viajes registrados después del 1 de
febrero de 2025. Usa un JOIN para verificar los viajes.

```bash
DELETE t
FROM turista t
LEFT JOIN viajes v ON t.usuario = v.usuario
WHERE t.pais_origen = 'Estados Unidos'
AND (v.hora_ini IS NULL OR v.hora_ini < '2025-02-01 00:00:01');
```

## Ejercicio 5
> Aumenta en un 20% el coste_total de los viajes cuya duración
supera la duración promedio de todos los viajes. Usa una subconsulta
para calcular el promedio.

```bash
UPDATE viajes
SET coste_total = coste_total * 1.20
WHERE TIME_TO_SEC(TIMEDIFF(hora_fin, hora_ini)) > (SELECT AVG(TIME_TO_SEC(TIMEDIFF(hora_fin, hora_ini)))
											FROM viajes);
```

## Ejercicio 6
> Borra de Bicicletas todas las bicis que no tienen viajes
asociados y cuyo estado es "disponible".
```bash
DELETE FROM bicicletas
WHERE estado = 'disponible'
AND matricula NOT IN (SELECT DISTINCT matricula
					  FROM viajes);
```
