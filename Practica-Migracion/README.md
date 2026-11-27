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

  <h2 >Practica:</h2>
  <h2>Migracion base de datos en MariaDB</h2> 
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

La migración de bases de datos es un proceso que permite trasladar la información, la estructura y los diferentes objetos de una base de datos hacia otro destino, conservando la integridad de los datos y su correcto funcionamiento.

En esta práctica se utilizará MariaDB para realizar la migración de una base de datos hacia una nueva dentro del mismo servidor, utilizando herramientas de exportación e importación desde la terminal. Posteriormente, se realizarán diferentes comprobaciones para verificar que los registros, las vistas y las restricciones de las tablas se hayan trasladado correctamente, sin pérdida de información.

---

<div align="center">
  <h1>Objetivo</h1>
</div>

Migrar una base de datos completa (estructura, datos y vista) hacia una **base nueva** usando solo MariaDB desde la terminal, y **validar** que la migración quedó íntegra.

---

<div align="center">
  <h1>Desarrollo</h1>
</div>

## Escenario


```
┌──────────────┐    mariadb-dump     ┌──────────────────┐
│  biblioteca  │ ──────────────────► │ biblioteca_nueva │
│   (origen)   │    archivo .sql     │    (destino)     │
└──────────────┘                     └──────────────────┘
```

> Para mantenerla sencilla, origen y destino están en el mismo servidor. Si tuvieras un segundo servidor, el proceso es idéntico: solo copias el archivo `.sql` y lo importas allá.

---

## Paso 1. Crear la base de datos de origen

```bash
mariadb -u root -p
```

```sql
CREATE DATABASE biblioteca;
USE biblioteca;

CREATE TABLE autores (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    pais VARCHAR(50)
);

CREATE TABLE libros (
    id INT AUTO_INCREMENT PRIMARY KEY,
    titulo VARCHAR(150) NOT NULL,
    anio INT,
    autor_id INT NOT NULL,
    FOREIGN KEY (autor_id) REFERENCES autores(id)
);

INSERT INTO autores (nombre, pais) VALUES
('Gabriel Garcia Marquez', 'Colombia'),
('Julio Cortazar', 'Argentina'),
('Octavio Paz', 'Mexico');

INSERT INTO libros (titulo, anio, autor_id) VALUES
('Cien anos de soledad', 1967, 1),
('El amor en los tiempos del colera', 1985, 1),
('Rayuela', 1963, 2),
('El laberinto de la soledad', 1950, 3);

CREATE VIEW v_libros_autor AS
SELECT l.titulo, l.anio, a.nombre AS autor
FROM libros l
JOIN autores a ON a.id = l.autor_id;

SELECT * FROM v_libros_autor;
```

**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20210332.png)

**Resultado esperado:** la vista muestra los 4 libros con su autor. ✅

Salir con `EXIT;`

## Paso 2. Registrar los datos del origen (para comparar después)

```bash
mariadb -u root -p -e "
SELECT COUNT(*) AS autores FROM biblioteca.autores;
SELECT COUNT(*) AS libros FROM biblioteca.libros;
CHECKSUM TABLE biblioteca.autores, biblioteca.libros;"
```
**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20210441.png)

**Resultado esperado:** 3 autores, 4 libros y un valor de `Checksum` para cada tabla. ✅

**Anota** estos resultados.

**1. Checksum autores:** `4192191419`

**2. Checksum libros:** `1759993422`

## Paso 3. Hacer un respaldo de seguridad antes de migrar

```bash
mariadb-dump -u root -p --databases biblioteca > respaldo_previo.sql
```

Comprobar que se creó:

```bash
ls -lh respaldo_previo.sql
```
**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20210810.png)

> Regla de oro: **siempre** se respalda antes de migrar.

## Paso 4. Exportar la base de datos

Se exporta **sin** la opción `--databases` para que el archivo no cree ni use la base original y podamos importarlo en otra con distinto nombre:

```bash
mariadb-dump -u root -p --single-transaction --routines --triggers --events biblioteca > biblioteca_migracion.sql
```

