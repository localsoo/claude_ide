# 🔧 Cara Memperbaiki Error API Key

## Error yang Anda Alami

```
Error talking to connector
api_key: 'YOUR_TMDB_API_KEY'
```

Error ini terjadi karena API key masih menggunakan **placeholder** `YOUR_TMDB_API_KEY` bukan API key asli Anda.

## ✅ Solusi: Setup API Key dengan Benar

### Langkah 1: Dapatkan API Key Asli

1. Buka https://www.themoviedb.org/settings/api
2. Login ke akun TMDb Anda
3. Jika belum punya API key, klik **Request an API Key** → **Developer**
4. Isi form aplikasi:
   - **Application Name**: TMDb GPT Assistant (atau nama apapun)
   - **Application URL**: http://localhost
   - **Application Summary**: Personal use untuk Custom GPT
5. Setujui terms dan submit
6. **COPY** API key Anda (format: `1234567890abcdef1234567890abcdef`)

API key akan terlihat seperti ini:
```
eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIxMjM0NTY3ODkwYWJjZGVmMTIzNDU2Nzg5MGFiY2RlZiIsInN1YiI6IjVlMzJhMWIzZjkzZTBhMDAxOGY3YjU5MCIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.1234567890abcdef
```

**ATAU** jika API key v3, akan lebih pendek:
```
1234567890abcdef1234567890abcdef
```

### Langkah 2: Masukkan API Key ke ChatGPT

**PENTING:** Jangan edit file `tmdb-openapi.yaml`! API key dimasukkan di ChatGPT, bukan di file YAML.

1. Buka Custom GPT Anda di ChatGPT
2. Klik **Edit GPT** (ikon pensil)
3. Tab **Configure**
4. Scroll ke bagian **Actions**
5. Klik **Authentication**
6. Pilih **API Key**

#### Konfigurasi Authentication:

- **Auth Type**: `API Key`
- **API Key**: Pilih **Custom**
- **Custom Header Name**: `api_key`
- **Auth Type**: Query Param
- **Param Name**: `api_key`
- **Value**: PASTE API KEY ASLI ANDA (bukan YOUR_TMDB_API_KEY!)

Contoh:
```
Parameter Name: api_key
Value: 1234567890abcdef1234567890abcdef
```

7. Klik **Save**

### Langkah 3: Test API

1. Kembali ke Custom GPT
2. Coba query sederhana:
   ```
   Carikan film Inception
   ```

3. Jika berhasil, GPT akan menampilkan detail film
4. Jika masih error 401, cek:
   - API key sudah approved (cek email dari TMDb)
   - Copy API key dengan benar (tidak ada spasi)
   - Gunakan API key v3 (bukan v4 access token)

## 🎯 Cara Menggunakan Setelah Setup

### 1. Film Trending
```
Apa film yang sedang trending minggu ini?
```

GPT akan call endpoint: `GET /trending/movie/week`

### 2. Top Rated Movies
```
Tampilkan 10 film terbaik sepanjang masa
```

GPT akan call endpoint: `GET /movie/top_rated`

### 3. Popular Movies
```
Apa film populer saat ini?
```

GPT akan call endpoint: `GET /movie/popular`

### 4. Search
```
Carikan film Marvel
```

GPT akan call endpoint: `GET /search/movie?query=Marvel`

### 5. Recommendations
```
Rekomendasikan film serupa dengan Inception
```

GPT akan:
1. Search "Inception" → movie_id: 27205
2. Call `GET /movie/27205/recommendations`

### 6. Discovery dengan Filter
```
Carikan film sci-fi dengan rating di atas 8.0
```

GPT akan:
1. Get genre ID untuk sci-fi → 878
2. Call `GET /discover/movie?with_genres=878&vote_average.gte=8.0`

## ❌ Yang TIDAK Boleh Dilakukan

1. **JANGAN** edit file `tmdb-openapi.yaml` untuk mengganti `your_api_key_here` dengan API key asli
   - API key harus di-setup di ChatGPT Authentication, bukan di file YAML
   - File YAML hanya template, API key diinjeksi otomatis oleh ChatGPT

2. **JANGAN** commit API key ke git repository
   - API key adalah credential rahasia
   - Jangan share ke publik

3. **JANGAN** gunakan API key orang lain
   - Setiap orang harus punya API key sendiri
   - Free tier: 1000 requests/day

## 🔒 Keamanan API Key

TMDb API Key adalah **READ-ONLY** untuk data publik. Tidak bisa:
- ❌ Menghapus data
- ❌ Mengubah data
- ❌ Akses data private
- ✅ Hanya membaca data film publik (aman)

Tapi tetap jangan share karena:
- Orang lain bisa pakai quota Anda (1000 requests/day)
- Rate limit 40 requests/10 seconds bisa terpakai

## 📊 Endpoint yang Tersedia

Setelah API key setup dengan benar:

| Endpoint | Fungsi | Contoh Query |
|----------|--------|--------------|
| `searchMovie` | Cari film | "Carikan film Inception" |
| `getMovieDetails` | Detail film | "Detail lengkap Avengers" |
| `getMovieRecommendations` | Rekomendasi | "Film serupa dengan Interstellar" |
| `getTrendingMovies` | Film trending | "Film trending minggu ini?" |
| `getTopRatedMovies` | Top rated | "Film terbaik sepanjang masa" |
| `getPopularMovies` | Populer | "Film populer saat ini?" |
| `discoverMovies` | Discovery | "Film aksi rating 8+" |
| `getMovieGenres` | List genre | GPT call otomatis saat perlu |

## 💡 Tips

1. **Bahasa Indonesia**: Default language=id-ID, hasil dalam bahasa Indonesia
2. **Bahasa Inggris**: Bilang ke GPT "use English" untuk language=en-US
3. **Poster**: GPT akan otomatis tampilkan link poster film
4. **Rating**: Format X.X/10 dengan jumlah votes
5. **Pagination**: Hasil default 20 film, bilang "tampilkan lebih banyak" untuk page berikutnya

## 🆘 Masih Error?

### Error 401 Unauthorized
- ✅ Pastikan API key sudah approved (cek email)
- ✅ Copy API key dengan benar (tidak ada spasi)
- ✅ Gunakan API key v3 (read access token), bukan v4

### Error 404 Not Found
- Film tidak ditemukan di database
- Coba judul original (English)

### Error 429 Too Many Requests
- Rate limit exceeded: 40 requests/10 seconds
- Tunggu beberapa detik, coba lagi

### GPT Tidak Memanggil Action
- ✅ Pastikan OpenAPI schema valid (test di editor)
- ✅ Check authentication setup dengan benar
- ✅ Instructions GPT jelas kapan harus call action

## 📚 Resources

- [TMDb API Docs](https://developer.themoviedb.org/docs)
- [Get API Key](https://www.themoviedb.org/settings/api)
- [API Status](https://status.themoviedb.org/)

---

**Butuh bantuan lebih lanjut?**
Baca file `README.md` untuk dokumentasi lengkap atau `QUICK_START.md` untuk panduan setup 5 menit.
