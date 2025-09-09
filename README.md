<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Panel de Marca - Dr. Acosta Danaher</title>
<style>
  body { font-family: Arial, sans-serif; background:#f4f4f4; margin:0; padding:0; }
  .container { max-width:1000px; margin:40px auto; background:#fff; padding:30px; border-radius:10px; box-shadow:0 0 10px rgba(0,0,0,0.1);}
  h2, h3 { color:#1e88e5; }
  .table-container { overflow-x:auto; margin-bottom:20px; }
  table { width:100%; border-collapse: collapse; min-width:900px; }
  th, td { padding:15px; border-bottom:1px solid #ddd; text-align:center; vertical-align:middle; }
  th { background:#f8f9fa; white-space:nowrap; }
  td div { display:flex; flex-direction:column; align-items:center; }
  .verified { color:green; font-weight:bold; font-size:18px; }
  .not-verified { color:red; font-weight:bold; font-size:18px; }
  .login-section { text-align:center; padding:50px; }
  input { padding:10px; width:200px; margin:10px 0; border:1px solid #ccc; border-radius:5px; }
  button { padding:12px 25px; background:#1e88e5; color:#fff; border:none; border-radius:6px; cursor:pointer; font-size:16px; }
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

  <h3>Novedades</h3>
  <div class="novedades" id="novedades-list">
    <!-- Novedades dinámicas -->
  </div>

  <button onclick="logout()">Cerrar Sesión</button>
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

  document.getElementById("consulta-status").innerHTML = `<div>${user.consultaVerificado ? "✔" : "✖"}<br>${user.consultaVerificado ? "Verificado" : "No verificado"}</div>`;
  document.getElementById("registro-status").innerHTML = `<div>${user.registroVerificado ? "✔" : "✖"}<br>${user.registroVerificado ? "Verificado" : "No verificado"}</div>`;
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
}

function logout() {
  document.getElementById("dashboard-container").style.display = "none";
  document.getElementById("login-container").style.display = "block";
  document.getElementById("dni").value = "";
  document.getElementById("password").value = "";
}
</script>

</body>
</html>
