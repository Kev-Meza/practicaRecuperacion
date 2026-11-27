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

  <h2 >Practica 1:</h2>
  <h2>Recuperacion de una base de datos</h2> 
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
  <h1>Introduccion</h1>
</div>

La recuperación de bases de datos es un proceso que permite restaurar la información después de que ocurre un fallo, una eliminación accidental o una pérdida de datos.

En esta práctica se utilizará MariaDB para realizar respaldos y restauraciones de una base de datos. También se utilizará el registro binario (binlog) para recuperar información almacenada después de haber realizado un respaldo.

---

<div align="center">
  <h1>Objetivo</h1>
</div>

Realizar un respaldo de una base de datos en MariaDB, simular una pérdida de información y aplicar diferentes métodos de recuperación utilizando la terminal.

---

<div align="center">
  <h1>Desarrollo</h1>
</div>

<div align="center">
<h2>Parte A. Respaldo y restauración</h2>
</div>

### Paso 1: Crear la base de datos y los datos

Entrar al cliente:

```bash
mariadb -u root -p
```
**Resultado:**

![Entrar al cliente](./img/Screenshot%202026-11-26%20182205.png)

Ejecutar:

```sql
CREATE DATABASE tienda;
USE tienda;

CREATE TABLE clientes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100),
    email VARCHAR(100)
);

INSERT INTO clientes (nombre, email) VALUES
('Ana Torres', 'ana@correo.com'),
('Luis Perez', 'luis@correo.com'),
('Maria Gomez', 'maria@correo.com'),
('Carlos Ruiz', 'carlos@correo.com'),
('Sofia Diaz', 'sofia@correo.com');
```
**Resultado:**

![Ejecutar](./img/Screenshot%202026-11-26%20182621.png)

Ejecutar: 

```sql
SELECT COUNT(*) FROM clientes;
```

**Resultado:**

![Ejecutar](./img/Screenshot%202026-11-26%20182839.png)

**Resultado esperado:** `5`✅

### Paso 2. Hacer el respaldo

Desde la terminal:

```bash
mariadb-dump -u root -p --databases tienda > tienda_backup.sql
```
**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20183806.png)

Comprobar que el archivo se creó:

```bash
ls -lh tienda_backup.sql
```
**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20184126.png)

**Resultado esperado:** el archivo existe y tiene un tamaño mayor a 0. ✅




### Paso 3. Simular el desastre

```bash
mariadb -u root -p
```

```sql
DROP DATABASE tienda;
SHOW DATABASES;
```

**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20184321.png)

**Resultado esperado:** la base `tienda` ya no aparece en la lista.✅

Salir con `EXIT;`

### Paso 4. Restaurar el respaldo

```bash
mariadb -u root -p < tienda_backup.sql
```
**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20184924.png)

### Paso 5. Verificar la recuperación

```bash
mariadb -u root -p -e "SELECT * FROM tienda.clientes;"
```
**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20185002.png)

**Resultado esperado:** aparecen los 5 clientes.✅

---

<div align="center">
<h2>Parte B. Recuperación de datos posteriores al respaldo mediante binlog</h2>
</div>

En la Parte A, todo lo que se hubiera guardado **después** del respaldo se habría perdido. El **binlog** registra todos los cambios y permite recuperarlos.

### Paso 1. Activar el binlog

Editar el archivo de configuración (en Debian/Ubuntu):

```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

(En otras distribuciones puede ser `/etc/my.cnf.d/server.cnf`.)

Debajo de la sección `[mariadbd]` agregar:

```ini
log_bin = /var/lib/mysql/mariadb-bin
server_id = 1
```
**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20190017.png)

Guardar y reiniciar:

```bash
sudo systemctl restart mariadb
```
**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20190316.png)

Verificar:

```bash
mariadb -u root -p -e "SHOW VARIABLES LIKE 'log_bin';"
```
**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20190413.png)

**Resultado esperado:** `log_bin = ON`✅

### Paso 2. Hacer un respaldo que guarde la posición del binlog

```bash
mariadb-dump -u root -p --databases tienda --flush-logs --master-data=2 > tienda_pitr.sql
```

Buscar la posición guardada:

```bash
grep "CHANGE MASTER" tienda_pitr.sql
```
**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20190538.png)

**Resultado esperado** (los números pueden variar):

```bash
-- CHANGE MASTER TO MASTER_LOG_FILE='mariadb-bin.000002', MASTER_LOG_POS=330;
```
✅

Anota el **archivo** y la **posición**.

- **Archivo:** `mariadb-bin.000016`
- **Posición:** `385`

### Paso 3. Agregar datos nuevos (posteriores al respaldo)

```bash
mariadb -u root -p
```

```sql
USE tienda;
INSERT INTO clientes (nombre, email) VALUES
('Pedro Nunez', 'pedro@correo.com'),
('Laura Vega', 'laura@correo.com');

SELECT COUNT(*) FROM clientes;
```
**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20191015.png)

**Resultado esperado:** `7`✅

Esperar unos 5 segundos y consultar la hora:

```sql
SELECT NOW();
```
**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20191045.png)

Anota la hora. Ejemplo: `2026-11-24 14:31:20`

- **Hora:** `2026-11-26 19:10:38`

### Paso 4. Simular el error

Esperar otros 5 segundos y ejecutar:

```sql
DELETE FROM clientes;
SELECT COUNT(*) FROM clientes;
```
**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20191259.png)

**Resultado esperado:** `0`✅

Salir con `EXIT;`

### Paso 5. Restaurar el respaldo

```bash
mariadb -u root -p < tienda_pitr.sql
```
**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20191410.png)

En este punto solo hay 5 clientes (faltan Pedro y Laura).

### Paso 6. Aplicar el binlog hasta antes del error

Usar la posición y el archivo que anotaste, y la hora anotada en el Paso 3:

```bash
sudo mariadb-binlog --start-position=385 --stop-datetime="2026-11-26 19:10:38" \
    /var/log/mysql/mysql-bin.000016 | mariadb -u root -p
```
**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20192500.png)

### Paso 7. Verificar

```bash
mariadb -u root -p -e "SELECT * FROM tienda.clientes;"
```
**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20192628.png)

**Resultado esperado:** 7 clientes, incluyendo **Pedro Nunez** y **Laura Vega**. ✅

**Parte B completada:** se recuperó todo excepto el `DELETE` accidental.

---

<div align="center">
  <h1>Conclusión</h1>
</div>

Durante esta práctica aprendí a realizar respaldos y recuperar una base de datos utilizando las herramientas que proporciona MariaDB. En la primera parte logré recuperar los cinco registros originales después de eliminar completamente la base de datos, comprobando la importancia de contar con una copia de seguridad.

En la segunda parte aprendí a utilizar el registro binario (binlog), el cual me permitió recuperar los dos registros que se agregaron después de realizar el respaldo, logrando recuperar los siete registros sin volver a ejecutar la eliminación accidental.

Finalmente, comprendí que realizar respaldos periódicamente no siempre es suficiente, ya que se pueden perder los datos generados después del último respaldo. Por ello, es importante combinar las copias de seguridad con otros métodos de recuperación, así como verificar que funcionen correctamente. Esta práctica me permitió comprender mejor la importancia de la recuperación de datos dentro de la administración de bases de datos.




