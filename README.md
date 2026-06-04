<?php
session_start();
include 'koneksi.php';

$error = "";
$success = "";

// ======================
// REGISTER
// ======================
if (isset($_POST['register'])) {
    $nama = mysqli_real_escape_string($conn, $_POST['nama']);
    $umur = mysqli_real_escape_string($conn, $_POST['umur']);
    $jenis_kelamin = mysqli_real_escape_string($conn, $_POST['jenis_kelamin']);
    $jenis_kulit = mysqli_real_escape_string($conn, $_POST['jenis_kulit']);
    $email = mysqli_real_escape_string($conn, $_POST['email']);
    $password = mysqli_real_escape_string($conn, $_POST['password']);
    $confirm_password = mysqli_real_escape_string($conn, $_POST['confirm_password']);

    if ($password !== $confirm_password) {
        $error = "Password dan konfirmasi password tidak sama.";
    } else {
        $cek = mysqli_query($conn, "SELECT * FROM users WHERE email='$email'");
        if (mysqli_num_rows($cek) > 0) {
            $error = "Email sudah terdaftar.";
        } else {
            $hash = password_hash($password, PASSWORD_DEFAULT);
            $query = mysqli_query($conn, "INSERT INTO users (nama, umur, jenis_kelamin, jenis_kulit, email, password) VALUES ('$nama', '$umur', '$jenis_kelamin', '$jenis_kulit', '$email', '$hash')");
            if ($query) {
                $success = "Registrasi berhasil, silakan login.";
            } else {
                $error = "Registrasi gagal.";
            }
        }
    }
}

// ======================
// LOGIN
// ======================
if (isset($_POST['login'])) {
    $email = mysqli_real_escape_string($conn, $_POST['email']);
    $password = mysqli_real_escape_string($conn, $_POST['password']);

    $query = mysqli_query($conn, "SELECT * FROM users WHERE email='$email'");
    if (mysqli_num_rows($query) > 0) {
        $data = mysqli_fetch_assoc($query);
        if (password_verify($password, $data['password'])) {
            $_SESSION['user_id'] = $data['id'];
            $_SESSION['nama'] = $data['nama'];
            $_SESSION['email'] = $data['email'];
            header("Location: index.php?page=home");
            exit;
        } else {
            $error = "Email atau password salah.";
        }
    } else {
        $error = "Email atau password salah.";
    }
}

// ======================
// CART ADD
// ======================
if (isset($_GET['add_cart'])) {
    if (!isset($_SESSION['user_id'])) {
        header("Location: index.php?page=login");
        exit;
    }
    $id_user = $_SESSION['user_id'];
    $id_produk = intval($_GET['add_cart']);

    $cek = mysqli_query($conn, "SELECT * FROM cart WHERE id_user='$id_user' AND id_produk='$id_produk'");
    if (mysqli_num_rows($cek) > 0) {
        mysqli_query($conn, "UPDATE cart SET jumlah = jumlah + 1 WHERE id_user='$id_user' AND id_produk='$id_produk'");
    } else {
        mysqli_query($conn, "INSERT INTO cart (id_user, id_produk, jumlah) VALUES ('$id_user', '$id_produk', 1)");
    }

    header("Location: index.php?page=cart");
    exit;
}

// ======================
// CART REMOVE
// ======================
if (isset($_GET['remove_cart'])) {
    if (!isset($_SESSION['user_id'])) {
        header("Location: index.php?page=login");
        exit;
    }
    $id_cart = intval($_GET['remove_cart']);
    $id_user = $_SESSION['user_id'];
    mysqli_query($conn, "DELETE FROM cart WHERE id_cart='$id_cart' AND id_user='$id_user'");
    header("Location: index.php?page=cart");
    exit;
}

