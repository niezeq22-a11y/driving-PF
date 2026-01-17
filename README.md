<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Driving Professional</title>

<style>
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: #f4f4f4;
}

nav {
  background: black;
  padding: 12px;
  text-align: center;
}

nav a {
  color: white;
  margin: 0 15px;
  text-decoration: none;
  font-weight: bold;
  cursor: pointer;
}

.hero {
  height: 90vh;
  background: linear-gradient(rgba(0,0,0,0.6), rgba(0,0,0,0.6)),
  url("https://images.unsplash.com/photo-1503376780353-7e6692767b70");
  background-size: cover;
  background-position: center;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
}

section {
  display: none;
  background: white;
  padding: 20px;
  margin: 20px auto;
  max-width: 500px;
  border-radius: 6px;
}

input, button {
  width: 100%;
  padding: 10px;
  margin: 6px 0;
}

button {
  background: black;
  color: white;
  border: none;
  cursor: pointer;
}

.driver {
  border: 1px solid #ccc;
  padding: 15px;
  margin: 10px 0;
  text-align: center;
}

.driver img {
  width: 120px;
  height: 120px;
  border-radius: 50%;
  object-fit: cover;
}

.hire {
  background: green;
}
</style>
</head>

<body>

<nav>
  <a onclick="showPage('home')">Home</a>
  <a onclick="showPage('drivers')">Drivers</a>
  <a onclick="showPage('register')">Register</a>
  <a onclick="showPage('login')">Login</a>
</nav>

<!-- HOME -->
<div id="home" class="hero">
  <div>
    <h1>Driving Professional</h1>
    <p>Trusted & Professional Drivers</p>
  </div>
</div>

<!-- LOGIN -->
<section id="login">
  <h2>Login</h2>
  <input type="email" placeholder="Email">
  <input type="password" placeholder="Password">
  <button onclick="alert('Login coming soon')">Login</button>
</section>

<!-- REGISTER -->
<section id="register">
  <h2>Register Driver</h2>
  <input id="name" placeholder="Name">
  <input id="age" type="number" placeholder="Age">
  <input id="license" placeholder="License Number">
  <input id="experience" type="number" placeholder="Experience (years)">
  <input id="photo" type="file">
  <button onclick="registerDriver()">Register Driver</button>
</section>

<!-- DRIVERS -->
<section id="drivers">
  <h2>Available Drivers</h2>
  <div id="driversList"></div>
</section>

<script>
let drivers = JSON.parse(localStorage.getItem("drivers")) || [];

function showPage(page) {
  document.querySelectorAll("section").forEach(s => s.style.display = "none");
  document.getElementById("home").style.display = "none";

  if (page === "home") {
    document.getElementById("home").style.display = "flex";
  } else {
    document.getElementById(page).style.display = "block";
    if (page === "drivers") loadDrivers();
  }
}

function registerDriver() {
  const name = document.getElementById("name").value;
  const age = document.getElementById("age").value;
  const license = document.getElementById("license").value;
  const experience = document.getElementById("experience").value;
  const photo = document.getElementById("photo").files[0];

  if (!name || !age || !license || !experience || !photo) {
    alert("Fill all fields");
    return;
  }

  const reader = new FileReader();
  reader.onload = function () {
    drivers.push({
      name, age, license, experience, photo: reader.result
    });
    localStorage.setItem("drivers", JSON.stringify(drivers));
    alert("Driver registered successfully!");
    showPage("drivers");
  };
  reader.readAsDataURL(photo);
}

function loadDrivers() {
  const list = document.getElementById("driversList");
  list.innerHTML = "";
  drivers.forEach(d => {
    list.innerHTML += `
      <div class="driver">
        <img src="${d.photo}">
        <p><b>${d.name}</b></p>
        <p>Age: ${d.age}</p>
        <p>License: ${d.license}</p>
        <p>Experience: ${d.experience} years</p>
        <button class="hire" onclick="alert('You hired ${d.name}')">Hire Driver</button>
      </div>
    `;
  });
}

showPage("home");
</script>

</body>
</html>
