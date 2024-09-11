# Airline Database Project
## Description


This project involves designing and implementing a relational database for an airline company. The database manages information about aircraft, pilots, crew members, flights, bases, destinations, and maintenance schedules.
Database Schema

## The database consists of the following tables:

avion (Aircraft)

id: int (Primary Key)
nombre: varchar


piloto (Pilot)

id: int (Primary Key)
nombre: varchar
horas_de_vuelo: bigint


miembro (Crew Member)

id: int (Primary Key)
nombre: varchar


vuelo (Flight)

id: int (Primary Key)
hora: varchar
id_piloto: int (Foreign Key to piloto)
id_avion: int (Foreign Key to avion)
id_destino: int (Foreign Key to destino)


destino (Destination)

id: int (Primary Key)
aeropuerto: varchar
id_ciudad: int (Foreign Key to ciudad)
id_pais: int (Foreign Key to pais)


ciudad (City)

id: int (Primary Key)
nombre: varchar


pais (Country)

id: int (Primary Key)
ciudad: varchar


base (Base)

id: int (Primary Key)
nombre: varchar


base_avion (Aircraft Base)

id: int (Primary Key)
nombre: varchar


avion_b (Aircraft-Base Relationship)

id_base: int (Foreign Key to base_avion)
id_avion: int (Foreign Key to avion)


tripulacion_base (Crew-Base Relationship)

id_base: int (Foreign Key to base)
id_miembro: int (Foreign Key to miembro)


piloto_base (Pilot-Base Relationship)

id_base: int (Foreign Key to base)
id_piloto: int (Foreign Key to piloto)


tripulacion_vuelo (Flight Crew)

id_vuelo: int (Foreign Key to vuelo)
id_miembro: int (Foreign Key to miembro)


mantenimiento (Maintenance)

id_avion: int (Foreign Key to avion)
dia: date
id_base: int (Foreign Key to base)



## Key Features

Manages aircraft, pilots, and crew members information
Tracks flights with their destinations, assigned aircraft, and crew
Handles base assignments for aircraft, pilots, and crew members
Records maintenance schedules for aircraft
Manages destinations with associated cities and countries

## Queries
1. Listado de todos los vuelos con los nombres del piloto y todos los miembros de la tripulación asignados a cada vuelo:

   ```sql
   SELECT v.id, v.hora, v.id_avion, v.id_piloto, p.nombre, (tv.id_miembro) FROM vuelo v JOIN piloto p ON v.id_piloto = p.id JOIN tripulacion_vuelo tv ON tv.id_vuelo = v.id GROUP BY v.id, v.hora, v.id_avion, v.id_piloto, p.nombre, tv.id_miembro;
   ```

   

2. Cantidad de vuelos realizados desde cada base con la suma de las horas de vuelo de los pilotos involucrados:

   ```sql
   SELECT ba.id, ba.nombre, SUM(v.id_avion) AS 'vuelos realizados en la base', SUM(p.horas_de_vuelo) AS 'horas de vuelo' FROM vuelo v JOIN piloto p ON p.id = v.id_piloto JOIN avion_b ab ON ab.id_avion = v.id_avion JOIN base_avion ba ON ba.id = ab.id_base GROUP BY ba.id, ba.nombre;
   ```

   

3. Promedio de horas de vuelo de los pilotos que han realizado vuelos en un tipo específico de avión:

   ```sql
   SELECT v.id_avion, AVG(p.horas_de_vuelo) AS 'horas de vuelo en el avion' FROM vuelo v JOIN piloto p ON p.id = v.id_piloto GROUP BY v.id_avion HAVING v.id_avion = 1;
   ```

   

4. Listado de vuelos con el total de miembros de la tripulación asignados y la suma de las horas de vuelo del piloto:

   ```sql
   SELECT v.id, v.hora, v.id_avion, p.horas_de_vuelo AS 'horas de vuelo del piloto', COUNT(tv.id_miembro) AS 'total de miembros' FROM vuelo v JOIN piloto p ON p.id = v.id_piloto JOIN tripulacion_vuelo tv ON tv.id_vuelo = v.id GROUP BY v.id, v.hora, v.id_avion, p.horas_de_vuelo;
   ```

   

5. Listado de las bases y el número de vuelos realizados y el tipo de avión más usado en cada base:

   ```sql
   SELECT ba.id, ba.nombre, COUNT(v.id_avion) AS 'vuelos realizados', ab.id_avion FROM base_avion ba JOIN avion_b ab ON ba.id = ab.id_base JOIN vuelo v ON v.id_avion = ab.id_avion GROUP BY ba.id, ba.nombre, ab.id_avion;
   ```

   

6. Listado de los pilotos que han realizado más de un determinado número de vuelos y la suma de sus horas de vuelo:

   ```sql
   SELECT p.id, p.nombre, COUNT(v.id) AS 'vuelos realizados', p.horas_de_vuelo FROM piloto p JOIN vuelo v ON v.id_piloto = p.id GROUP BY p.id, p.nombre, p.horas_de_vuelo HAVING COUNT(v.id) > 0;
   ```

   

7. Listado de aviones y el número de veces que han sido usados en vuelos, junto con el total de horas de vuelo de los pilotos que los han volado:

   ```sql
   
   ```

   

8. Listado de vuelos realizados en los últimos tres meses, con el nombre del piloto y la cantidad de miembros de tripulación:

   

   ```sql
   
   ```

   

9. Promedio de horas de vuelo de los pilotos y el número de vuelos realizados por cada tipo de avión en cada base:

   ```sql
   
   ```

   

10. Listado de vuelos, piloto y los miembros de tripulación, mostrando solo aquellos vuelos donde la tripulación está compuesta por al menos tres miembros:

    ```sql
    
    ```

    

11. Listado de pilotos con más de 1000 horas de vuelo:

    ```sql
    
    ```

    

12. Listado de vuelos y el nombre del piloto y miembros de tripulación asignados:

    ```sql
    
    ```

    

13. Listado de vuelos y el tipo de avión utilizado:

    ```sql
    
    ```

    

14. Listado de todos los vuelos que salen de una base específica:

    ```sql
    
    ```

    

15. Listado de vuelos y los nombres de los miembros de la tripulación asignados:

    ```sql
    SELECT tv.id_vuelo, tv.id_miembro, m.nombre FROM tripulacion_vuelo tv JOIN miembro m ON tv.id_miembro = m.id ORDER BY tv.id_vuelo;
    ```

    

16. Cantidad de vuelos realizados por cada tipo de avión:

    ```sql
    SELECT a.id, a.nombre, COUNT(v.id_avion) AS 'vuelos realizados' FROM avion a JOIN vuelo v ON v.id_avion = a.id GROUP BY a.id, a.nombre;
    ```

    

17. Cantidad de vuelos realizados por cada piloto:

    ```sql
    SELECT p.id, p.nombre, COUNT(v.id_piloto) AS 'vuelos realizados' FROM piloto p JOIN vuelo v ON v.id_piloto = p.id GROUP BY p.id, p.nombre;
    ```

    

18. Cantidad total de horas de vuelo acumuladas por todos los pilotos:

    ```sql
    select sum(horas_de_vuelo) from piloto;
    ```

    

19. Listado de todos los miembros de tripulación:

    ```sql
    select id, nombre from miembro;
    ```

    

20. Listado de todos los pilotos con sus horas de vuelo totales:

    ```sql
    select id, nombre, sum(horas_de_vuelo) from piloto group by id;
    ```

    
