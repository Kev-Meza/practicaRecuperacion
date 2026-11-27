<div align="center">
  <img src="./img/Logo-TecNM-2017.png" width="200" align="left">
  <img src="./img/logo-itma2.png" width="100" align="right">
  <br><br><br><br>
  
  <h2>Tecnológico Nacional de México</h2>
  <h2>Instituto Tecnológico de Milpa Alta II</h2>
  <br>

  **Ingeniería en Sistemas Computacionales** <br>
  <br>

  **Quinto Semestre**  <br>
  <br>

  **Administración de base de datos**  <br>
  <br>

  **Diego Alberto Bollas Paredes**  <br>
  <br>

  <h2 >Anexo:</h2>
  <h2>Investigacion de terminos y herramientas desconocidas</h2> 
  <br>

  **Kevin Castañeda Meza**
  <br>

  <div align="right">
    26-Nov-2026 
  </div>
  <br>
  
  <br><br><br>
</div>

---

<div align="center">
  <h1>Terminos y herramientas desconocidas</h1> 
</div>



## Conceptos y términos

### 1. Migración de bases de datos

Es el proceso de trasladar los datos, la estructura y los objetos de una base de datos hacia otro destino, conservando la integridad de la información y su funcionamiento.

### 2. Migración homogénea

Consiste en trasladar una base de datos entre sistemas que utilizan el mismo gestor de bases de datos, como MariaDB a MariaDB.

### 3. Migración heterogénea

Consiste en trasladar una base de datos entre diferentes gestores, como PostgreSQL, Oracle o SQL Server hacia MariaDB. Puede requerir modificaciones en los tipos de datos y las instrucciones SQL.

### 4. Big Bang

Es una estrategia de migración en la que se detiene temporalmente el sistema para trasladar toda la información en un solo proceso.

### 5. Migración por fases

Consiste en trasladar la información de manera gradual, ya sea por tablas o módulos, en lugar de migrar todo el sistema al mismo tiempo.

### 6. Migración con replicación

Consiste en mantener sincronizadas las bases de datos de origen y destino antes de realizar el cambio definitivo, reduciendo el tiempo de interrupción del servicio.

### 7. Checksum

Es un valor calculado a partir del contenido de una tabla que permite comparar sus datos con los de otra tabla. Se utiliza para comprobar que la información no haya sufrido modificaciones durante la migración.

### 8. Integridad referencial

Es una propiedad que garantiza que las relaciones entre las tablas sean válidas mediante el uso de claves foráneas.

### 9. Charset

Es el conjunto de caracteres que puede almacenar una base de datos, incluyendo letras, números, símbolos y caracteres especiales.

### 10. Collation

Es el conjunto de reglas que determina cómo se comparan y ordenan los caracteres dentro de una base de datos.

### 11. UTF8MB4

Es una codificación de caracteres que permite almacenar caracteres Unicode, incluyendo letras de diferentes idiomas, símbolos y emojis.

### 12. Cutover

Es el momento en el que se realiza el cambio definitivo de la base de datos de origen hacia la base de datos de destino.

### 13. Plan de rollback

Es un conjunto de procedimientos que permite regresar al sistema original en caso de que ocurra algún problema durante la migración.

### 14. DEFINER

Identifica la cuenta de usuario asociada a la definición de objetos como vistas, procedimientos y triggers. Si dicha cuenta no existe en el destino, pueden presentarse problemas de permisos o funcionamiento.

### 15. CSV (Comma-Separated Values)

Es un formato de archivo de texto que permite almacenar información mediante valores separados por delimitadores, normalmente comas. Se utiliza para transferir información entre diferentes sistemas.

### 16. ETL (Extract, Transform, Load)

Es un proceso que consiste en extraer información de un origen, transformarla según las necesidades del destino y posteriormente cargarla en otro sistema.

### 17. Downtime

Es el periodo durante el cual un sistema permanece fuera de servicio debido a mantenimiento, fallos o procesos de migración.

### 18. Migración In-place

Consiste en actualizar la versión del gestor de bases de datos directamente en el mismo servidor, sin trasladar la información hacia otro equipo.

### 19. `--single-transaction` 
Permite generar una exportación consistente de las tablas InnoDB sin bloquearlas durante todo el proceso. 

### 20. `--databases` 
Incluye las instrucciones para crear y utilizar la base de datos original. 

### 21. `--all-databases` 
Permite exportar todas las bases de datos del servidor. 

### 22. `--routines` 
Incluye los procedimientos almacenados y las funciones. 

### 23. `--triggers` 
Incluye los disparadores de las tablas. 

### 24. `--events` 
Incluye los eventos programados. 

### 25. `--no-data` 
Exporta únicamente la estructura, sin los registros. 

### 26. `--no-create-info` 
Omite las instrucciones de creación de las tablas durante la exportación. 

### 27. `CHECKSUM TABLE` 
Calcula un valor de comprobación del contenido de una tabla. 

### 28. `CHECK TABLE` 
Comprueba la existencia de errores en una tabla. 

### 29. `SHOW CREATE TABLE` 
Muestra las instrucciones utilizadas para definir la estructura de una tabla. 

### 30. `ANALYZE TABLE` 
Actualiza las estadísticas de una tabla. 

### 31. `OPTIMIZE TABLE` 
Permite reorganizar el almacenamiento de una tabla cuando el motor lo admite. 

### 32. `LOAD DATA INFILE`
Permite importar información desde un archivo hacia una tabla. 

### 33. `INTO OUTFILE` 
Permite exportar el resultado de una consulta hacia un archivo. 

### 34. `EXPLAIN`
Muestra cómo MariaDB ejecutaría una consulta. 

### 35. `FLUSH TABLES WITH READ LOCK`
Establece un bloqueo global de lectura que impide nuevas modificaciones mientras se mantiene activo. 

---

<div align="center">
  <h1>Fuentes de consulta</h1> 
</div>

- Material de clase: *Migración de bases de datos en MariaDB*.
- Material de clase: *Práctica 2: Migración de una base de datos en MariaDB*.