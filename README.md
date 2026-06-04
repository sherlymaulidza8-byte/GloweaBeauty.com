
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Admin Dashboard - Glowea Beauty</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
  <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    :root{
      --pink:#F8D7DA;
      --cream:#FFF6EE;
      --nude:#EFCFCF;
      --white:#FFFFFF;
      --text:#4a4a4a;
      --shadow:0 10px 30px rgba(0,0,0,0.08);
      --shadow-soft:0 6px 20px rgba(248,215,218,0.35);
      --radius:20px;
    }

    *{
      box-sizing:border-box;
    }

    body{
      background: linear-gradient(135deg, var(--cream), #fff, var(--pink));
      color: var(--text);
      overflow-x: hidden;
      font-family: 'Poppins', sans-serif;
    }

    .sidebar {
      width: 250px;
      min-height: 100vh;
      background: linear-gradient(180deg, #fff, #fff8f9);
      color: var(--text);
      position: fixed;
      top: 0;
      left: 0;
      padding: 24px 18px;
      box-shadow: var(--shadow);
      border-right: 1px solid rgba(239, 207, 207, 0.5);
    }

    .sidebar .brand{
      display:flex;
      align-items:center;
      gap:12px;
      margin-bottom: 30px;
      padding: 14px 16px;
      border-radius: 18px;
      background: linear-gradient(135deg, #fff, #fff0f3);
      box-shadow: var(--shadow-soft);
    }

    .sidebar .brand i{
      font-size: 1.5rem;
      color: #e78ca3;
    }

    .sidebar h4 {
      margin: 0;
      font-weight: 700;
      font-size: 1.05rem;
    }

    .sidebar small{
      color:#888;
      font-size:.82rem;
    }

    .sidebar a {
      color: #666;
      text-decoration: none;
      display: block;
      padding: 12px 14px;
      border-radius: 14px;
      margin-bottom: 10px;
      transition: .3s ease;
      font-weight: 500;
    }

    .sidebar a:hover,
    .sidebar a.active {
      background: linear-gradient(135deg, var(--pink), var(--nude));
      color: #222;
      transform: translateX(4px);
    }

    .sidebar a i{
      width: 22px;
    }

    .main-content {
      margin-left: 250px;
      padding: 22px;
    }

    .topbar {
      background: rgba(255,255,255,0.85);
      backdrop-filter: blur(12px);
      padding: 16px 20px;
      border-radius: 20px;
      box-shadow: var(--shadow);
      margin-bottom: 22px;
      border: 1px solid rgba(239, 207, 207, 0.35);
    }

    .topbar h3{
      margin:0;
      font-weight:700;
    }

    .topbar .welcome{
      color:#666;
      font-size:.95rem;
    }

    .card-stat {
      border: none;
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      background: rgba(255,255,255,0.88);
      backdrop-filter: blur(10px);
      transition: .3s ease;
    }

    .card-stat:hover{
      transform: translateY(-4px);
      box-shadow: 0 14px 35px rgba(0,0,0,0.12);
    }

    .stat-icon{
      width: 52px;
      height: 52px;
      border-radius: 16px;
      display:flex;
      align-items:center;
      justify-content:center;
      margin-bottom: 12px;
      background: linear-gradient(135deg, var(--pink), var(--nude));
      color:#222;
      font-size:1.3rem;
    }

    .card-title{
      font-size:.95rem;
      color:#777;
      margin-bottom:6px;
    }

    .card-value{
      font-size:1.7rem;
      font-weight:700;
      margin:0;
    }

    .topbar-cta{
      background: linear-gradient(135deg, var(--pink), var(--nude));
      color:#222;
      border:none;
      border-radius: 14px;
      padding: 10px 16px;
      font-weight: 600;
      transition:.3s ease;
    }

    .topbar-cta:hover{
      transform: translateY(-2px);
      box-shadow: var(--shadow-soft);
    }

    .chart-card,
    .table-card,
    .form-card {
      border: none;
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      background: rgba(255,255,255,0.88);
      backdrop-filter: blur(10px);
    }

    .section-title{
      font-weight:700;
      margin-bottom: 0;
    }

    .section-subtitle{
      color:#777;
      font-size:.92rem;
    }

    .list-group-item{
      border: none;
      padding-left: 0;
      padding-right: 0;
      background: transparent;
      color:#555;
    }

    .badge-soft{
      background: #fff0f4;
      color: #b34b67;
      border: 1px solid #f2c5d0;
      border-radius: 999px;
      padding: 7px 12px;
      font-weight: 500;
    }

    .table thead th{
      background: #fff4f7;
      color:#555;
      border-bottom: none;
      font-weight: 600;
    }

    .table tbody tr:hover{
      background: #fffafc;
    }

    .btn-pink{
      background: linear-gradient(135deg, var(--pink), var(--nude));
      color:#222;
      border:none;
      border-radius: 14px;
      font-weight: 600;
      padding: 10px 16px;
      transition:.3s ease;
    }

    .btn-pink:hover{
      transform: translateY(-2px);
      box-shadow: var(--shadow-soft);
      color:#222;
    }

    .btn-outline-soft{
      border:1px solid #f0c9d3;
      background:#fff;
      color:#555;
      border-radius: 14px;
      font-weight:600;
      padding: 10px 16px;
    }

    .btn-outline-soft:hover{
      background:#fff4f7;
      color:#222;
    }

    .form-control, .form-select, textarea{
      border-radius: 14px;
      border: 1px solid #eed8de;
      padding: 12px 14px;
    }

    .form-control:focus, .form-select:focus, textarea:focus{
      border-color: #efcfcf;
      box-shadow: 0 0 0 4px rgba(239,207,207,.22);
    }

    .action-btn{
      border-radius: 12px;
      padding: 7px 10px;
      margin-right: 6px;
      display:inline-flex;
      align-items:center;
      justify-content:center;
      width: 36px;
      height: 36px;
    }

    .product-thumb{
      width: 52px;
      height: 52px;
      object-fit: cover;
      border-radius: 14px;
      box-shadow: var(--shadow-soft);
    }

    @media (max-width: 768px) {
      .sidebar {
        position: relative;
        width: 100%;
        min-height: auto;
      }
      .main-content {
        margin-left: 0;
      }
    }
  </style>
</head>
<body>

  <div class="sidebar">
    <div class="brand">
      <i class="fa-solid fa-spa"></i>
      <div>
        <h4>Glowea Beauty</h4>
        <small>Admin Dashboard</small>
      </div>
    </div>

    <a href="#dashboard" class="active"><i class="fa-solid fa-gauge"></i> Dashboard</a>
    <a href="#users"><i class="fa-solid fa-users"></i> Users</a>
    <a href="#orders"><i class="fa-solid fa-bag-shopping"></i> Orders</a>
    <a href="#products"><i class="fa-solid fa-box"></i> Products</a>
    <a href="home.php"><i class="fa-solid fa-house"></i> Home</a>
    <a href="logout.php"><i class="fa-solid fa-right-from-bracket"></i> Logout</a>
  </div>

  <div class="main-content" id="dashboard">
    <div class="topbar d-flex justify-content-between align-items-center flex-wrap gap-2">
      <div>
        <h3 class="mb-1">Dashboard Admin</h3>
        <div class="welcome">Welcome, <?= htmlspecialchars($nama_admin) ?> ✨</div>
      </div>
      <button class="topbar-cta">
        <i class="fa-solid fa-sparkles me-1"></i> Premium Skincare Panel
      </button>
    </div>

    <div class="row g-3 mb-4">
      <div class="col-md-3 col-sm-6">
        <div class="card card-stat p-3">
          <div class="stat-icon"><i class="fa-solid fa-users"></i></div>
          <div class="card-title">Total Users</div>
          <p class="card-value"><?= $total_users ?></p>
          <small class="text-success">Data customer terdaftar</small>
        </div>
      </div>
      <div class="col-md-3 col-sm-6">
        <div class="card card-stat p-3">
          <div class="stat-icon"><i class="fa-solid fa-box"></i></div>
          <div class="card-title">Total Products</div>
          <p class="card-value"><?= $total_products ?></p>
          <small class="text-success">Produk skincare aktif</small>
        </div>
      </div>
      <div class="col-md-3 col-sm-6">
        <div class="card card-stat p-3">
          <div class="stat-icon"><i class="fa-solid fa-bag-shopping"></i></div>
          <div class="card-title">Total Orders</div>
          <p class="card-value"><?= $total_orders ?></p>
          <small class="text-warning">Pesanan customer</small>
        </div>
      </div>
      <div class="col-md-3 col-sm-6">
        <div class="card card-stat p-3">
          <div class="stat-icon"><i class="fa-solid fa-sack-dollar"></i></div>
          <div class="card-title">Revenue</div>
          <p class="card-value">Rp <?= number_format($total_revenue, 0, ',', '.') ?></p>
          <small class="text-success">Total pemasukan</small>
        </div>
      </div>
    </div>

    <div class="row g-3">
      <div class="col-md-8">
        <div class="card chart-card p-3">
          <div class="d-flex justify-content-between align-items-center mb-3">
            <div>
              <h5 class="section-title">Sales Overview</h5>
              <div class="section-subtitle">Grafik penjualan skincare</div>
            </div>
            <span class="badge-soft">2026</span>
          </div>
          <canvas id="salesChart" height="120"></canvas>
        </div>
      </div>

      <div class="col-md-4">
        <div class="card chart-card p-3 h-100">
          <h5 class="section-title mb-2">Recent Activity</h5>
          <div class="section-subtitle mb-3">Aktivitas terbaru sistem</div>
          <ul class="list-group list-group-flush">
            <li class="list-group-item"><i class="fa-solid fa-user-plus me-2 text-danger"></i> New user registered</li>
            <li class="list-group-item"><i class="fa-solid fa-bag-shopping me-2 text-danger"></i> Order baru masuk</li>
            <li class="list-group-item"><i class="fa-solid fa-box me-2 text-danger"></i> Produk baru ditambahkan</li>
            <li class="list-group-item"><i class="fa-solid fa-whatsapp me-2 text-danger"></i> Notifikasi WhatsApp terkirim</li>
          </ul>
        </div>
      </div>
    </div>

    <div class="card table-card p-3 mt-4" id="users">
      <div class="d-flex justify-content-between align-items-center flex-wrap gap-2 mb-3">
        <div>
          <h5 class="section-title">Data Users</h5>
          <div class="section-subtitle">Daftar user yang sudah register</div>
        </div>
      </div>
      <div class="table-responsive">
        <table class="table align-middle">
          <thead>
            <tr>
              <th>ID</th>
              <th>Nama</th>
              <th>Umur</th>
              <th>Jenis Kelamin</th>
              <th>Jenis Kulit</th>
              <th>Email</th>
            </tr>
          </thead>
          <tbody>
            <?php while($u = mysqli_fetch_assoc($users)): ?>
            <tr>
              <td>#<?= $u['id'] ?></td>
              <td><?= htmlspecialchars($u['nama']) ?></td>
              <td><?= $u['umur'] ?></td>
              <td><?= htmlspecialchars($u['jenis_kelamin']) ?></td>
              <td><span class="badge-soft"><?= htmlspecialchars($u['jenis_kulit']) ?></span></td>
              <td><?= htmlspecialchars($u['email']) ?></td>
            </tr>
            <?php endwhile; ?>
          </tbody>
        </table>
      </div>
    </div>

    <div class="card table-card p-3 mt-4" id="orders">
      <div class="d-flex justify-content-between align-items-center flex-wrap gap-2 mb-3">
        <div>
          <h5 class="section-title">Data Orders</h5>
          <div class="section-subtitle">Pesanan customer yang sudah checkout</div>
        </div>
      </div>
      <div class="table-responsive">
        <table class="table align-middle">
          <thead>
            <tr>
              <th>ID Order</th>
              <th>Customer</th>
              <th>Total</th>
              <th>Alamat</th>
              <th>WhatsApp</th>
              <th>Tanggal</th>
            </tr>
          </thead>
          <tbody>
            <?php while($o = mysqli_fetch_assoc($orders)): ?>
            <tr>
              <td>#<?= $o['id_order'] ?></td>
              <td><?= htmlspecialchars($o['nama']) ?></td>
              <td>Rp <?= number_format($o['total_harga'], 0, ',', '.') ?></td>
              <td><?= htmlspecialchars($o['alamat']) ?></td>
              <td><?= htmlspecialchars($o['nomor_whatsapp']) ?></td>
              <td><?= $o['tanggal_order'] ?></td>
            </tr>
            <?php endwhile; ?>
          </tbody>
        </table>
      </div>
    </div>

    <div class="card table-card p-3 mt-4" id="products">
      <div class="d-flex justify-content-between align-items-center flex-wrap gap-2 mb-3">
        <div>
          <h5 class="section-title">Data Products</h5>
          <div class="section-subtitle">Kelola produk skincare premium</div>
        </div>
        <a href="tambah_produk.php" class="btn btn-pink">
          <i class="fa-solid fa-plus me-1"></i> Tambah Produk
        </a>
      </div>
      <div class="table-responsive">
        <table class="table align-middle">
          <thead>
            <tr>
              <th>ID</th>
              <th>Gambar</th>
              <th>Nama Produk</th>
              <th>Harga</th>
              <th>Deskripsi</th>
              <th>Aksi</th>
            </tr>
          </thead>
          <tbody>
            <?php while($p = mysqli_fetch_assoc($products)): ?>
            <tr>
              <td><?= $p['id_produk'] ?></td>
              <td>
                <img src="<?= htmlspecialchars($p['gambar']) ?>" class="product-thumb" alt="produk">
              </td>
              <td><?= htmlspecialchars($p['nama_produk']) ?></td>
              <td>Rp <?= number_format($p['harga'], 0, ',', '.') ?></td>
              <td><?= htmlspecialchars($p['deskripsi']) ?></td>
              <td>
                <a href="edit_produk.php?id=<?= $p['id_produk'] ?>" class="btn btn-sm btn-outline-soft action-btn" title="Edit">
                  <i class="fa-solid fa-pen"></i>
                </a>
                <a href="hapus_produk.php?id=<?= $p['id_produk'] ?>" class="btn btn-sm btn-danger action-btn" title="Hapus" onclick="return confirm('Hapus produk ini?')">
                  <i class="fa-solid fa-trash"></i>
                </a>
              </td>
            </tr>
            <?php endwhile; ?>
          </tbody>
        </table>
      </div>
    </div>
  </div>

  <script>
    const ctx = document.getElementById('salesChart').getContext('2d');
    new Chart(ctx, {
      type: 'line',
      data: {
        labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
        datasets: [{
          label: 'Sales',
          data: [12, 19, 15, 25, 22, 30],
          borderColor: '#e78ca3',
          backgroundColor: 'rgba(248, 215, 218, 0.35)',
          tension: 0.4,
          fill: true,
          pointBackgroundColor: '#e78ca3',
          pointBorderColor: '#fff',
          pointRadius: 5
        }]
      },
      options: {
        responsive: true,
        plugins: {
          legend: {
            display: true
          }
        },
        scales: {
          y: {
            beginAtZero: true,
            grid: {
              color: 'rgba(0,0,0,0.05)'
            }
          },
          x: {
            grid: {
              display: false
            }
          }
        }
      }
    });
  </script>

</body>
</html>
