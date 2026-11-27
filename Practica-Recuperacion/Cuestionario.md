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
<div align = "center">
    <h2>Responde las siguientes preguntas</h2>
</div>


**1. ¿Qué datos se habrían perdido si solo hubieras usado el respaldo de la Parte A?**
Se habrian perdido los dos datos que se agregaronn despues del respaldo.

**2. ¿Para qué sirve la opción `--master-data=2`?**
Guarda en el archivo la posición del binlog (clave para PITR).

**3. ¿Por qué es necesario activar el binlog **antes** de que ocurra el error?**
Porque el binlog se encarga de registrar los cambios realizados en la base de datos. Si se activa después del error, no se tendrán registrados los cambios anteriores y no será posible recuperarlos mediante este método.

**4. ¿Por qué se esperaron 5 segundos antes y después de anotar la hora?**
Para tener una separación de tiempo entre la inserción de los nuevos registros y la eliminación accidental. Y tener un punto de recuperacion.

<div align="center">
  <h2>Complementos</h2>
</div>

[Practica](/README.md)

[Anexo de investigación](/Anexo.md)
