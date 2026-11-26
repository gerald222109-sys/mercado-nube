# mercado-nube
mi tienda online
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="description" content="Mercado Nube: tienda demostrativa con catálogo y carrito de compras.">
<title>Mercado Nube | Tienda online</title>
<style>
:root{--yellow:#ffe600;--ink:#242424;--blue:#3483fa;--bg:#f5f5f5;--muted:#777}
*{box-sizing:border-box}body{margin:0;background:var(--bg);font:15px Arial,sans-serif;color:var(--ink)}
header{background:var(--yellow);padding:12px 4%;position:sticky;top:0;z-index:5;box-shadow:0 2px 5px #0001}
.top{max-width:1100px;margin:auto;display:flex;align-items:center;gap:18px;flex-wrap:wrap}
.logo{font-size:23px;font-weight:800;white-space:nowrap}.logo span{color:#3483fa}
.search{flex:1;display:flex;min-width:210px}.search input{width:100%;padding:12px;border:0;border-radius:5px 0 0 5px;font-size:15px}.search button{border:0;background:white;padding:0 16px;border-left:1px solid #ddd;border-radius:0 5px 5px 0}
nav{max-width:1100px;margin:10px auto 0;display:flex;gap:18px;font-size:13px}nav a{color:#333;text-decoration:none}
main{max-width:1100px;margin:22px auto;padding:0 16px}.hero{background:linear-gradient(110deg,#dceaff,#fff);border-radius:10px;padding:30px;display:flex;align-items:center;justify-content:space-between;gap:20px}.hero h1{font-size:clamp(25px,4vw,38px);margin:0 0 10px}.hero p{color:#555;max-width:520px;line-height:1.5}.hero .art{font-size:75px}
.toolbar{display:flex;justify-content:space-between;align-items:center;gap:12px;margin:24px 0 14px;flex-wrap:wrap}.toolbar h2{margin:0;font-size:22px}.filters{display:flex;gap:8px;flex-wrap:wrap}.filters button{border:1px solid #ddd;background:white;border-radius:20px;padding:8px 13px;cursor:pointer}.filters button.active{background:#3483fa;color:white;border-color:#3483fa}
.grid{display:grid;grid-template-columns:repeat(4,1fr);gap:14px}.product{background:white;border-radius:9px;overflow:hidden;box-shadow:0 1px 3px #0001;display:flex;flex-direction:column}.photo{height:150px;background:#f1f4f8;display:grid;place-items:center;font-size:58px}.info{padding:13px;display:flex;flex-direction:column;flex:1}.name{min-height:38px;line-height:1.3}.price{font-size:23px;margin:10px 0 4px}.ship{font-size:12px;color:#16834a;margin-bottom:12px}.info button,.primary{background:#3483fa;color:white;border:0;border-radius:6px;padding:11px;cursor:pointer;font-weight:600}.info button{margin-top:auto}.info button:hover,.primary:hover{background:#246bd0}
.panel{background:white;border-radius:9px;padding:18px;margin-top:24px}.panel h2{margin-top:0}.cartrow{display:flex;justify-content:space-between;gap:12px;padding:10px 0;border-bottom:1px solid #eee}.muted{color:var(--muted);font-size:13px}.cart-actions{display:flex;gap:8px;flex-wrap:wrap;margin-top:14px}.secondary{background:#eee;color:#333;border:0;border-radius:6px;padding:10px 13px;cursor:pointer}
footer{text-align:center;color:#777;padding:28px 12px;font-size:12px}
dialog{border:0;border-radius:10px;padding:22px;max-width:420px;width:calc(100% - 28px);box-shadow:0 10px 50px #0004}dialog::backdrop{background:#0007}dialog input,dialog textarea{width:100%;padding:11px;margin:7px 0 12px;border:1px solid #ccc;border-radius:5px}dialog menu{display:flex;justify-content:flex-end;gap:8px;padding:0;margin-top:14px}
@media(max-width:800px){.grid{grid-template-columns:repeat(2,1fr)}.hero .art{font-size:55px}}
@media(max-width:430px){.grid{grid-template-columns:1fr 1fr;gap:9px}.photo{height:115px}.info{padding:10px}.price{font-size:19px}.logo{font-size:20px}.hero{padding:20px}.hero .art{display:none}}
</style>
</head>
<body>
<header>
 <div class="top">
  <div class="logo">mercado<span>nube</span></div>
  <div class="search"><input id="search" placeholder="Buscar productos…" aria-label="Buscar productos"><button onclick="render()">⌕</button></div>
  <button class="secondary" onclick="showCart()">🛒 Carrito (<b id="count">0</b>)</button>
 </div>
 <nav><a href="#catalog">Categorías</a><a href="#catalog">Ofertas</a><a href="#cart">Carrito</a><a href="#about">Ayuda</a></nav>
</header>
<main>
 <section class="hero">
  <div><h1>Todo lo que buscas, en un solo lugar</h1><p>Explora productos, encuentra buenas opciones y arma tu carrito. Esta es una demostración de tienda: no procesa pagos reales.</p><button class="primary" onclick="document.querySelector('#catalog').scrollIntoView({behavior:'smooth'})">Ver productos</button></div>
  <div class="art" aria-hidden="true">🛍️</div>
 </section>
 <section id="catalog">
  <div class="toolbar"><h2>Productos destacados</h2><div class="filters" id="filters"></div></div>
  <div class="grid" id="products"></div>
 </section>
 <section class="panel" id="cart">
  <h2>Tu carrito</h2><div id="cartItems" class="muted">Todavía no has agregado productos.</div>
  <div class="cart-actions"><button class="primary" onclick="checkout()">Continuar</button><button class="secondary" onclick="clearCart()">Vaciar carrito</button></div>
 </section>
 <section class="panel" id="about"><h2>Información</h2><p class="muted">Mercado Nube es una plantilla demostrativa. Para vender públicamente hace falta conectar un servidor, una base de datos, autenticación segura, gestión de inventario y un proveedor de pagos autorizado. No ingreses contraseñas ni datos bancarios en esta demostración.</p></section>
</main>
<footer>© 2026 Mercado Nube · Tienda demostrativa · Condiciones · Privacidad · Atención al cliente</footer>
<dialog id="orderDialog"><form method="dialog" id="orderForm"><h2>Solicitud de pedido</h2><p class="muted">Demo: no se realizará ningún cobro ni envío.</p><label>Nombre para el pedido<input id="buyer" required maxlength="70" placeholder="Tu nombre"></label><label>Nota (opcional)<textarea id="note" rows="3" maxlength="250" placeholder="Comentarios sobre el pedido"></textarea></label><menu><button class="secondary" value="cancel">Cancelar</button><button class="primary" value="ok">Crear resumen</button></menu></form></dialog>
<script>
const products=[
{id:1,name:"Auriculares inalámbricos",price:18.99,cat:"Tecnología",emoji:"🎧"},
{id:2,name:"Reloj digital",price:24.50,cat:"Tecnología",emoji:"⌚"},
{id:3,name:"Mochila urbana",price:19.90,cat:"Accesorios",emoji:"🎒"},
{id:4,name:"Lámpara de escritorio",price:15.75,cat:"Hogar",emoji:"💡"},
{id:5,name:"Botella reutilizable",price:8.50,cat:"Hogar",emoji:"🥤"},
{id:6,name:"Gafas de sol",price:12.00,cat:"Accesorios",emoji:"🕶️"},
{id:7,name:"Teclado compacto",price:29.99,cat:"Tecnología",emoji:"⌨️"},
{id:8,name:"Cuaderno de notas",price:4.25,cat:"Papelería",emoji:"📓"}
];
let category="Todos",cart={};
const money=n=>"$"+n.toFixed(2);
function renderFilters(){let cats=["Todos",...new Set(products.map(p=>p.cat))];document.getElementById("filters").innerHTML=cats.map(c=>`<button class="${category===c?'active':''}" onclick="setCategory('${c}')">${c}</button>`).join("")}
function setCategory(c){category=c;render()}
function render(){renderFilters();let q=document.getElementById("search").value.toLowerCase();let list=products.filter(p=>(category==="Todos"||p.cat===category)&&p.name.toLowerCase().includes(q));document.getElementById("products").innerHTML=list.map(p=>`<article class="product"><div class="photo">${p.emoji}</div><div class="info"><div class="name">${p.name}</div><div class="price">${money(p.price)}</div><div class="ship">Envío sujeto a disponibilidad</div><button onclick="add(${p.id})">Agregar al carrito</button></div></article>`).join("")||'<p class="muted">No encontramos productos.</p>';renderCart()}
function add(id){cart[id]=(cart[id]||0)+1;renderCart()}
function renderCart(){let entries=Object.entries(cart);let count=entries.reduce((s,[,n])=>s+n,0);document.getElementById("count").textContent=count;let el=document.getElementById("cartItems");if(!entries.length){el.innerHTML="Todavía no has agregado productos.";return}let total=0;el.innerHTML=entries.map(([id,n])=>{let p=products.find(x=>x.id==id),sub=p.price*n;total+=sub;return `<div class="cartrow"><span>${p.emoji} ${p.name} × ${n}</span><b>${money(sub)}</b></div>`}).join("")+`<p><b>Total estimado: ${money(total)}</b></p>`}
function showCart(){document.getElementById("cart").scrollIntoView({behavior:"smooth"})}
function clearCart(){cart={};renderCart()}
function checkout(){if(!Object.keys(cart).length){alert("Agrega al menos un producto primero.");return}document.getElementById("orderDialog").showModal()}
document.getElementById("orderForm").addEventListener("submit",e=>{if(e.submitter?.value==="ok"){e.preventDefault();let name=document.getElementById("buyer").value.trim();if(!name)return;let lines=Object.entries(cart).map(([id,n])=>{let p=products.find(x=>x.id==id);return `${p.name} x ${n} — ${money(p.price*n)}`}).join("\n");let total=Object.entries(cart).reduce((s,[id,n])=>s+products.find(p=>p.id==id).price*n,0);alert(`RESUMEN DEMOSTRATIVO\nNombre: ${name}\n${lines}\nTotal: ${money(total)}\n\nNo es una compra real ni se ha realizado un pago.`);document.getElementById("orderDialog").close();}});
document.getElementById("search").addEventListener("input",render);
render();
</script>
</body></html>