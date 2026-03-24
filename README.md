<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Frigibet - Cassino Demo</title>

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:Arial}
body{background:#12001e;color:#fff;padding-bottom:70px;padding-top:70px}

/* HEADER */
.header{
background:linear-gradient(90deg,#5b0a8d,#2d004a);
padding:12px;
display:flex;
justify-content:space-between;
align-items:center;
position:fixed;
top:0;
width:100%;
z-index:1000;
}
.logo img{height:30px}
.header div{display:flex;gap:10px;align-items:center}

.saldo{
background:#24003a;
padding:6px 12px;
border-radius:20px;
font-weight:bold;
}

.depositar{
background:#19ff7a;
color:#000;
padding:6px 14px;
border-radius:20px;
font-weight:bold;
cursor:pointer;
}

/* SEÇÕES */
.section{padding:15px;display:none}
.grid{display:grid;grid-template-columns:repeat(3,1fr);gap:12px}

/* CARD */
.card{
position:relative;
background:#2b0044;
border-radius:14px;
overflow:hidden;
cursor:pointer;
height:170px;
transition:0.2s;
}
.card:hover{transform:scale(1.05)}
.card img{width:100%;height:100%;object-fit:cover}

.nome-jogo{
position:absolute;
bottom:0;
width:100%;
padding:8px;
background:rgba(0,0,0,0.6);
text-align:center;
}

/* INPUT */
input, textarea, select{
width:100%;
padding:12px;
margin-top:10px;
border-radius:10px;
border:none;
outline:none;
}

/* BOTÕES */
button{
width:100%;
padding:12px;
margin-top:10px;
border:none;
border-radius:10px;
background:#19ff7a;
font-weight:bold;
cursor:pointer;
}

/* MENU */
.menu{
position:fixed;
bottom:0;
width:100%;
background:#2d004a;
display:flex;
justify-content:space-around;
padding:10px;
}
.menu a{color:#aaa;font-size:12px}
.menu a.active{color:#19ff7a}

/* MINES */
#campoMines{
display:grid;
grid-template-columns:repeat(5,1fr);
gap:5px;
margin-top:10px;
}
#campoMines button{
padding:15px;
background:#00dfff;
border-radius:8px;
font-size:20px;
}
</style>
</head>

<body>

<div class="header">
<div class="logo">
<img src="https://frigibet.fun/assets/images/logo_1766856561.png">
</div>
<div>
<span class="saldo" id="saldo">R$ 0,00</span>
<span class="depositar" onclick="mostrarSecao('registro')">Registrar</span>
<span class="depositar" onclick="mostrarSecao('deposito')">Depositar</span>
</div>
</div>

<!-- MENU -->
<div class="section" id="menu">
<h3>🌍 Idioma</h3>
<button onclick="setIdioma('pt')">🇧🇷 Português</button>
<button onclick="setIdioma('tr')">🇹🇷 Turco</button>
</div>

<!-- SAQUE -->
<div class="section" id="saque">
<h3>💸 Saque</h3>

<select id="tipoSaque">
<option value="telefone">📱 Telefone</option>
<option value="cpf">🪪 CPF</option>
<option value="gmail">📧 Gmail</option>
</select>

<input type="text" id="chavePix" placeholder="Digite sua chave PIX">
<input type="number" id="valorSaque" placeholder="Valor para sacar">

<button onclick="sacar()">Solicitar Saque</button>
</div>

<!-- REGISTRO -->
<div class="section" id="registro">
<h3>📋 Registro</h3>
<input type="text" id="nome" placeholder="Nome completo">
<input type="tel" id="numero" placeholder="Número">
<input type="password" id="senha" placeholder="Senha">
<button onclick="registrar()">Registrar</button>
<button onclick="mostrarSecao('cassino')">🎰 Ir para o Cassino</button>
</div>

<!-- DEPOSITO -->
<div class="section" id="deposito">
<h3>💰 Depositar via PIX</h3>
<input type="number" id="valorDeposito" placeholder="Digite o valor">

<img src="https://api.qrserver.com/v1/create-qr-code/?size=250x250&data=PIX-DEMO"
style="width:250px;margin:auto;display:block;border-radius:10px;">

<textarea id="pixCode" readonly>00020101021126790014BR.GOV.BCB.PIX2557pix-qr.mercadopago.com/instore/ol/v2/rZJ2HYUC7YJWRHVbOUEd5204000053039865802BR5916Maristela Santos6009SAO PAULO62080504mpis6304A679</textarea>

<button onclick="copiarPix()">📋 Copiar PIX</button>
<button onclick="confirmarDeposito()">✅ Já paguei</button>
</div>

<!-- CASSINO -->
<div class="section" id="cassino">

<h3>🔥 Destaque</h3>
<div class="grid" id="gridDestaque"></div>

<h3>🎰 Outros Jogos</h3>
<div class="grid" id="gridOutros"></div>

<h3>🎲 Extras</h3>
<div class="grid" id="gridExtras"></div>

<h3>💣 Mines</h3>
<div class="grid">
<div class="card" onclick="abrirMines()">
<img src="https://niceslot.bet/game_logo/SELF/SE0001.jpg?t=1">
<div class="nome-jogo">Mines</div>
</div>
</div>

</div>

<!-- MENU FOOTER -->
<div class="menu">
<a onclick="mostrarSecao('cassino')" class="active">🎰 Cassino</a>
<a onclick="mostrarSecao('menu')">📋 Menu</a>
<a onclick="mostrarSecao('saque')">💸 Saque</a>
<a onclick="mostrarSecao('registro')">👤 Conta</a>
</div>

<script>
let saldo=0;
let idiomaAtual="pt";

function mostrarSecao(id){
document.querySelectorAll('.section').forEach(s=>s.style.display='none');
document.getElementById(id).style.display='block';
}

function registrar(){alert("Conta criada!")}

// Copiar PIX
function copiarPix(){
const pix=document.getElementById("pixCode").value;
navigator.clipboard.writeText(pix).then(()=>{alert("PIX copiado! ✅");})
.catch(()=>{alert("Erro ao copiar PIX")});
}

function confirmarDeposito(){
let valor=parseFloat(document.getElementById("valorDeposito").value);
if(isNaN(valor)||valor<=0)return alert("Valor inválido");
if(idiomaAtual === "tr") valor *= 22;
saldo+=valor;
atualizarSaldo();
mostrarSecao('cassino');
}

function sacar(){
const valor=parseFloat(document.getElementById("valorSaque").value);
const chave=document.getElementById("chavePix").value;
const tipo=document.getElementById("tipoSaque").value;

if(!chave)return alert("Digite sua chave PIX");
if(isNaN(valor)||valor<=0)return alert("Valor inválido");
if(valor>saldo)return alert("Saldo insuficiente");

saldo-=valor;
atualizarSaldo();
alert("Saque solicitado via " + tipo.toUpperCase() + " ✅");
}

function atualizarSaldo(){
let simbolo = idiomaAtual === "tr" ? "₺" : "R$";
document.getElementById("saldo").textContent=simbolo+" "+saldo.toFixed(2);
}

function setIdioma(id){
idiomaAtual=id;
alert(id==="pt"?"Português ativado 🇧🇷":"Türkçe ativado 🇹🇷");
atualizarSaldo();
}

function abrirMines(){alert("Abrir Mines!")}

// Lista de jogos automática
const jogos = [
{nome:"Fortune Tiger", capa:"https://d1ss1ykt5u2h7w.cloudfront.net/images/gameicon/pg/v1/1.jpg", tipo:"destaque"},
{nome:"Fortune Mouse", capa:"https://d1ss1ykt5u2h7w.cloudfront.net/images/gameicon/pg/v1/4.jpg", tipo:"destaque"},
{nome:"Fortune Ox", capa:"https://d1ss1ykt5u2h7w.cloudfront.net/images/gameicon/pg/v1/3.jpg", tipo:"outros"},
{nome:"Fortune Rabbit", capa:"https://d1ss1ykt5u2h7w.cloudfront.net/images/gameicon/pg/v1/2.jpg", tipo:"outros"},
{nome:"Fortune Dragon", capa:"https://d1ss1ykt5u2h7w.cloudfront.net/images/gameicon/pg/v1/icon_fortune.jpg", tipo:"outros"},
{nome:"Fortune Panda", capa:"https://d1ss1ykt5u2h7w.cloudfront.net/images/gameicon/pg/v1/5.jpg", tipo:"extras"},
{nome:"Fortune Lion", capa:"https://d1ss1ykt5u2h7w.cloudfront.net/images/gameicon/pg/v1/6.jpg", tipo:"extras"},
{nome:"Fortune Elephant", capa:"https://d1ss1ykt5u2h7w.cloudfront.net/images/gameicon/pg/v1/7.jpg", tipo:"extras"},
// Adicione mais jogos aqui
{nome:"Fortune Monkey", capa:"https://d1ss1ykt5u2h7w.cloudfront.net/images/gameicon/pg/v1/8.jpg", tipo:"extras"},
{nome:"Fortune Bird", capa:"https://d1ss1ykt5u2h7w.cloudfront.net/images/gameicon/pg/v1/9.jpg", tipo:"extras"},
];

function renderizarJogos(){
jogos.forEach(j=>{
    let div = document.createElement("div");
    div.className="card";
    div.innerHTML=`<img src="${j.capa}"><div class="nome-jogo">${j.nome}</div>`;
    if(j.tipo==="destaque") document.getElementById("gridDestaque").appendChild(div);
    if(j.tipo==="outros") document.getElementById("gridOutros").appendChild(div);
    if(j.tipo==="extras") document.getElementById("gridExtras").appendChild(div);
});

}

renderizarJogos();
mostrarSecao('cassino');
</script>

</body>
</html>
