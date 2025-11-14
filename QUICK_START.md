# Quick Start Guide - TMDb Custom GPT

Panduan cepat untuk setup TMDb API di Custom GPT dalam 5 menit!

## 📦 File yang Dibutuhkan

- `tmdb-openapi.yaml` - OpenAPI specification untuk GPT Actions
- `README.md` - Dokumentasi lengkap

## ⚠️ PENTING: Setup API Key

**JANGAN** gunakan `YOUR_TMDB_API_KEY` sebagai API key!
Anda harus memasukkan **API key asli** dari TMDb di ChatGPT Actions Authentication.

## ⚡ Setup Cepat (5 Langkah)

### 1️⃣ Dapatkan TMDb API Key

1. Buka https://www.themoviedb.org/settings/api
2. Login/Register
3. Request API Key → Developer
4. Copy API key Anda

### 2️⃣ Buat Custom GPT

1. Buka https://chat.openai.com/
2. Klik **Explore GPTs** → **Create**
3. Tab **Configure**

### 3️⃣ Isi Konfigurasi

**Name:**
```
TMDb Movie Assistant
```

**Description:**
```
Asisten pencarian dan rekomendasi film menggunakan TMDb
```

**Instructions:** (Copy-paste ini)
```
Anda adalah asisten film menggunakan TMDb API. Bantu user mencari film, lihat detail, dan dapatkan rekomendasi.

- Gunakan language=id-ID untuk bahasa Indonesia
- Format rating: 8.4/10
- Tampilkan poster dengan URL: https://image.tmdb.org/t/p/w500{poster_path}
- Berikan 5-10 hasil teratas untuk pencarian
- Jelaskan mengapa merekomendasikan film tertentu

Format response:
🎬 **Judul** (Tahun)
⭐ Rating: X.X/10
📝 Sinopsis: ...
🎭 Genre: ...
```

**Conversation Starters:**
```
Carikan film aksi terbaik tahun 2023
Rekomendasikan film serupa dengan Inception
Cari film Marvel terbaru
Apa film sci-fi dengan rating tinggi?
```

### 4️⃣ Import OpenAPI Schema

1. Scroll ke **Actions** → **Create new action**
2. Copy seluruh isi file `tmdb-openapi.yaml`
3. Paste ke editor Actions
4. Atau upload file `tmdb-openapi.yaml` langsung

### 5️⃣ Setup Authentication

1. Di **Authentication**, pilih **API Key**
2. **Auth Type**: `API Key`
3. **Custom**: `Query Parameter`
4. **Parameter Name**: `api_key`
5. **API Key**: Paste API key TMDb dari step 1

### ✅ Test & Save

1. Klik **Test** → Coba query "Inception"
2. Jika success → **Save**
3. Pilih **Only me** atau **Public**

## 🎯 Test Queries

Setelah setup, coba queries ini:

```
Apa film yang sedang trending minggu ini?
```

```
Tampilkan 10 film terbaik sepanjang masa
```

```
Carikan film Inception
```

```
Rekomendasikan 5 film serupa dengan The Dark Knight
```

```
Cari film action dengan rating di atas 8.0
```

```
Apa film populer saat ini?
```

## 🔧 Troubleshooting Cepat

**Error 401?**
- Cek API key (harus sudah approved via email)
- Pastikan tidak ada spasi di API key

**GPT tidak call action?**
- Check OpenAPI schema valid
- Test action di editor dulu
- Pastikan instructions jelas

**Hasil kosong?**
- Coba judul English untuk hasil lebih baik
- Hapus filter tahun dulu

## 📊 Endpoints yang Tersedia

| Endpoint | Fungsi |
|----------|--------|
| `searchMovie` | Cari film by keyword |
| `getMovieDetails` | Detail lengkap film |
| `getMovieRecommendations` | Film serupa |
| `getTrendingMovies` | Film trending hari ini/minggu ini |
| `getTopRatedMovies` | Film rating tertinggi sepanjang masa |
| `getPopularMovies` | Film populer saat ini |
| `discoverMovies` | Filter genre/rating/tahun |
| `getMovieGenres` | List semua genre |

## 🎨 Menampilkan Poster

Format URL poster:
```
https://image.tmdb.org/t/p/w500{poster_path}
```

Ukuran tersedia: `w92`, `w154`, `w185`, `w342`, `w500`, `w780`, `original`

## 💡 Tips Pro

1. **Multi-language**: Gunakan `language=en-US` untuk English
2. **Pagination**: Tambah `page=2` untuk hasil berikutnya
3. **Filter Rating**: Gunakan `vote_average.gte=8.0` untuk film bagus
4. **Sort**: `sort_by=popularity.desc` untuk film populer

## 📚 Butuh Detail?

Lihat **README.md** untuk:
- Dokumentasi lengkap semua endpoints
- Contoh penggunaan detail
- Best practices
- Error handling
- Security tips

## 🚀 Siap Digunakan!

Custom GPT Anda sekarang bisa:
- ✅ Mencari film dari 1 juta+ database TMDb
- ✅ Menampilkan detail lengkap (rating, sinopsis, budget, revenue)
- ✅ Memberikan rekomendasi cerdas
- ✅ Menampilkan film trending harian/mingguan
- ✅ Menampilkan top rated movies sepanjang masa
- ✅ Menampilkan film populer saat ini
- ✅ Filter berdasarkan genre, tahun, rating
- ✅ Support multi-bahasa

**Selamat mencoba! 🎬🍿**
