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

  <h2 >Cuestionario:</h2>
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
  <h1>Responde las siguientes preguntas</h1>
</div>


**1. ¿Por qué se hace un respaldo antes de migrar?**
Para evitar la pérdida de información en caso de que ocurra algún error durante la migración. De esta manera, se puede recuperar la base de datos original si es necesario.

**2. ¿Para qué sirve la opción `--single-transaction`?**
Sirve para generar un respaldo consistente de las tablas que utilizan el motor InnoDB, sin necesidad de bloquearlas durante todo el proceso de exportación.

**3. ¿Qué diferencia hay entre exportar con y sin `--databases`?**
Al utilizar --databases, el archivo SQL incluye las instrucciones para crear y utilizar la base de datos original. Al exportar sin esta opción, podemos importar la estructura y los datos en otra base de datos con un nombre diferente.

**4. ¿Qué dos pruebas permiten confirmar que los datos migraron sin pérdidas?**
El conteo de registros mediante SELECT COUNT(*) y la comparación de los valores obtenidos mediante CHECKSUM TABLE. Estas pruebas permiten comprobar que la cantidad de registros y el contenido de las tablas coincidan entre el origen y el destino.

**5. Si el destino estuviera en otro servidor, ¿qué paso adicional sería necesario?**
Sería necesario transferir el archivo SQL generado durante la exportación al servidor de destino. Esto se puede realizar utilizando una herramienta como scp, para posteriormente importar el archivo en MariaDB.