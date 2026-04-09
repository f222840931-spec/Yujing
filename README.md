<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>yujing 優淨 - 專業美髮品牌</title>
    <style>
        :root {
            --primary-color: #333;
            --bg-color: #ffffff;
            --accent-color: #f8f8f8;
            --price-color: #c0392b;
        }

        body {
            font-family: "PingFang TC", "Microsoft JhengHei", sans-serif;
            margin: 0;
            background-color: var(--bg-color);
            color: var(--primary-color);
        }

        /* 導覽列 */
        header {
            padding: 20px 5%;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid #eee;
            position: sticky;
            top: 0;
            background: #fff;
            z-index: 100;
        }

        .logo { font-size: 24px; font-weight: bold; letter-spacing: 2px; }

        /* 商品區 */
        .container { padding: 40px 5%; }
        .product-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 30px;
        }

        .product-card {
            border: 1px solid #eee;
            padding: 15px;
            text-align: center;
            transition: 0.3s;
        }

        .product-card:hover { box-shadow: 0 5px 15px rgba(0,0,0,0.05); }

        .product-img {
            width: 100%;
            height: 200px;
            background: #f9f9f9;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #ccc;
        }

        .btn-add {
            background: #333;
            color: #fff;
            border: none;
            padding: 10px 20px;
            cursor: pointer;
            width: 100%;
        }

        /* 購物車側欄 */
        #cart-panel {
            position: fixed;
            right: -400px;
            top: 0;
            width: 350px;
            height: 100%;
            background: white;
            box-shadow: -2px 0 10px rgba(0,0,0,0.1);
            transition: 0.3s;
            padding: 20px;
            z-index: 1001;
        }

        /* 註冊禮彈窗 */
        #promo-modal {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: white;
            padding: 40px;
            border: 2px solid #000;
            z-index: 2000;
            text-align: center;
        }

        .overlay {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.5);
            z-index: 1999;
        }

        .shipping-info { font-size: 14px; color: #666; margin: 10px 0; }
    </style>
</head>
<body>

<header>
    <div class="logo">yujing 優淨</div>
    <div class="nav-icons">
        <span onclick="toggleCart()" style="cursor:pointer">🛒 購物車 (<span id="cart-count">0</span>)</span>
    </div>
</header>

<div class="container">
    <div class="shipping-info">✨ 全館滿 $1500 免運 | 首次註冊送 $100 折價券</div>
    
    <div class="product-grid">
        <div class="product-card">
            <div class="product-img">商品圖片</div>
            <h3>優淨 柔順洗髮精</h3>
            <p>$680</p>
            <button class="btn-add" onclick="addToCart('柔順洗髮精', 680)">加入購物車</button>
        </div>
        <div class="product-card">
            <div class="product-img">商品圖片</div>
            <h3>優淨 護髮精油</h3>
            <p>$850</p>
            <button class="btn-add" onclick="addToCart('護髮精油', 850)">加入購物車</button>
        </div>
    </div>
</div>

<div id="cart-panel">
    <h2>您的購物車</h2>
    <div id="cart-items"></div>
    <hr>
    <p>小計: $<span id="subtotal">0</span></p>
    <p>運費: $<span id="shipping-fee">0</span></p>
    <p style="font-weight: bold; font-size: 1.2em;">總計: $<span id="total">0</span></p>
    
    <h4>配送方式</h4>
    <select id="ship-method" onchange="calculateTotal()">
        <option value="100">宅配 ($100)</option>
        <option value="60">店到店 ($60)</option>
    </select>

    <h4>付款方式</h4>
    <select>
        <option>貨到付款</option>
        <option>銀行轉帳 (請於備註填寫末五碼)</option>
    </select>
    
    <button class="btn-add" style="margin-top:20px">前往結帳</button>
    <button onclick="toggleCart()" style="margin-top:10px; background:none; border:none; color:grey; cursor:pointer">關閉</button>
</div>

<div id="overlay" class="overlay"></div>
<div id="promo-modal">
    <h2>歡迎來到 yujing 優淨</h2>
    <p>首次加入會員，立即領取</p>
    <h1 style="color:var(--price-color)">$100 折價券</h1>
    <button class="btn-add" onclick="closeModal()">立即領取並購物</button>
</div>

<script>
    let cart = [];
    let subtotal = 0;

    function addToCart(name, price) {
        cart.push({name, price});
        updateCart();
        if(document.getElementById('cart-panel').style.right !== '0px') toggleCart();
    }

    function updateCart() {
        document.getElementById('cart-count').innerText = cart.length;
        const list = document.getElementById('cart-items');
        list.innerHTML = cart.map(item => `<p>${item.name} - $${item.price}</p>`).join('');
        
        subtotal = cart.reduce((sum, item) => sum + item.price, 0);
        document.getElementById('subtotal').innerText = subtotal;
        calculateTotal();
    }

    function calculateTotal() {
        let shipFee = parseInt(document.getElementById('ship-method').value);
        if (subtotal >= 1500 || subtotal === 0) {
            shipFee = 0;
        }
        document.getElementById('shipping-fee').innerText = shipFee;
        document.getElementById('total').innerText = subtotal + shipFee;
    }

    function toggleCart() {
        const panel = document.getElementById('cart-panel');
        panel.style.right = panel.style.right === '0px' ? '-400px' : '0px';
    }

    function closeModal() {
        document.getElementById('promo-modal').style.display = 'none';
        document.getElementById('overlay').style.display = 'none';
    }
</script>

</body>
</html>
