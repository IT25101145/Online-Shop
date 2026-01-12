const products = [
  {id:1,name:"iPhone 15 Case",price:1290,img:"img/case.png",desc:"Crystal-clear slim case"},
  {id:2,name:"25W Fast Charger",price:2490,img:"img/charger.png",desc:"Samsung/Apple fast charge"},
  {id:3,name:"Type-C Cable 1m",price:690,img:"img/cable.png",desc:"Nylon braided, 3A"},
  {id:4,name:"AirPods Style Earbuds",price:3490,img:"img/earbuds.png",desc:"Bluetooth 5.3, touch control"},
  {id:5,name:"Tempered Glass",price:490,img:"img/glass.png",desc:"9H, full glue, 2-pack"},
  {id:6,name:"10 000 mAh Power Bank",price:3290,img:"img/powerbank.png",desc:"22.5W PD, dual ports"}
];

function render(list, node = document.getElementById('products') || document.getElementById('featured')) {
  node.innerHTML = '';
  list.forEach(p => {
    node.innerHTML += `
    <div class="card">
      <img src="${p.img}" alt="">
      <h4>${p.name}</h4>
      <p>Rs ${p.price}</p>
      <button class="btn" onclick="addToCart(${p.id})">Add to Cart</button>
      <a href="product.html?id=${p.id}">View</a>
    </div>`;
  });
}

function addToCart(id) {
  let cart = JSON.parse(localStorage.getItem('cart') || '[]');
  const item = cart.find(x => x.id == id);
  if (item) item.qty++;
  else cart.push({ ...products.find(p => p.id == id), qty: 1 });
  localStorage.setItem('cart', JSON.stringify(cart));
  document.getElementById('cart-count').textContent = cart.reduce((a, b) => a + b.qty, 0);
}

document.getElementById('year').textContent = new Date().getFullYear();
document.getElementById('cart-count').textContent = (JSON.parse(localStorage.getItem('cart') || '[]')).reduce((a, b) => a + b.qty, 0);

/* ---------- shop logic ---------- */
if (location.pathname.includes('shop')) {
  const node = document.getElementById('products');
  document.getElementById('filter-brand').onchange = e => {
    const v = e.target.value;
    render(products.filter(p => !v || p.name.toLowerCase().includes(v.toLowerCase())), node);
  };
  document.getElementById('sort').onchange = e => {
    const sorted = [...products].sort((a, b) => e.target.value === 'asc' ? a.price - b.price : b.price - a.price);
    render(sorted, node);
  };
  render(products, node);
} else {
  render(products.slice(0, 4)); // featured only
}