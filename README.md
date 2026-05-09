<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>IRA FX™ BOOK STORE 📚</title>

<style>
*{
  margin:0;
  padding:0;
  box-sizing:border-box;
}

body{
  font-family:Segoe UI, sans-serif;
  background:linear-gradient(135deg,#0d1b2a,#1b263b);
  color:white;
  min-height:100vh;
  text-align:center;
  padding:20px;
}

h1{
  margin-top:20px;
  margin-bottom:10px;
  font-size:32px;
}

.telegram-link{
  display:inline-block;
  margin-bottom:20px;
  color:#00b4d8;
  text-decoration:none;
  font-weight:bold;
}

input{
  width:260px;
  padding:12px;
  margin:8px;
  border:none;
  border-radius:10px;
  outline:none;
}

button{
  background:#0096c7;
  color:white;
  border:none;
  padding:12px 18px;
  border-radius:10px;
  cursor:pointer;
  transition:0.3s;
  margin:5px;
  font-weight:bold;
}

button:hover{
  background:#00b4d8;
  transform:scale(1.05);
}

#books{
  display:flex;
  flex-wrap:wrap;
  justify-content:center;
  gap:20px;
  margin-top:20px;
}

.card{
  background:#1b263b;
  width:230px;
  padding:15px;
  border-radius:18px;
  box-shadow:0 0 15px rgba(0,0,0,0.4);
}

.card img{
  width:100%;
  height:250px;
  object-fit:cover;
  border-radius:12px;
}

.card h3{
  margin-top:10px;
}

.card p{
  margin:10px 0;
  font-size:18px;
  color:#90e0ef;
}

#loginBox,
#adminPanel{
  margin-top:20px;
}

.admin-box{
  background:#1b263b;
  padding:20px;
  border-radius:15px;
  width:320px;
  margin:auto;
  box-shadow:0 0 10px rgba(0,0,0,0.4);
}

hr{
  margin:30px 0;
  border:1px solid rgba(255,255,255,0.1);
}
</style>
</head>

<body>

<a class="telegram-link" href="https://t.me/ira_fx99" target="_blank">
TELEGRAM CHANNEL™
</a>

<h1>IRA FX™ BOOK STORE 📚</h1>

<!-- SEARCH -->
<input type="text" id="search" placeholder="Search books..." onkeyup="filterBooks()">

<br>

<!-- ADMIN BUTTON -->
<button onclick="showLogin()">🔐 Admin Login</button>
<button onclick="logout()">🚪 Logout</button>

<!-- LOGIN BOX -->
<div id="loginBox" style="display:none;">
  <div class="admin-box">
    <h2>Admin Login</h2>
    <br>
    <input type="password" id="adminPass" placeholder="Enter password">
    <br>
    <button onclick="checkPassword()">Login</button>
  </div>
</div>

<!-- ADMIN PANEL -->
<div id="adminPanel" style="display:none;">
  <div class="admin-box">
    <h2>Add New Book</h2>

    <input type="text" id="title" placeholder="Book Name">

    <input type="number" id="price" placeholder="Price ETB">

    <input type="file" id="image" accept="image/*">

    <br>

    <button onclick="addBook()">➕ Add Book</button>
  </div>
</div>

<hr>

<!-- BOOKS -->
<div id="books"></div>

<script>

// ================================
// SETTINGS
// ================================

const ADMIN_PASSWORD = "Imanufx@99";
const TELEGRAM_USERNAME = "irafx100";

// ================================
// STORAGE
// ================================

let books = JSON.parse(localStorage.getItem("books")) || [];
let isAdmin = localStorage.getItem("isAdmin") === "true";

// ================================
// LOAD WEBSITE
// ================================

window.onload = function(){
  if(isAdmin){
    document.getElementById("adminPanel").style.display = "block";
  }

  displayBooks(books);
}

// ================================
// SHOW LOGIN
// ================================

function showLogin(){
  document.getElementById("loginBox").style.display = "block";
}

// ================================
// LOGIN SYSTEM
// ================================

function checkPassword(){
  const pass = document.getElementById("adminPass").value;

  if(pass === ADMIN_PASSWORD){
    isAdmin = true;

    localStorage.setItem("isAdmin", "true");

    document.getElementById("adminPanel").style.display = "block";
    document.getElementById("loginBox").style.display = "none";

    displayBooks(books);

    alert("✅ Login Successful");

  }else{
    alert("❌ Wrong Password");
  }
}

// ================================
// LOGOUT
// ================================

function logout(){
  isAdmin = false;

  localStorage.removeItem("isAdmin");

  document.getElementById("adminPanel").style.display = "none";

  displayBooks(books);

  alert("✅ Logged Out");
}

// ================================
// ADD BOOK
// ================================

function addBook(){

  const title = document.getElementById("title").value.trim();
  const price = document.getElementById("price").value.trim();
  const imageInput = document.getElementById("image");

  if(title === "" || price === ""){
    alert("Please fill all fields");
    return;
  }

  if(imageInput.files.length === 0){
    alert("Please select image");
    return;
  }

  const file = imageInput.files[0];

  const reader = new FileReader();

  reader.onload = function(e){

    const newBook = {
      title: title,
      price: price,
      image: e.target.result
    };

    books.push(newBook);

    localStorage.setItem("books", JSON.stringify(books));

    document.getElementById("title").value = "";
    document.getElementById("price").value = "";
    document.getElementById("image").value = "";

    displayBooks(books);

    alert("✅ Book Added Successfully");
  }

  reader.readAsDataURL(file);
}

// ================================
// DISPLAY BOOKS
// ================================

function displayBooks(bookList){

  let html = "";

  if(bookList.length === 0){
    html = "<h2>No books available</h2>";
  }

  bookList.forEach((book,index)=>{

    const message = `Hello, I want to buy ${book.title} for ${book.price} ETB`;

    const telegramLink = `https://t.me/${TELEGRAM_USERNAME}?text=${encodeURIComponent(message)}`;

    html += `

      <div class="card">

        <img src="${book.image}">

        <h3>${book.title}</h3>

        <p>${book.price} ETB</p>

        <a href="${telegramLink}" target="_blank">
          <button>🛒 Buy Now</button>
        </a>

        <br>

        ${isAdmin ? `<button onclick="deleteBook(${index})">❌ Delete</button>` : ""}

      </div>

    `;

  });

  document.getElementById("books").innerHTML = html;
}

// ================================
// DELETE BOOK
// ================================

function deleteBook(index){

  const confirmDelete = confirm("Delete this book?");

  if(confirmDelete){

    books.splice(index,1);

    localStorage.setItem("books", JSON.stringify(books));

    displayBooks(books);
  }
}

// ================================
// SEARCH BOOKS
// ================================

function filterBooks(){

  const value = document.getElementById("search").value.toLowerCase();

  const filteredBooks = books.filter(book =>
    book.title.toLowerCase().includes(value)
  );

  displayBooks(filteredBooks);
}

</script>

</body>
</html>

