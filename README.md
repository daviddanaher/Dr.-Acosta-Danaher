<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Panel de Marca - Dr. Acosta Danaher</title>
<style>
  body { font-family: Arial, sans-serif; background:#f4f4f4; margin:0; padding:0; }
  .container { max-width:1200px; margin:40px auto; background:#fff; padding:30px; border-radius:10px; box-shadow:0 0 10px rgba(0,0,0,0.1);}
  h2, h3 { color:#1e88e5; }
  .table-container { overflow-x:auto; margin-bottom:20px; }
  table { width:100%; border-collapse: collapse; table-layout: fixed; }
  th, td { padding:15px; border-bottom:1px solid #ddd; text-align:center; vertical-align:middle; width:10%; }
  th { background:#f8f9fa; }
  td div { display:flex; flex-direction:column; align-items:center; word-wrap: break-word; }
  .completed { color:green; font-weight:bold; font-size:16px; }
  .not-completed { color:red; font-weight:bold; font-size:16px; }
  .login-section { text-align:center; padding:50px; }
  input { padding:10px; width:200px; margin:10px 0; border:1px solid #ccc; border-radius:5px; }
  button { padding:12px 25px; background:#1e88e5; color:#fff; border:none; border-radius:6px; cursor:pointer; font-size:16px; margin:5px; }
  .btn-whatsapp { display:inline-flex; align-items:center; background:#25d366; color:#fff; padding:12px 20px; border-radius:6px; text-decoration:none; margin:5px; }
  .btn-whatsapp img { width:20px; height:20px; margin-right:10px; }
  .novedades { margin-top:20px; }
  .novedad-item { padding:10px; background:#f8f9fa; border-left:4px solid #1e88e5; margin-bottom:10px; border-radius:5px; text-align:left; }
</style>
</head>
<body>

<div class="container" id="login-container">
  <div class="login-section">
    <h2>Iniciar Sesión</h2>
    <p>Usuario: DNI | Contraseña: DNI</p>
    <input type="text" id="dni" placeholder="Ingrese su DNI"><br>
    <input type="password" id="password" placeholder="Ingrese su DNI como contraseña"><br>
    <button onclick="login()">Ingresar</button>
    <p id="login-error" style="color:red;"></p>
  </div>
</div>

<div class="container" id="dashboard-container" style="display:none;">
  <h2>Bienvenido, <span id="user-name"></span></h2>

  <h3>Datos de la Marca</h3>
  <div class="table-container">
    <table>
      <thead>
        <tr>
          <th>Marca</th>
          <th>Categoría</th>
          <th>Medio de Pago</th>
          <th>Consulta Disponibilidad</th>
          <th>Registro de Marca</th>
          <th>Expediente</th>
          <th>Fecha de Inicio</th>
          <th>Propietario</th>
          <th>Estado Actual</th>
          <th>Última Actualización</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td id="marca-name"></td>
          <td id="marca-categoria"></td>
          <td>Mercado Pago</td>
          <td id="consulta-status"><div></div></td>
          <td id="registro-status"><div></div></td>
          <td id="expediente"><div></div></td>
          <td id="fecha-inicio"><div></div></td>
          <td id="propietario"><div></div></td>
          <td id="estado-actual"><div></div></td>
          <td id="ultima-actualizacion"><div></div></td>
        </tr>
      </tbody>
    </table>
  </div>

  <h3>Acciones</h3>
  <div style="text-align:center;">
    <button onclick="cambiarContrasena()">Cambiar Contraseña</button>
    <a id="btn-ayuda" class="btn-whatsapp" target="_blank">
      <img src="https://upload.wikimedia.org/wikipedia/commons/6/6b/WhatsApp.svg" alt="WhatsApp"> Ayuda
    </a>
    <button onclick="logout()">Cerrar Sesión</button>
  </div>

  <h3>Novedades</h3>
  <div class="novedades" id="novedades-list"></div>
</div>

<script>
const users = {
  "37170491": {
    nombre: "Mauricio David Acosta Danaher",
    email: "mauricio@dracostadanaher.com",
    provincia: "Ciudad Autónoma de Buenos Aires",
    marca: "DE ESTRENO",
    categoria: "Medio de comunicación",
    pagoConsulta: "09/09/2025",
    pagoRegistro: "09/09/2025",
    consultaVerificado: true,
    registroVerificado: true,
    expediente: "52545",
    fechaInicio: "09/09/2025",
    propietario: "100%",
    estadoActual: "En trámite",
    ultActAuto: true,
    novedades: [
      {fecha:"08/09/2025", hora:"10:56", descripcion:"Abonó consulta de disponibilidad de marca"},
      {fecha:"09/09/2025", hora:"10:57", descripcion:"Abonó Registro de marca"},
      {fecha:"09/09/2025", hora:"11:00", descripcion:"Inicio de Registro de marca"}
    ]
  },
  "93976879": {
    nombre: "Maria Lucia Carpio Peralta",
    email: "maria@dracostadanaher.com",
    provincia: "Buenos Aires",
    marca: "PASTELITTO DOLCE",
    categoria: "Gastronomía, ventas por internet",
    pagoConsulta: "08/09/2025",
    pagoRegistro: null,
    consultaVerificado: true,
    registroVerificado: false,
    expediente: "5452454",
    fechaInicio: "09/09/2025",
    propietario: "100%",
    estadoActual: "Pendiente pago registro",
    ultActAuto: false,
    novedades: [
      {fecha:"08/09/2025", hora:"10:00", descripcion:"Abonó consulta de disponibilidad de marca"},
      {fecha:"09/09/2025", hora:"10:00", descripcion:"Requiere pago de Registro de marca"}
    ]
  }
};

function login() {
  const dni = document.getElementById("dni").value.trim();
  const password = document.getElementById("password").value.trim();
  const error = document.getElementById("login-error");

  if(users[dni] && password === dni) {
    error.textContent = "";
    showDashboard(users[dni]);
  } else {
    error.textContent = "DNI o contraseña incorrectos.";
  }
}

function showDashboard(user) {
  document.getElementById("login-container").style.display = "none";
  document.getElementById("dashboard-container").style.display = "block";

  document.getElementById("user-name").textContent = user.nombre;
  document.getElementById("marca-name").textContent = user.marca;
  document.getElementById("marca-categoria").textContent = user.categoria;

  document.getElementById("consulta-status").innerHTML = `<div>${user.consultaVerificado ? "✔" : "✖"}<br><span class="${user.consultaVerificado ? 'completed' : 'not-completed'}">${user.consultaVerificado ? 'Completado' : 'No completado'}</span></div>`;
  document.getElementById("registro-status").innerHTML = `<div>${user.registroVerificado ? "✔" : "✖"}<br><span class="${user.registroVerificado ? 'completed' : 'not-completed'}">${user.registroVerificado ? 'Completado' : 'No completado'}</span></div>`;

  document.getElementById("expediente").innerHTML = `<div>${user.expediente}</div>`;
  document.getElementById("fecha-inicio").innerHTML = `<div>${user.fechaInicio}</div>`;
  document.getElementById("propietario").innerHTML = `<div>${user.propietario}</div>`;
  document.getElementById("estado-actual").innerHTML = `<div>${user.estadoActual}</div>`;

  const ultimaAct = user.ultActAuto ? new Date().toLocaleDateString("es-AR") : "-";
  document.getElementById("ultima-actualizacion").innerHTML = `<div>${ultimaAct}</div>`;

  // Novedades
  const novedadesList = document.getElementById("novedades-list");
  novedadesList.innerHTML = "";
  user.novedades.forEach(n => {
    const div = document.createElement("div");
    div.className = "novedad-item";
    div.innerHTML = `<strong>${n.fecha}, ${n.hora}</strong> - ${n.descripcion}`;
    novedadesList.appendChild(div);
  });

  // Configurar botón ayuda WhatsApp con expediente
  const ayudaBtn = document.getElementById("btn-ayuda");
  const mensaje = encodeURIComponent(`Hola, necesito ayuda sobre el proceso de registración de mi marca, mi expediente es ${user.expediente}`);
  ayudaBtn.href = `https://wa.me/541121679936?text=${mensaje}`;
}

function logout() {
  document.getElementById("dashboard-container").style.display = "none";
  document.getElementById("login-container").style.display = "block";
  document.getElementById("dni").value = "";
  document.getElementById("password").value = "";
}

function cambiarContrasena() {
  alert("Funcionalidad de cambio de contraseña en desarrollo.");
}
</script>

</body>
</html>