// ======================
// CHECKOUT
// ======================
if (isset($_POST['checkout'])) {
    if (!isset($_SESSION['user_id'])) {
        header("Location: index.php?page=login");
        exit;
    }

    $id_user = $_SESSION['user_id'];
    $nama_customer = mysqli_real_escape_string($conn, $_POST['nama_customer']);
    $alamat = mysqli_real_escape_string($conn, $_POST['alamat']);
    $nomor_whatsapp = mysqli_real_escape_string($conn, $_POST['nomor_whatsapp']);
    $total_harga = intval($_POST['total_harga']);

    $insert = mysqli_query($conn, "INSERT INTO orders (id_user, total_harga, alamat, nomor_whatsapp, tanggal_order) VALUES ('$id_user', '$total_harga', '$alamat', '$nomor_whatsapp', NOW())");
    if ($insert) {
        mysqli_query($conn, "DELETE FROM cart WHERE id_user='$id_user'");
        $success = "Checkout berhasil! Pesanan Anda telah disimpan.";
    } else {
        $error = "Checkout gagal.";
    }
}

$page = isset($_GET['page']) ? $_GET['page'] : (isset($_SESSION['user_id']) ? 'home' : 'login');

// Ambil data produk
$products = mysqli_query($conn, "SELECT * FROM products ORDER BY id_produk DESC");

