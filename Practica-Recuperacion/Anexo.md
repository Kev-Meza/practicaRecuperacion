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



### 1. Propiedades ACID

Son un conjunto de propiedades que permiten garantizar el funcionamiento confiable de las transacciones de una base de datos.

- **Atomicidad:** una transacción se realiza completamente o no se realiza.
- **Consistencia:** los datos deben cumplir las reglas establecidas en la base de datos.
- **Aislamiento:** las transacciones concurrentes no deben interferir indebidamente entre sí.
- **Durabilidad:** los cambios confirmados deben conservarse incluso después de un fallo.



### 2. Redo log

Es un registro que permite recuperar los cambios confirmados que todavía no se habían escrito en los archivos de datos al momento de un fallo.

### 3. Undo log

Es un registro que almacena información anterior a las modificaciones, permitiendo deshacer los cambios de las transacciones que no fueron confirmadas.

### 4. Checkpoint

Es un punto de control que permite determinar desde dónde se debe revisar el redo log durante una recuperación.

### 5. Doublewrite buffer

Es un mecanismo que protege los datos contra escrituras incompletas cuando ocurre una caída del servidor.

### 6. Binary log (binlog)

Es un registro binario que almacena los cambios realizados en la base de datos. Se utiliza principalmente para la recuperación de información y la replicación entre servidores.

### 7. PITR (Point-in-Time Recovery)

Es un método de recuperación que combina un respaldo con los registros binarios para restaurar una base de datos hasta un momento específico, evitando ejecutar nuevamente las operaciones que provocaron el error.

### 8. Crash recovery

Es el proceso automático mediante el cual InnoDB recupera la consistencia de la base de datos después de una interrupción inesperada del servidor.

### 9. Respaldo incremental

Es una copia de seguridad que almacena únicamente los cambios realizados desde un respaldo anterior.

### 10. Replicación

Consiste en mantener copias actualizadas de una base de datos en diferentes servidores.

### 11. Failover

Es el proceso mediante el cual un servidor secundario toma el lugar del servidor principal cuando este presenta un fallo.


### 12. mariadb-dump

Herramienta que permite generar respaldos lógicos de una base de datos mediante archivos SQL que contienen su estructura y sus registros.

### 13. mariadb-backup

Herramienta que permite realizar respaldos físicos de los archivos de datos de MariaDB sin necesidad de detener el servicio durante la copia.

### 14. mariadb-binlog

Herramienta que permite leer los registros binarios y recuperar las operaciones realizadas después de un respaldo.

### 15. innodb_force_recovery

Parámetro de configuración que permite intentar iniciar MariaDB cuando existen problemas de corrupción en InnoDB, con el propósito de rescatar la información.



### 16. `--databases`
  Permite incluir la creación de la base de datos en el respaldo. 
### 17. `--master-data=2` 
Guarda el nombre y la posición del binlog como un comentario dentro del respaldo. 

### 18.  `--flush-logs`
Realiza una rotación de los registros binarios. 

### 19. `--start-position`
Indica la posición desde donde comienza la lectura del binlog.
### 20. `--stop-datetime`
Establece el límite de fecha y hora de las operaciones que se recuperarán. 
### 21. `--stop-position` 
Establece la posición donde se detendrá la lectura del binlog. 
### 22. `log_bin` 
Permite activar el registro binario. 
### 23. `server_id` 
Identifica al servidor de manera única dentro de una configuración de replicación. 
### 24. `SHOW BINARY LOGS` 
Muestra los archivos binlog disponibles. 
### 25. `COMMIT` 
Los cambios de una transacción. 
### 26. `ROLLBACK` 
Revierte los cambios no confirmados de una transacción. 

---

<div align="center">
  <h2>Fuentes de consulta</h2>
</div> 

- Material de clase: *Métodos de recuperación en MariaDB*.
- Material de clase: *Práctica 1: Recuperación de una base de datos en MariaDB*.


