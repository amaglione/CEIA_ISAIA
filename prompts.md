# Conversación - Trabajo UX/UI Malo

Tengo que hacer un trabajo para la materia Ingenieria de software asistida por IA. El objetivo del trabajo es hacer en un unico archico html y usando unicamente como tecnologias HTML, CSS y Javascript vainilla ejemplos de diseños mal hechos. Algunos ejemplos de diseños UX y UI mal hechos serian: 1) Un campo de documento numerico donde el usuario tenga que poner su dato sin poder escribirlo. 2) Un selector de fecha con un slider para cada campo Dia , Mes y Año. 3) Un mensaje de error en color verde y con un simbolo de OK Para mi ejemplo quiero que me armes un selector de fecha de nacimiento atipico que consiste en: - 3 selectores en forma de circulo que al girar a la derecha se mueva en sentido contrario. - Cada selector sera usado para cargar un dato de la fecha que estara en formato DD/MM/AAAA - Crea un display para que el usuario vaya viendo que fecha se setea

# Selector de Fecha de Nacimiento Atípico (UX/UI Malo)

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Selector de Fecha Horrible</title>

  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: Comic Sans MS, cursive;
    }

    body {
      background: linear-gradient(135deg, #ff00cc, #3333ff);
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      overflow: hidden;
    }

    .container {
      background: yellow;
      border: 12px dashed red;
      padding: 40px;
      border-radius: 50px;
      text-align: center;
      box-shadow: 0 0 40px black;
      width: 900px;
    }

    h1 {
      color: blue;
      font-size: 42px;
      margin-bottom: 30px;
      text-shadow: 3px 3px lime;
    }

    .selectors {
      display: flex;
      justify-content: center;
      gap: 40px;
      margin-bottom: 40px;
    }

    .wheel-container {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 10px;
    }

    .label {
      background: black;
      color: white;
      padding: 10px 20px;
      border-radius: 20px;
      font-size: 22px;
    }

    .wheel {
      width: 180px;
      height: 180px;
      border-radius: 50%;
      background: radial-gradient(circle, #ffffff, #999999);
      border: 15px solid cyan;
      display: flex;
      justify-content: center;
      align-items: center;
      cursor: grab;
      user-select: none;
      position: relative;
      transition: transform 0.1s linear;
      box-shadow: inset 0 0 20px black;
    }

    .wheel::before {
      content: "↺";
      position: absolute;
      top: 15px;
      right: 20px;
      color: red;
      font-size: 35px;
      font-weight: bold;
    }

    .value {
      font-size: 40px;
      font-weight: bold;
      color: darkred;
      text-shadow: 2px 2px white;
    }

    .display {
      background: black;
      color: lime;
      font-size: 50px;
      padding: 30px;
      border-radius: 25px;
      letter-spacing: 8px;
      border: 10px groove white;
      animation: blink 0.8s infinite alternate;
    }

    @keyframes blink {
      from {
        opacity: 1;
      }
      to {
        opacity: 0.5;
      }
    }

    .instructions {
      margin-top: 25px;
      color: red;
      font-size: 24px;
      background: white;
      padding: 15px;
      border-radius: 20px;
      border: 5px dotted blue;
    }

    .warning {
      margin-top: 20px;
      font-size: 20px;
      color: green;
      font-weight: bold;
    }
  </style>
</head>
<body>

  <div class="container">
    <h1>INGRESE SU FECHA DE NACIMIENTO</h1>

    <div class="selectors">

      <div class="wheel-container">
        <div class="label">DÍA</div>
        <div class="wheel" id="dayWheel">
          <div class="value" id="dayValue">01</div>
        </div>
      </div>

      <div class="wheel-container">
        <div class="label">MES</div>
        <div class="wheel" id="monthWheel">
          <div class="value" id="monthValue">01</div>
        </div>
      </div>

      <div class="wheel-container">
        <div class="label">AÑO</div>
        <div class="wheel" id="yearWheel">
          <div class="value" id="yearValue">2000</div>
        </div>
      </div>

    </div>

    <div class="display" id="displayDate">
      01 / 01 / 2000
    </div>

    <div class="instructions">
      Gire las ruedas hacia la DERECHA para que los números se muevan hacia la IZQUIERDA.
    </div>

    <div class="warning">
      ✔ Fecha cargada correctamente
    </div>

  </div>

  <script>

    const dayWheel = document.getElementById('dayWheel');
    const monthWheel = document.getElementById('monthWheel');
    const yearWheel = document.getElementById('yearWheel');

    const dayValue = document.getElementById('dayValue');
    const monthValue = document.getElementById('monthValue');
    const yearValue = document.getElementById('yearValue');

    const displayDate = document.getElementById('displayDate');

    let day = 1;
    let month = 1;
    let year = 2000;

    function updateDisplay() {
      const dd = String(day).padStart(2, '0');
      const mm = String(month).padStart(2, '0');

      dayValue.textContent = dd;
      monthValue.textContent = mm;
      yearValue.textContent = year;

      displayDate.textContent = `${dd} / ${mm} / ${year}`;
    }

    function setupWheel(wheel, type) {
      let rotation = 0;

      wheel.addEventListener('wheel', function(e) {
        e.preventDefault();

        // UX HORRIBLE:
        // Girar hacia abajo aumenta visualmente
        // pero el valor decrementa.

        if (e.deltaY > 0) {
          rotation += 20;

          if (type === 'day') {
            day--;
            if (day < 1) day = 31;
          }

          if (type === 'month') {
            month--;
            if (month < 1) month = 12;
          }

          if (type === 'year') {
            year--;
            if (year < 1900) year = 2025;
          }

        } else {
          rotation -= 20;

          if (type === 'day') {
            day++;
            if (day > 31) day = 1;
          }

          if (type === 'month') {
            month++;
            if (month > 12) month = 1;
          }

          if (type === 'year') {
            year++;
            if (year > 2025) year = 1900;
          }
        }

        // El círculo gira al revés del movimiento esperado
        wheel.style.transform = `rotate(${-rotation}deg)`;

        updateDisplay();
      });
    }

    setupWheel(dayWheel, 'day');
    setupWheel(monthWheel, 'month');
    setupWheel(yearWheel, 'year');

    updateDisplay();

  </script>

</body>
</html>
```

## Características de mal diseño UX/UI incluidas

* Las ruedas giran al revés del movimiento del usuario.
* El usuario debe usar la rueda del mouse para cambiar valores.
* Colores extremadamente saturados y molestos.
* Tipografía poco profesional (Comic Sans).
* Animaciones innecesarias.
* Mensaje de éxito verde incluso antes de completar la fecha.
* Bordes exagerados y demasiados efectos visuales.
* No valida fechas reales.
* El año hace loop entre 1900 y 2025.
* Los selectores son demasiado grandes y poco precisos.



Quiero verlo en canvas.
Canvas Selector Fecha Nacimiento Ux Malo
No funciona. Las ruedas no giran. 
Debe ser complejo de usar pero tiene que funcionar.
Canvas Selector Fecha Nacimiento Ux Malo
Las ruedas siguen sin girar. Indicame en que lugar debo presionar para que giren.
Canvas Selector Fecha Nacimiento Ux Malo
Cambia los estilos de css para use colores mas pasteles. El objetivo es que la UX sea complicada, la UI puede ser un poco mejor.
Canvas Selector Fecha Nacimiento Ux Malo
Quita la aclaración que mover a la derecha disminuye el valor y mover a la izquierda lo aumenta. Qee eso lo descubra el usuario.
Canvas Selector Fecha Nacimiento Ux Malo
Bien. Ahora agreguemos un boton que sea "Continuar". Este boton debe mostrar un cartel de confirmación si y solo si la fecha que se selecciono es el 1 de abril de cualquier año posterior al 1990.
Para que el usuario pueda entender que fecha debe poner el mensaje de validación debe mostrar la diferencia de dias entre la fecha que ingreso y la fecha valida para continuar.
Canvas Selector Fecha Nacimiento Ux Malo
El mensaje de error tiene que aparece como un toast en color Verde.

Al poner el valor correcto la pantalla de debe limpiar y se debe mostrar un div que indique que esto fue resuelto y un boton para reiniciar que se mueva al intentar tocarlo. 

Este movimiento se debe hacer hasta que finalmente le permita al usuario presionarlo.
Canvas Selector Fecha Nacimiento Ux Malo
Revisa el codigo. AL ejecutarlo esta dando el siguiente error 1
Ejecutar
SyntaxError: Uncaught SyntaxError: Failed to execute 'write' on 'Document': Unexpected token 'const'
Canvas Selector Fecha Nacimiento Ux Malo
Al reiniciar, el juego debe reiniciarse. Es decir, debe aparece la misma pantalla para cargar la fecha.
Luego cambio el titulo INGRESO FECHA DE NACIMIENTO por INGRESE FECHA
Canvas Selector Fecha Nacimiento Ux Malo
Sacar los textos "El sistema puede fallar si nacio un martes"