// Hitung cart
$cart_total_items = 0;
$cart_total_harga = 0;
if (isset($_SESSION['user_id'])) {
    $uid = $_SESSION['user_id'];
    $cart_count_q = mysqli_query($conn, "SELECT SUM(jumlah) AS total FROM cart WHERE id_user='$uid'");
    $cart_count_data = mysqli_fetch_assoc($cart_count_q);
    $cart_total_items = $cart_count_data['total'] ? $cart_count_data['total'] : 0;
}
?>
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Glow Beauty Skincare</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">
    <style>
        :root{
            --pink:#F8D7DA;
            --cream:#FFF6EE;
            --nude:#EFCFCF;
            --white:#FFFFFF;
            --text:#3b3b3b;
            --soft-shadow:0 10px 30px rgba(0,0,0,.08);
            --radius:22px;
        }
        *{box-sizing:border-box;margin:0;padding:0}
        body{
            font-family:'Poppins',sans-serif;
            background:linear-gradient(135deg,var(--cream),#fff, var(--pink));
            color:var(--text);
            scroll-behavior:smooth;
        }
        a{text-decoration:none;color:inherit}
        .container{width:min(1200px,92%);margin:auto}
        .btn{
            display:inline-block;padding:12px 22px;border:none;border-radius:999px;
            cursor:pointer;transition:.3s ease;font-weight:600
        }
        .btn:hover{transform:translateY(-3px);box-shadow:var(--soft-shadow)}
        .btn-primary{background:linear-gradient(135deg,var(--pink),var(--nude));color:#222}
        .btn-outline{background:#fff;border:1px solid #eee}
        .card{
            background:rgba(255,255,255,.8);
            backdrop-filter:blur(10px);
            border-radius:var(--radius);
            box-shadow:var(--soft-shadow);
        }
        .section{padding:80px 0}
        .title{font-size:2.2rem;font-weight:700;text-align:center;margin-bottom:10px}
        .subtitle{text-align:center;color:#666;margin-bottom:35px}
        .navbar{
            position:sticky;top:0;z-index:999;
            background:rgba(255,255,255,.75);backdrop-filter:blur(14px);
            box-shadow:0 4px 20px rgba(0,0,0,.05)
        }
        .nav-wrap{display:flex;justify-content:space-between;align-items:center;padding:16px 0}
        .logo{display:flex;align-items:center;gap:10px;font-weight:700;font-size:1.2rem}
        .logo i{color:#e78ca3}
        .nav-links{display:flex;gap:18px;align-items:center;flex-wrap:wrap}
        .nav-links a{padding:8px 12px;border-radius:999px;transition:.3s}
        .nav-links a:hover{background:#fff0f3}
        .badge{
            background:#ff6b81;color:#fff;border-radius:999px;padding:2px 8px;font-size:.75rem
        }
        .hero{
            min-height:90vh;display:grid;grid-template-columns:1.1fr .9fr;gap:30px;
            align-items:center;padding:60px 0
        }
        .hero-box{
            padding:40px
        }
        .hero h1{font-size:3.5rem;line-height:1.05;margin-bottom:18px}
        .hero p{font-size:1.05rem;color:#666;margin-bottom:24px;max-width:560px}
        .hero-visual{
            height:520px;border-radius:32px;
            background:linear-gradient(135deg,#fff,#fde6ea,#fff6ee);
            display:flex;align-items:center;justify-content:center;position:relative;overflow:hidden
        }
        .blob{
            width:280px;height:280px;border-radius:50%;
            background:radial-gradient(circle at top left,#f8d7da,#efcfcf);
            filter:blur(2px);opacity:.9
        }
        .floating{
            position:absolute;width:90px;height:90px;border-radius:22px;background:rgba(255,255,255,.75);
            box-shadow:var(--soft-shadow);display:flex;align-items:center;justify-content:center;font-size:2rem
        }
        .f1{top:50px;left:50px}.f2{bottom:70px;right:60px}.f3{top:120px;right:80px}
        .grid-products{
            display:grid;grid-template-columns:repeat(auto-fit,minmax(240px,1fr));gap:22px
        }
        .product{
            overflow:hidden;transition:.35s ease
        }
        .product:hover{transform:translateY(-8px)}
        .product img{width:100%;height:240px;object-fit:cover;display:block}
        .product-body{padding:18px}
        .product h3{font-size:1.05rem;margin-bottom:6px}
        .price{color:#c25b74;font-weight:700;margin:8px 0 14px}
        .small{font-size:.92rem;color:#666}
        .auth-wrap{
            width:min(460px,92%);
            margin:70px auto;
            padding:34px
        }
        .auth-wrap h2{text-align:center;margin-bottom:18px}
        form{display:grid;gap:14px}
        input,select,textarea{
            width:100%;padding:13px 15px;border:1px solid #eee;border-radius:16px;
            outline:none;font-family:inherit;background:#fff
        }
        input:focus,select:focus,textarea:focus{border-color:#efcfcf;box-shadow:0 0 0 4px rgba(239,207,207,.25)}
        .alert{
            padding:12px 16px;border-radius:14px;margin-bottom:16px
        }
        .error{background:#ffe8ea;color:#b00020}
        .success{background:#ecfff0;color:#146c2e}
        .two-col{display:grid;grid-template-columns:1fr 1fr;gap:14px}
        .cart-list,.checkout-box,.admin-box{padding:26px}
        .cart-item{
            display:grid;grid-template-columns:90px 1fr auto;gap:14px;align-items:center;
            padding:14px 0;border-bottom:1px solid #f1f1f1
        }
        .cart-item img{width:90px;height:90px;border-radius:18px;object-fit:cover}
        .total-box{
            display:flex;justify-content:space-between;align-items:center;margin-top:16px;
            padding-top:16px;border-top:2px dashed #f1d5d9;font-weight:700
        }
        .footer{
            padding:30px 0;text-align:center;color:#666
        }
        .socials{display:flex;justify-content:center;gap:14px;margin-top:12px}
        .socials a{
            width:44px;height:44px;border-radius:50%;background:#fff;display:flex;align-items:center;justify-content:center;
            box-shadow:var(--soft-shadow)
        }
        .chatbot{
            position:fixed;right:20px;bottom:20px;z-index:9999
        }
        .chatbot a{
            width:58px;height:58px;border-radius:50%;background:#25d366;color:#fff;
            display:flex;align-items:center;justify-content:center;font-size:1.7rem;box-shadow:var(--soft-shadow)
        }
        .responsive-menu{display:none}
        .hidden{display:none}
        .admin-table{width:100%;border-collapse:collapse;overflow:hidden}
        .admin-table th,.admin-table td{padding:12px;border-bottom:1px solid #f0f0f0;text-align:left;font-size:.95rem}
        .admin-table th{background:#fff4f6}
        .section-box{margin-top:30px}
        .testi-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(240px,1fr));gap:20px}
        .testi{padding:22px}
        .avatar{width:64px;height:64px;border-radius:50%;object-fit:cover;margin-bottom:12px}
        .anim{animation:fadeUp .8s ease}
        @keyframes fadeUp{from{opacity:0;transform:translateY(18px)}to{opacity:1;transform:translateY(0)}}
        @media(max-width:900px){
            .hero{grid-template-columns:1fr}
            .hero h1{font-size:2.5rem}
            .nav-links{display:none}
            .responsive-menu{display:block}
            .two-col{grid-template-columns:1fr}
        }
    </style>
</head>
<body>

<nav class="navbar">
    <div class="container nav-wrap">
        <div class="logo"><i class="fa-solid fa-spa"></i> Glow Skin</div>
        <div class="nav-links">
            <?php if(isset($_SESSION['user_id'])): ?>
                <a href="#home">Home</a>
                <a href="#product">Product</a>
                <a href="#about">About</a>
                <a href="#contact">Contact</a>
                <a href="index.php?page=cart">Cart <span class="badge"><?php echo $cart_total_items; ?></span></a>
                <a href="index.php?page=dashboard">Dashboard</a>
                <a href="logout.php">Logout</a>
            <?php else: ?>
                <a href="index.php?page=login">Login</a>
                <a href="index.php?page=register">Register</a>
            <?php endif; ?>
        </div>
    </div>
</nav>

<div class="container">
<?php if($error): ?><div class="alert error anim"><?php echo $error; ?></div><?php endif; ?>
<?php if($success): ?><div class="alert success anim"><?php echo $success; ?></div><?php endif; ?>
</div>

<?php if($page == 'login'): ?>
<div class="auth-wrap card anim">
    <h2>Login</h2>
    <form method="post">
        <input type="email" name="email" placeholder="Email" required>
        <input type="password" name="password" placeholder="Password" required>
        <button class="btn btn-primary" type="submit" name="login">Login</button>
        <p style="text-align:center">Belum punya akun? <a href="index.php?page=register"><b>Register</b></a></p>
    </form>
</div>

<?php elseif($page == 'register'): ?>
<div class="auth-wrap card anim">
    <h2>Register</h2>
    <form method="post">
        <input type="text" name="nama" placeholder="Nama lengkap" required>
        <input type="number" name="umur" placeholder="Umur" required>
        <select name="jenis_kelamin" required>
            <option value="">Jenis Kelamin</option>
            <option value="Perempuan">Perempuan</option>
            <option value="Laki-laki">Laki-laki</option>
        </select>
        <select name="jenis_kulit" required>
            <option value="">Jenis Kulit</option>
            <option value="berminyak">Berminyak</option>
            <option value="kering">Kering</option>
            <option value="kombinasi">Kombinasi</option>
            <option value="sensitif">Sensitif</option>
            <option value="normal">Normal</option>
        </select>
        <input type="email" name="email" placeholder="Email" required>
        <input type="password" name="password" placeholder="Password" required>
        <input type="password" name="confirm_password" placeholder="Konfirmasi Password" required>
        <button class="btn btn-primary" type="submit" name="register">Register</button>
        <p style="text-align:center">Sudah punya akun? <a href="index.php?page=login"><b>Login</b></a></p>
    </form>
</div>

<?php elseif($page == 'home' && isset($_SESSION['user_id'])): ?>
<section class="section" id="home">
    <div class="container hero">
        <div class="hero-box card anim">
            <h1>Reveal Your Natural Glow ✨</h1>
            <p>Website skincare premium dengan nuansa clean girl aesthetic dan Korean skincare vibes. Elegan, lembut, modern, minimalis, dan responsive.</p>
            <a href="#product" class="btn btn-primary">Shop Now</a>
        </div>
        <div class="hero-visual card anim">
            <div class="blob"></div>
            <div class="floating f1"><i class="fa-solid fa-leaf" style="color:#e78ca3"></i></div>
            <div class="floating f2"><i class="fa-solid fa-heart" style="color:#e78ca3"></i></div>
            <div class="floating f3"><i class="fa-solid fa-sparkles" style="color:#e78ca3"></i></div>
        </div>
    </div>
</section>

<section class="section" id="product">
    <div class="container">
        <h2 class="title">Produk Skincare</h2>
        <p class="subtitle">Produk premium pilihan untuk kulit sehat dan glowing</p>
        <div class="grid-products">
            <?php while($p = mysqli_fetch_assoc($products)): ?>
            <div class="product card anim">
                <img src="<?php echo htmlspecialchars($p['gambar']); ?>" alt="<?php echo htmlspecialchars($p['nama_produk']); ?>">
                <div class="product-body">
                    <h3><?php echo htmlspecialchars($p['nama_produk']); ?></h3>
                    <div class="price">Rp <?php echo number_format($p['harga'],0,',','.'); ?></div>
                    <p class="small"><?php echo htmlspecialchars($p['deskripsi']); ?></p>
                    <div style="margin-top:14px">
                        <a class="btn btn-primary" href="index.php?add_cart=<?php echo $p['id_produk']; ?>"><i class="fa-solid fa-cart-plus"></i> Add to Cart</a>
                    </div>
                </div>
            </div>
            <?php endwhile; ?>
        </div>
    </div>
</section>

<section class="section" id="about">
    <div class="container card" style="padding:34px">
        <h2 class="title">About</h2>
        <p class="subtitle">Toko skincare premium dengan tampilan clean, soft, dan feminin.</p>
        <p>Kami menghadirkan produk skincare yang dipilih dengan konsep Korean skincare vibes, cocok untuk kamu yang suka tampilan elegan dan simple tapi tetap mewah.</p>
    </div>
</section>

<section class="section" id="testimoni">
    <div class="container">
        <h2 class="title">Testimoni Customer</h2>
        <p class="subtitle">Cerita cantik dari pelanggan kami</p>
        <div class="testi-grid">
            <div class="card testi anim">
                <img class="avatar" src="https://images.unsplash.com/photo-1494790108377-be9c29b29330?w=200" alt="">
                <h3>Putri</h3>
                <p>Serumnya bagus banget, kulit jadi lebih glowing dan lembap.</p>
            </div>
            <div class="card testi anim">
                <img class="avatar" src="https://images.unsplash.com/photo-1438761681033-6461ffad8d80?w=200" alt="">
                <h3>Salsa</h3>
                <p>Packaging-nya aesthetic, produknya juga original.</p>
            </div>
            <div class="card testi anim">
                <img class="avatar" src="https://images.unsplash.com/photo-1544005313-94ddf0286df2?w=200" alt="">
                <h3>Amara</h3>
                <p>Pelayanannya ramah, cocok banget buat skincare routine harian.</p>
            </div>
        </div>
    </div>
</section>

<section class="section" id="contact">
    <div class="container card" style="padding:34px;text-align:center">
        <h2 class="title">Contact</h2>
        <p class="subtitle">Instagram, TikTok, dan WhatsApp tersedia</p>
        <div class="socials">
            <a href="#"><i class="fa-brands fa-instagram"></i></a>
            <a href="#"><i class="fa-brands fa-tiktok"></i></a>
            <a href="https://wa.me/6281234567890?text=Halo%20kak%20%F0%9F%92%95%20Selamat%20datang%20di%20toko%20skincare%20kami.%20Ada%20yang%20bisa%20kami%20bantu%3F" target="_blank"><i class="fa-brands fa-whatsapp"></i></a>
        </div>
    </div>
</section>

<footer class="footer">© 2026 Glow Skin. All rights reserved.</footer>

<div class="chatbot">
    <a target="_blank" href="https://wa.me/6281234567890?text=Halo%20kak%20%F0%9F%92%95%20Selamat%20datang%20di%20toko%20skincare%20kami.%20Ada%20yang%20bisa%20kami%20bantu%3F">
        <i class="fa-brands fa-whatsapp"></i>
    </a>
</div>

<?php elseif($page == 'cart' && isset($_SESSION['user_id'])): ?>
<div class="container section">
    <h2 class="title">Shopping Cart</h2>
    <div class="card cart-list">
    <?php
    $uid = $_SESSION['user_id'];
    $cart = mysqli_query($conn,"SELECT cart.*, products.nama_produk, products.harga, products.gambar FROM cart JOIN products ON cart.id_produk=products.id_produk WHERE cart.id_user='$uid'");
    $grand = 0;
    while($c = mysqli_fetch_assoc($cart)):
        $sub = $c['harga'] * $c['jumlah'];
        $grand += $sub;
    ?>
        <div class="cart-item">
            <img src="<?php echo htmlspecialchars($c['gambar']); ?>" alt="">
            <div>
                <h3><?php echo htmlspecialchars($c['nama_produk']); ?></h3>
                <p>Jumlah: <?php echo $c['jumlah']; ?></p>
                <p>Subtotal: Rp <?php echo number_format($sub,0,',','.'); ?></p>
            </div>
            <a class="btn btn-outline" href="index.php?remove_cart=<?php echo $c['id_cart']; ?>">Hapus</a>
        </div>
    <?php endwhile; ?>
        <div class="total-box">
            <span>Total</span>
            <span>Rp <?php echo number_format($grand,0,',','.'); ?></span>
        </div>
        <div style="margin-top:20px">
            <a href="index.php?page=checkout" class="btn btn-primary">Checkout</a>
        </div>
    </div>
</div>

<?php elseif($page == 'checkout' && isset($_SESSION['user_id'])): ?>
<div class="container section">
    <h2 class="title">Checkout</h2>
    <div class="card checkout-box">
        <form method="post">
            <input type="text" name="nama_customer" placeholder="Nama Customer" required>
            <textarea name="alamat" rows="4" placeholder="Alamat lengkap" required></textarea>
            <input type="text" name="nomor_whatsapp" placeholder="Nomor WhatsApp" required>
            <input type="number" name="total_harga" placeholder="Total Pembayaran" required>
            <button class="btn btn-primary" type="submit" name="checkout">Simpan Checkout</button>
        </form>
    </div>
</div>

<?php elseif($page == 'dashboard' && isset($_SESSION['user_id'])): ?>
<div class="container section">
    <h2 class="title">Admin Dashboard</h2>

    <div class="section-box card admin-box">
        <h3>Data User</h3>
        <table class="admin-table">
            <tr><th>ID</th><th>Nama</th><th>Umur</th><th>Kelamin</th><th>Kulit</th><th>Email</th></tr>
            <?php
            $userq = mysqli_query($conn, "SELECT * FROM users ORDER BY id DESC");
            while($u = mysqli_fetch_assoc($userq)):
            ?>
            <tr>
                <td><?php echo $u['id']; ?></td>
                <td><?php echo htmlspecialchars($u['nama']); ?></td>
                <td><?php echo $u['umur']; ?></td>
                <td><?php echo $u['jenis_kelamin']; ?></td>
                <td><?php echo $u['jenis_kulit']; ?></td>
                <td><?php echo htmlspecialchars($u['email']); ?></td>
            </tr>
            <?php endwhile; ?>
        </table>
    </div>

    <div class="section-box card admin-box">
        <h3>Data Pesanan</h3>
        <table class="admin-table">
            <tr><th>ID Order</th><th>User</th><th>Total</th><th>Alamat</th><th>WA</th><th>Tanggal</th></tr>
            <?php
            $orderq = mysqli_query($conn, "SELECT orders.*, users.nama FROM orders JOIN users ON orders.id_user=users.id ORDER BY id_order DESC");
            while($o = mysqli_fetch_assoc($orderq)):
            ?>
            <tr>
                <td><?php echo $o['id_order']; ?></td>
                <td><?php echo htmlspecialchars($o['nama']); ?></td>
                <td>Rp <?php echo number_format($o['total_harga'],0,',','.'); ?></td>
                <td><?php echo htmlspecialchars($o['alamat']); ?></td>
                <td><?php echo htmlspecialchars($o['nomor_whatsapp']); ?></td>
                <td><?php echo $o['tanggal_order']; ?></td>
            </tr>
            <?php endwhile; ?>
        </table>
    </div>

    <div class="section-box card admin-box">
        <h3>Data Produk</h3>
        <table class="admin-table">
            <tr><th>ID</th><th>Nama</th><th>Harga</th><th>Deskripsi</th><th>Gambar</th></tr>
            <?php
            $prodq = mysqli_query($conn, "SELECT * FROM products ORDER BY id_produk DESC");
            while($pr = mysqli_fetch_assoc($prodq)):
            ?>
            <tr>
                <td><?php echo $pr['id_produk']; ?></td>
                <td><?php echo htmlspecialchars($pr['nama_produk']); ?></td>
                <td>Rp <?php echo number_format($pr['harga'],0,',','.'); ?></td>
                <td><?php echo htmlspecialchars($pr['deskripsi']); ?></td>
                <td><?php echo htmlspecialchars($pr['gambar']); ?></td>
            </tr>
            <?php endwhile; ?>
        </table>
    </div>
</div>
<?php endif; ?>

</body>
</html>
