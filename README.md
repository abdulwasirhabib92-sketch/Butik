<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MIMEES COLLECTION</title>

<style>
*{margin:0;padding:0;box-sizing:border-box;}
body{
    font-family:'Segoe UI',sans-serif;
    background:#0f0f0f;
    color:white;
}

/* LOGIN */
.container{
    max-width:400px;
    margin:80px auto;
    background:#fff;
    color:#333;
    padding:30px;
    border-radius:12px;
}

input, button{
    width:100%;
    padding:12px;
    margin:10px 0;
}

button{
    background:#ff2e63;
    color:white;
    border:none;
    cursor:pointer;
}

.switch{
    text-align:center;
    color:#ff2e63;
    cursor:pointer;
}

/* DASHBOARD */
#dashboard{display:none;}

nav{
    background:#000;
    padding:15px;
    display:flex;
    justify-content:space-between;
}

.logout{cursor:pointer;color:#ff2e63;}

/* VIDEO SHOP */
.feed{
    height:100vh;
    overflow-y:scroll;
    scroll-snap-type:y mandatory;
}

.product{
    height:100vh;
    position:relative;
    scroll-snap-align:start;
}

.product video{
    width:100%;
    height:100%;
    object-fit:cover;
}

/* OVERLAY CONTENT */
.overlay{
    position:absolute;
    bottom:20px;
    left:20px;
}

.overlay h2{
    font-size:24px;
}

.overlay p{
    margin:5px 0;
}

.order-btn{
    background:#25D366;
    padding:10px 15px;
    border:none;
    border-radius:8px;
    color:white;
    cursor:pointer;
}
</style>
</head>

<body>

<!-- LOGIN -->
<div class="container" id="authBox">
    <h2 id="formTitle">Login</h2>

    <input type="text" id="username" placeholder="Username">
    <input type="password" id="password" placeholder="Password">

    <button onclick="handleAuth()">Login</button>

    <div class="switch" onclick="toggleForm()">
        Don't have an account? Sign up
    </div>
</div>

<!-- DASHBOARD -->
<div id="dashboard">

    <nav>
        <div>MIMEES COLLECTION</div>
        <div>
            <span id="userDisplay"></span> |
            <span class="logout" onclick="logout()">Logout</span>
        </div>
    </nav>

    <div id="welcomeBox" style="padding:15px;"></div>

    <!-- TIKTOK STYLE SHOP -->
    <div class="feed">

        <!-- PRODUCT 1 -->
        <div class="product">
            <video src="videos/dress.mp4" autoplay muted loop playsinline></video>
            <div class="overlay">
                <h2>👗 Ladies Dress</h2>
                <p>Trendy & elegant</p>
                <button class="order-btn" onclick="order('Ladies Dress')">
                    Order on WhatsApp
                </button>
            </div>
        </div>

        <!-- PRODUCT 2 -->
        <div class="product">
            <video src="videos/sneakers.mp4" autoplay muted loop playsinline></video>
            <div class="overlay">
                <h2>👟 Sneakers</h2>
                <p>Comfort & style</p>
                <button class="order-btn" onclick="order('Sneakers')">
                    Order on WhatsApp
                </button>
            </div>
        </div>

        <!-- PRODUCT 3 -->
        <div class="product">
            <video src="videos/heels.mp4" autoplay muted loop playsinline></video>
            <div class="overlay">
                <h2>👠 Heels</h2>
                <p>Elegant vibes</p>
                <button class="order-btn" onclick="order('Heels')">
                    Order on WhatsApp
                </button>
            </div>
        </div>

        <!-- PRODUCT 4 -->
        <div class="product">
            <video src="videos/men.mp4" autoplay muted loop playsinline></video>
            <div class="overlay">
                <h2>👕 Men's Outfit</h2>
                <p>Clean modern look</p>
                <button class="order-btn" onclick="order('Mens Outfit')">
                    Order on WhatsApp
                </button>
            </div>
        </div>

    </div>

</div>

<script>
let isLogin = true;

// SWITCH FORM
function toggleForm(){
    isLogin = !isLogin;
    document.getElementById("formTitle").innerText = isLogin ? "Login" : "Sign Up";
    document.querySelector("button").innerText = isLogin ? "Login" : "Sign Up";
    document.querySelector(".switch").innerText = isLogin 
        ? "Don't have an account? Sign up"
        : "Already have an account? Login";
}

// AUTH SYSTEM
function handleAuth(){
    let username = document.getElementById("username").value;
    let password = document.getElementById("password").value;

    if(!username || !password){
        alert("Fill all fields");
        return;
    }

    let users = JSON.parse(localStorage.getItem("users")) || {};

    if(isLogin){
        if(users[username] && users[username].password === password){
            localStorage.setItem("currentUser", username);
            loadDashboard();
        } else {
            alert("Invalid login");
        }
    } else {
        if(users[username]){
            alert("User exists");
            return;
        }

        users[username] = {
            password: password,
            firstLogin: true
        };

        localStorage.setItem("users", JSON.stringify(users));
        alert("Signup successful! Login now.");
        toggleForm();
    }
}

// LOAD DASHBOARD
function loadDashboard(){
    let username = localStorage.getItem("currentUser");
    let users = JSON.parse(localStorage.getItem("users"));

    document.getElementById("authBox").style.display = "none";
    document.getElementById("dashboard").style.display = "block";
    document.getElementById("userDisplay").innerText = username;

    let welcomeBox = document.getElementById("welcomeBox");

    if(users[username].firstLogin){
        welcomeBox.innerHTML = `
            <h3>🎉 Welcome ${username}!</h3>
            <p>Welcome to MIMEES COLLECTION — your fashion plug 🔥</p>
        `;
        users[username].firstLogin = false;
        localStorage.setItem("users", JSON.stringify(users));
    } else {
        welcomeBox.innerHTML = `<h3>Welcome back ${username} 👋</h3>`;
    }
}

// WHATSAPP ORDER
function order(product){
    let phone = "233591808780";
    let message = `Hello, I want to order: ${product} from MIMEES COLLECTION`;
    let url = `https://wa.me/${phone}?text=${encodeURIComponent(message)}`;
    window.open(url, "_blank");
}

// LOGOUT
function logout(){
    localStorage.removeItem("currentUser");
    location.reload();
}

// AUTO LOGIN
window.onload = function(){
    let user = localStorage.getItem("currentUser");
    if(user){
        loadDashboard();
    }
}
</script>

</body>
</html>
