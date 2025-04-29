<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Horario con Formspree</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      margin: 40px;
      background-color: #f1f8fc;
    }
    h1 {
      text-align: center;
      color: #2c3e50;
      text-transform: uppercase;
      letter-spacing: 2px;
    }
    table {
      width: 90%;
      margin: 0 auto;
      border-collapse: collapse;
      background-color: #fff;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
      border-radius: 8px;
    }
    th, td {
      border: 1px solid #ddd;
      padding: 14px;
      text-align: center;
      vertical-align: top;
    }
    th {
      background-color: #3498db;
      color: white;
      font-size: 1.1em;
    }
    tr:hover {
      background-color: #ecf0f1;
    }
    caption {
      margin-bottom: 10px;
      font-size: 1.2em;
      color: #7f8c8d;
    }
    .tarea-pendiente {
      display: block;
      margin-top: 8px;
      font-size: 0.9em;
      color: #2c3e50;
      cursor: pointer;
      transition: color 0.3s ease;
    }
    .tarea-pendiente:hover {
      color: #e74c3c;
    }
    .nueva-tarea {
      width: 90%;
      margin-top: 8px;
      font-size: 0.9em;
      padding: 6px;
      border-radius: 5px;
      border: 1px solid #3498db;
      background-color: #f0f8ff;
    }
    .nueva-tarea:focus {
      outline: none;
      border-color: #2980b9;
    }
    .registro {
      text-align: center;
      margin-top: 20px;
    }
    .registro input {
      padding: 10px;
      font-size: 1em;
      border-radius: 5px;
      border: 1px solid #3498db;
      width: 250px;
      margin-right: 10px;
    }
    .registro button {
      padding: 10px 20px;
      font-size: 1em;
      color: white;
      background-color: #3498db;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }
    .registro button:hover {
      background-color: #2980b9;
    }
  </style>
</head>
<body>

  <h1>Mi Horario Semanal</h1>

  <table id="horario">
    <caption>Semana 1</caption>
    <thead>
      <tr>
        <th>Hora</th>
        <th>Lunes</th>
        <th>Martes</th>
        <th>Miércoles</th>
        <th>Jueves</th>
        <th>Viernes</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>7:30 - 8:30 am</td>
        <td>Inglés</td><td>Inglés</td><td>Inglés</td><td>Inglés</td><td>Inglés</td>
      </tr>
      <tr>
        <td>8:30 - 9:30 am</td>
        <td>Matemáticas</td><td>Matemáticas</td><td>Matemáticas</td><td>Matemáticas</td><td>Matemáticas</td>
      </tr>
      <tr>
        <td>9:30 - 10:30 am</td>
        <td>Lectura C</td><td>Lectura C</td><td>Lectura C</td><td>Lectura C</td><td>Lectura C</td>
      </tr>
      <tr>
        <td>10:30 - 11:30 am</td>
        <td>Biología</td><td>Biología</td><td>Biología</td><td>Biología</td><td>Biología</td>
      </tr>
      <tr>
        <td>11:30 - 12:30 pm</td>
        <td>Historia</td><td>Historia</td><td>Historia</td><td>Historia</td><td>Historia</td>
      </tr>
      <tr>
        <td>6:30 - 8:30 pm</td>
        <td>Programación</td><td>Programación</td><td>Programación</td><td>Programación</td><td>Programación</td>
      </tr>
    </tbody>
  </table>

  <!-- Formulario de Registro -->
  <div class="registro">
    <h3>Registra tu correo para recibir las tareas completadas</h3>
    <form id="formRegistro">
      <input type="email" id="correoUsuario" placeholder="Ingresa tu correo" required />
      <button type="submit">Registrar</button>
    </form>
  </div>

  <!-- Formulario oculto para enviar por Formspree -->
  <form id="formularioTareas" action="https://formspree.io/f/mzzrdrjg" method="POST" style="display:none;">
    <input type="hidden" name="message" id="mensajeFormspree" />
    <input type="email" name="email" id="emailUsuario" />
    <button type="submit">Enviar</button>
  </form>

  <script>
    let correoUsuario = '';
    
    // Manejo del registro de usuario
    document.getElementById('formRegistro').addEventListener('submit', function(event) {
      event.preventDefault();
      correoUsuario = document.getElementById('correoUsuario').value;
      document.getElementById('emailUsuario').value = correoUsuario;
      alert("¡Correo registrado! Ahora podrás recibir las tareas completadas.");
    });

    const tabla = document.getElementById('horario');

    tabla.addEventListener('click', function(event) {
      const celda = event.target;
      if (celda.tagName === 'TD' && celda.cellIndex !== 0) {
        if (!celda.querySelector('.nueva-tarea')) {
          const textoActual = celda.innerText.trim();
          celda.innerHTML = ` 
            <div contenteditable="true" class="actividad">${textoActual}</div>
            <textarea class="nueva-tarea" placeholder="Agregar tarea pendiente..."></textarea>
          `;

          const textarea = celda.querySelector('.nueva-tarea');
          textarea.addEventListener('keydown', function(e) {
            if (e.key === 'Enter') {
              e.preventDefault();
              if (textarea.value.trim() !== '') {
                const tarea = document.createElement('span');
                tarea.className = 'tarea-pendiente';
                tarea.textContent = `🔵 ${textarea.value.trim()}`;
                tarea.addEventListener('click', function() {
                  tarea.textContent = tarea.textContent.startsWith('✅') ? 
                    `🔵 ${tarea.textContent.slice(2)}` : 
                    `✅ ${tarea.textContent.slice(2)}`;
                  verificarTareas();
                });
                celda.appendChild(tarea);
                textarea.value = '';
              }
            }
          });
        }
      }
    });

    function verificarTareas() {
      const tareas = document.querySelectorAll('.tarea-pendiente');
      if (tareas.length > 0 && Array.from(tareas).every(t => t.textContent.startsWith('✅'))) {
        enviarCorreoFormspree();
      }
    }

    function enviarCorreoFormspree() {
      const tareas = Array.from(document.querySelectorAll('.tarea-pendiente'))
        .map(t => t.textContent).join('\n');
      document.getElementById('mensajeFormspree').value = `🎉 ¡Has completado todas tus tareas!\n\n${tareas}`;
      document.getElementById('formularioTareas').submit();
      alert("✅ Correo enviado con Formspree.");
    }
  </script>

</body>
</html>
