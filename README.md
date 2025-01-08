# UAS21.01.55.0003
NABILA ANTANIA PUTRI ANJANI
### Login.php 
```
<?php
session_start();
require_once 'config.php';

if(isset($_POST['login'])) {
    $username = $_POST['username'];
    $password = $_POST['password'];
    
    // Gunakan fungsi PASSWORD() untuk mencocokkan dengan hash di database
    $query = "SELECT * FROM users WHERE username='$username' AND password=PASSWORD('$password')";
    $result = mysqli_query($conn, $query);
    
    if(mysqli_num_rows($result) == 1) {
        $row = mysqli_fetch_assoc($result);
        $_SESSION['user_id'] = $row['id'];
        $_SESSION['username'] = $row['username'];
        $_SESSION['name'] = $row['name'];
            
        header("Location: index.php");
        exit();
    } else {
        $error = "Username atau Password salah!";
    }
}
?>

<!DOCTYPE html>
<html>
<head>
    <title>Login</title>
   <style>
    body {
            background-color:rgb(255, 179, 185); /* Warna latar belakang merah muda */
            background-image: url('path/to/your/cake-shop-background.jpg'); /* Ganti dengan path gambar Anda */
            background-size: cover; /* Mengatur gambar agar menutupi seluruh latar belakang */
            background-position: center; /* Memusatkan gambar */
            font-family: Arial, sans-serif; /* Font yang lebih modern */
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh; /* Mengisi tinggi viewport */
            margin: 0;
            color: white; /* Mengubah warna teks menjadi putih untuk kontras yang lebih baik */
        }
        .login-container {
            background-color: rgba(109, 46, 46, 0.9); /* Warna latar belakang untuk formulir dengan transparansi */
            padding: 20px;
            border-radius: 8px; /* Sudut yang membulat */
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2); /* Bayangan untuk efek kedalaman */
            width: 300px; /* Lebar tetap untuk formulir */
        }
        h1 {
            text-align: center; /* Pusatkan judul */
            color: #333; /* Warna teks judul */
            margin-bottom: 20px; /* Jarak bawah judul */
        }
        p {
            margin: 10px 0; /* Jarak antar elemen */
        }
        input[type="text"],
        input[type="password"] {
            width: 100%; /* Lebar penuh */
            padding: 10px; /* Padding untuk ruang dalam */
            border: 1px solid #ccc; /* Border abu-abu */
            border-radius: 5px; /* Sudut yang membulat */
            box-sizing: border-box; /* Menghitung padding dan border dalam lebar */
        }
        input[type="submit"] {
            background-color:rgb(207, 82, 176); /* Warna latar belakang tombol */
            color: white; /* Warna teks tombol */
            border: none; /* Tanpa border */
            padding: 10px; /* Padding untuk tombol */
            border-radius: 5px; /* Sudut yang membulat */
            cursor: pointer; /* Kursor pointer saat hover */
            width: 100%; /* Lebar penuh */
            transition: background-color 0.3s; /* Transisi untuk efek hover */
        }
        input[type="submit"]:hover {
            background-color:rgb(207, 82, 176); /* Warna saat hover */
        }
        .error {
            color: red; /* Warna merah untuk pesan error */
            text-align: center; /* Pusatkan pesan error */
        }
    </style>
</head>
<body>
<div class="login-container">
        <h1>Login Form</h1> <!-- Judul dipindahkan ke sini -->
        <?php if(isset($error)) { ?>
            <p class="error"><?php echo $error; ?></p>
        <?php } ?>
    
    <form action="" method="POST">
        <p>
            Username: <input type="text" name="username" required>
        </p>
        <p>
            Password: <input type="password" name="password" required>
        </p>
        <p>
            <input type="submit" name="login" value="Login">
        </p>
    </form>
</body>
</html>
```
### config.php
```
<?php
$servername = "localhost";  // Ganti dengan server Anda
$username = "root";         // Ganti dengan username database Anda
$password = "";             // Ganti dengan password database Anda
$dbname = "moviestore";  // Ganti dengan nama database Anda

// Membuat koneksi
$conn = new mysqli($servername, $username, $password, $dbname);

// Cek koneksi
if ($conn->connect_error) {
    die("Koneksi gagal: " . $conn->connect_error);
}
?>
```
### index.php
```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Daftar Movies</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <!-- <style>
        .btn-group-action {
            white-space: nowrap;
        }
    </style> -->
    <style>
        body {
            background-color: rgb(245, 204, 227);
            /* Warna latar belakang merah muda */
        }

        h1 {
            color: #343a40;
            margin-bottom: 20px;
        }

        table {
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            border-radius: 8px;
            overflow: hidden;
            background-color: #ffffff;
            /* Warna latar belakang untuk tabel */
        }

        th,
        td {
            text-align: center;
        }

        .btn-group-action {
            white-space: nowrap;
        }

        .btn {
            transition: background-color 0.3s, transform 0.3s;
        }

        .btn:hover {
            transform: translateY(-2px);
        }

        .modal-content {
            border-radius: 8px;
            background-color: #f8f9fa;
            /* Warna latar belakang untuk modal */
        }

        .modal-header {
            background-color: #007bff;
            color: white;
        }

        .form-control {
            border-radius: 5px;
        }

        .form-control:focus {
            box-shadow: 0 0 5px rgba(0, 123, 255, 0.5);
            border-color: #80bdff;
        }

        .modal-footer .btn {
            border-radius: 5px;
        }
    </style>
</head>
<style>
    .btn-group-action {
        white-space: nowrap;
    }
</style>
</head>

<body class="container py-4">

    <h1>Daftar Movies</h1>
    <div class="row mb-3">
        <div class="col">
            <input type="text" id="searchInput" class="form-control" placeholder="Cari berdasarkan ID">
        </div>
        <div class="col-auto">
            <button onclick="searchMovie()" class="btn btn-primary">Cari</button>
        </div>
        <div class="col-auto">
            <button type="button" class="btn btn-success" data-bs-toggle="modal" data-bs-target="#movieModal">
                Tambah Movie
                <button type="button" class="btn btn-danger ms-2" onclick="logout()">Logout</button>
            </button>
        </div>
    </div>

    <table class="table table-striped">
        <thead class="table-dark">
            <tr>
                <th>ID</th>
                <th>Judul</th>
                <th>Genre</th>
                <th>Tahun</th>
                <th>Rating</th>
            </tr>
        </thead>
        <tbody id="movieList">
        </tbody>
    </table>

    <!-- Modal untuk Tambah/Edit Movie -->
    <div class="modal fade" id="movieModal" tabindex="-1">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="modalTitle">Tambah Movie</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
                </div>
                <div class="modal-body">
                    <form id="movieForm">
                        <input type="hidden" id="movieId">
                        <div class="mb-3">
                            <label for="title" class="form-label">Judul</label>
                            <input type="text" class="form-control" id="title" required>
                        </div>
                        <div class="mb-3">
                            <label for="genre" class="form-label">Genre</label>
                            <input type="text" class="form-control" id="genre" required>
                        </div>
                        <div class="mb-3">
                            <label for="year" class="form-label">Tahun</label>
                            <input type="number" class="form-control" id="year" required>
                        </div>
                        <div class="mb-3">
                            <label for="rating" class="form-label">Rating</label>
                            <input type="number" class="form-control" id="rating" required>
                        </div>
                    </form>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Batal</button>
                    <button type="button" class="btn btn-primary" onclick="saveMovie()">Simpan</button>
                </div>
            </div>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"></script>
    <script>
        const API_URL = 'http://localhost/rest_movies/movies_api.php';
        let movieModal;

        document.addEventListener('DOMContentLoaded', function() {
            movieModal = new bootstrap.Modal(document.getElementById('movieModal'));
            loadMovies(); // Memuat semua movies saat halaman pertama kali dibuka
        });

        function loadMovies() {
            fetch(API_URL)
                .then(response => response.json())
                .then(movies => {
                    const movieList = document.getElementById('movieList');
                    movieList.innerHTML = '';
                    movies.forEach(movie => {
                        movieList.innerHTML +=
                            `<tr>
                                <td>${movie.id}</td>
                                <td>${movie.title}</td>
                                <td>${movie.genre}</td>
                                <td>${movie.year}</td>
                                <td>${movie.rating}</td>
                                <td class="btn-group-action">
                                    <button class="btn btn-sm btn-warning me-1" onclick="editMovie(${movie.id})">Edit</button>
                                    <button class="btn btn-sm btn-danger" onclick="deleteMovie(${movie.id})">Hapus</button>
                                </td>
                            </tr>`;
                    });
                })
                .catch(error => alert('Error loading movies: ' + error));
        }

        function searchMovie() {
            const id = document.getElementById('searchInput').value.trim();
            if (!id) {
                loadMovies(); // Jika input ID kosong, tampilkan semua movies
                return;
            }

            const url = `${API_URL}/${id}`;

            fetch(url)
                .then(response => response.json())
                .then(movie => {
                    const movieList = document.getElementById('movieList');
                    movieList.innerHTML = '';

                    if (movie.message) {
                        alert('Movie not found');
                        return;
                    }

                    movieList.innerHTML =
                        `<tr>
                            <td>${movie.id}</td>
                            <td>${movie.title}</td>
                            <td>${movie.genre}</td>
                            <td>${movie.year}</td>
                            <td>${movie.rating}</td>
                            <td class="btn-group-action">
                                <button class="btn btn-sm btn-warning me-1" onclick="editMovie(${movie.id})">Edit</button>
                                <button class="btn btn-sm btn-danger" onclick="deleteMovie(${movie.id})">Hapus</button>
                            </td>
                        </tr>`;
                })
                .catch(error => alert('Error searching movie: ' + error));
        }

        function editMovie(id) {
            fetch(`${API_URL}/${id}`)
                .then(response => response.json())
                .then(movie => {
                    document.getElementById('movieId').value = movie.id;
                    document.getElementById('title').value = movie.title;
                    document.getElementById('genre').value = movie.genre;
                    document.getElementById('year').value = movie.year;
                    document.getElementById('rating').value = movie.rating;
                    document.getElementById('modalTitle').textContent = 'Edit Movie';
                    movieModal.show();
                })
                .catch(error => alert('Error loading movie details: ' + error));
        }

        function deleteMovie(id) {
            if (confirm('Are you sure you want to delete this movie?')) {
                fetch(`${API_URL}/${id}`, {
                        method: 'DELETE'
                    })
                    .then(response => response.json())
                    .then(data => {
                        alert('Movie deleted successfully');
                        loadMovies();
                    })
                    .catch(error => alert('Error deleting movie: ' + error));
            }
        }

        function saveMovie() {
            const movieId = document.getElementById('movieId').value;
            const movieData = {
                title: document.getElementById('title').value,
                genre: document.getElementById('genre').value,
                year: document.getElementById('year').value,
                rating: document.getElementById('rating').value
            };

            const method = movieId ? 'PUT' : 'POST';
            const url = movieId ? `${API_URL}/${movieId}` : API_URL;

            fetch(url, {
                    method: method,
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify(movieData)
                })
                .then(response => response.json())
                .then(data => {
                    alert(movieId ? 'Movie updated successfully' : 'Movie added successfully');
                    movieModal.hide();
                    loadMovies();
                    resetForm();
                })
                .catch(error => alert('Error saving movie: ' + error));
        }

        function resetForm() {
            document.getElementById('movieId').value = '';
            document.getElementById('movieForm').reset();
            document.getElementById('modalTitle').textContent = 'Tambah Movie';
        }

        document.getElementById('movieModal').addEventListener('hidden.bs.modal', resetForm);
    </script>
</body>

</html>
```