**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20210929.png)

## Paso 5. Crear la base de datos de destino

```bash
mariadb -u root -p -e "CREATE DATABASE biblioteca_nueva;"
```
**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20211012.png)

## Paso 6. Importar en el destino

```bash
mariadb -u root -p biblioteca_nueva < biblioteca_migracion.sql
```
**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20211052.png)

Si no aparece ningún error, la importación fue exitosa.

## Paso 7. Validar la migración

### 7.1 Comparar conteos y checksum

```bash
mariadb -u root -p -e "
SELECT COUNT(*) AS autores FROM biblioteca_nueva.autores;
SELECT COUNT(*) AS libros FROM biblioteca_nueva.libros;
CHECKSUM TABLE biblioteca_nueva.autores, biblioteca_nueva.libros;"
```
**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20211149.png)

**Resultado esperado:** 3 autores, 4 libros y los **mismos valores de `Checksum`** del Paso 2. ✅

### 7.2 Verificar que la vista funciona

```bash
mariadb -u root -p -e "SELECT * FROM biblioteca_nueva.v_libros_autor;"
```
**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20211353.png)

**Resultado esperado:** los mismos 4 libros con su autor. ✅

### 7.3 Verificar la integridad referencial

```bash
mariadb -u root -p -e "INSERT INTO biblioteca_nueva.libros (titulo, anio, autor_id) VALUES ('Prueba', 2020, 999);"
```
**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20211516.png)

**Resultado esperado:** error de clave foránea. Esto confirma que la restricción también se migró. ✅

### 7.4 Verificar la estructura

```bash
mariadb -u root -p -e "SHOW CREATE TABLE biblioteca_nueva.libros\G"
```
**Resultado:**

![Resultado](./img/Screenshot%202026-11-26%20211614.png)

## Paso 8. Registrar el resultado

Completa esta tabla:

| Comprobación | Origen | Destino | ¿Coincide? | 
|---|---|---|---|
| Filas en `autores` | 3 | 3 | ✅ |  
| Filas en `libros` | 4 | 4 | ✅ |
| Checksum `autores` | `4192191419` | `4192191419` | ✅ |
| Checksum `libros` | `1759993422` | `1759993422` | ✅ |
| Vista `v_libros_autor` funciona | Sí | Sí | ✅|
| Clave foránea activa | Sí | Sí | ✅|

Si todo coincide, la migración fue exitosa.

**1. Filas y cheksum de origen:**

![Imagen](./img/Screenshot%202026-11-26%20213510.png)

**2. Filas y cheksum de destino:**

![Imagen](./img/Screenshot%202026-11-26%20213621.png)

**3. Vista de origen:**

![Imagen](./img/Screenshot%202026-11-26%20213729.png)

**4. Vista de destino:**

![Imagen](./img/Screenshot%202026-11-26%20213802.png)

**5. Clave foranea de origen:**

![Imagen](./img/Screenshot%202026-11-26%20213836.png)

**6. Clave foranea de destino:**

![Imagen](./img/Screenshot%202026-11-26%20213900.png)

---

<div align="center">
  <h1>Conclusion</h1>
</div>

Durante esta práctica aprendí a realizar la migración de una base de datos utilizando las herramientas que proporciona MariaDB desde la terminal. Comprendí la importancia de realizar un respaldo antes de iniciar una migración, ya que permite recuperar la información en caso de que ocurra algún error durante el proceso.

También aprendí a exportar e importar una base de datos con un nombre diferente, conservando su estructura, sus registros, sus vistas y sus restricciones. Mediante el conteo de registros, la comparación de los valores checksum y las pruebas de integridad referencial, pude verificar que los elementos de la base de datos se trasladaron correctamente.

Finalmente, comprendí que una migración no consiste únicamente en trasladar información de una base de datos a otra, sino también en comprobar que los datos se conserven y que los diferentes objetos continúen funcionando correctamente. Esta práctica me permitió adquirir conocimientos que pueden aplicarse al momento de trasladar bases de datos entre diferentes servidores.

