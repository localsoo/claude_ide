# TMDb API untuk Custom GPT 🎬

OpenAPI specification untuk mengintegrasikan TMDb (The Movie Database) dengan Custom GPT di ChatGPT.

## 📋 Fitur

- ✅ **Pencarian Film** - Cari film berdasarkan judul/kata kunci
- ✅ **Detail Film** - Dapatkan informasi lengkap film (rating, sinopsis, dll)
- ✅ **Rekomendasi** - Saran film serupa berdasarkan film tertentu
- ✅ **Trending Movies** - Film yang sedang trending harian/mingguan
- ✅ **Top Rated** - Film dengan rating tertinggi sepanjang masa
- ✅ **Popular Movies** - Film populer saat ini
- ✅ **Discover** - Jelajahi film dengan filter genre, tahun, rating
- ✅ **Genre List** - Daftar lengkap genre film
- ✅ **Multi-bahasa** - Support Bahasa Indonesia dan English

## 🚀 Cara Setup Custom GPT

### 1. Dapatkan API Key TMDb

1. Buka [https://www.themoviedb.org/](https://www.themoviedb.org/)
2. Daftar/Login ke akun TMDb
3. Pergi ke **Settings** → **API**
4. Klik **Request an API Key**
5. Pilih **Developer**
6. Isi form aplikasi:
   - **Application Name**: Isi nama aplikasi Anda
   - **Application URL**: Bisa isi `http://localhost` untuk testing
   - **Application Summary**: Deskripsi singkat penggunaan
7. Setujui terms dan submit
8. **Copy API Key** Anda (format: `1234567890abcdef1234567890abcdef`)

### 2. Setup Custom GPT di ChatGPT

1. Buka [ChatGPT](https://chat.openai.com/)
2. Klik **Explore GPTs** (sidebar kiri)
3. Klik **Create** di pojok kanan atas
4. Klik tab **Configure**

#### Konfigurasi GPT:

**Name:**
```
TMDb Movie Assistant
```

**Description:**
```
Asisten pencarian dan rekomendasi film menggunakan database TMDb. Cari film, lihat detail, dan dapatkan rekomendasi film serupa.
```

**Instructions:**
```
Anda adalah asisten film yang membantu pengguna mencari informasi film menggunakan TMDb API.

Capabilities:
- Cari film berdasarkan judul atau kata kunci
- Tampilkan detail lengkap film (judul, sinopsis, rating, tahun rilis, genre, budget, revenue)
- Berikan rekomendasi film serupa
- Tampilkan film trending, top rated, dan popular
- Bantu pengguna menemukan film berdasarkan genre, tahun, atau rating tertentu
- Tampilkan poster film sebagai IMAGE langsung di chat

Guidelines:
- Selalu gunakan bahasa Indonesia (language=id-ID) kecuali user minta bahasa lain
- Format rating dengan 1 desimal (contoh: 8.4/10)
- Sertakan emoji yang relevan untuk membuat response lebih menarik
- Jika mencari film, tampilkan top 5-10 hasil terbaik
- Untuk rekomendasi, jelaskan mengapa film tersebut direkomendasikan
- Format tanggal rilis dengan format readable (contoh: 16 Juli 2010)
- Tampilkan budget dan revenue dalam format yang mudah dibaca (contoh: $160 juta)

PENTING - Cara Menampilkan Poster:
- SELALU tampilkan poster sebagai IMAGE menggunakan markdown image syntax
- Format: ![Judul Film](https://image.tmdb.org/t/p/w500{poster_path})
- Jika poster_path ada, WAJIB tampilkan sebagai image, BUKAN hanya link
- Gunakan w500 untuk ukuran poster yang optimal

Contoh Response dengan Poster:
"🎬 **Inception** (2010)

![Poster Inception](https://image.tmdb.org/t/p/w500/9gk7adHYeDvHkCSEqAvQNLV5Uge.jpg)

⭐ Rating: 8.4/10 dari 32,000 votes
📝 Sinopsis: Dom Cobb adalah pencuri terampil dalam seni extraction, mencuri rahasia dari alam bawah sadar seseorang saat mereka bermimpi...
🎭 Genre: Action, Science Fiction, Thriller
⏱️ Durasi: 148 menit
📅 Rilis: 16 Juli 2010
💰 Budget: $160 juta | Revenue: $829 juta

Mau rekomendasi film serupa? 🎥"
```

**Conversation starters:**
```
Apa film yang sedang trending minggu ini?
Tampilkan 10 film terbaik sepanjang masa
Carikan film aksi terbaik tahun 2023
Rekomendasikan film serupa dengan Inception
```

### 3. Import OpenAPI Schema

1. Scroll ke bagian **Actions**
2. Klik **Create new action**
3. Klik **Import from URL** atau **Import**
4. Pilih file `tmdb-openapi.yaml` atau paste URL jika sudah di-host
5. Atau **copy-paste** isi file `tmdb-openapi.yaml` ke editor

### 4. Setup Authentication

1. Di bagian **Authentication**, pilih **API Key**
2. Pilih **Custom** header
3. **Auth Type**: Query Parameter
4. **Parameter Name**: `api_key`
5. **API Key**: Paste API key TMDb Anda dari langkah 1

### 5. Test & Publish

1. Klik **Test** untuk test action
2. Coba search film: "Inception"
3. Jika berhasil, klik **Save** di pojok kanan atas
4. Pilih **Only me** (private) atau **Public** sesuai kebutuhan

## 💡 Contoh Penggunaan

### Mencari Film

**User:**
```
Carikan film tentang superhero Marvel
```

**GPT akan:**
- Memanggil `searchMovie` dengan query "Marvel"
- Menampilkan hasil dengan detail (judul, rating, tahun, sinopsis)
- Menampilkan poster film

### Melihat Detail Film

**User:**
```
Tunjukkan detail lengkap film Avengers: Endgame
```

**GPT akan:**
- Search film terlebih dahulu untuk dapatkan movie_id
- Memanggil `getMovieDetails` dengan movie_id tersebut
- Menampilkan detail lengkap (genre, budget, revenue, runtime, dll)

### Mendapat Rekomendasi

**User:**
```
Rekomendasikan film serupa dengan Inception
```

**GPT akan:**
- Search "Inception" untuk dapatkan movie_id
- Memanggil `getMovieRecommendations` dengan movie_id
- Menampilkan 5-10 rekomendasi film serupa

### Filter & Discovery

**User:**
```
Carikan film sci-fi dengan rating di atas 8.0
```

**GPT akan:**
- Memanggil `getMovieGenres` untuk dapatkan genre ID sci-fi
- Memanggil `discoverMovies` dengan filter genre dan rating
- Menampilkan hasil yang sesuai kriteria

### Film Trending

**User:**
```
Apa film yang sedang trending minggu ini?
```

**GPT akan:**
- Memanggil `getTrendingMovies` dengan time_window="week"
- Menampilkan 10-20 film yang sedang trending
- Menjelaskan mengapa film tersebut trending

### Top Rated Movies

**User:**
```
Tampilkan 10 film terbaik sepanjang masa
```

**GPT akan:**
- Memanggil `getTopRatedMovies`
- Menampilkan film dengan rating tertinggi
- Termasuk info rating dan jumlah votes

## 📊 Endpoint Reference

### 1. Search Movie
```
GET /search/movie?api_key={key}&query={keyword}&language=id-ID
```

**Use Case:** Cari film berdasarkan judul/kata kunci

**Parameters:**
- `query` (required): Kata kunci pencarian
- `language`: Bahasa hasil (default: id-ID)
- `year`: Filter tahun rilis
- `page`: Halaman hasil (pagination)

**Response:** Array film dengan title, overview, rating, poster_path

---

### 2. Get Movie Details
```
GET /movie/{movie_id}?api_key={key}&language=id-ID
```

**Use Case:** Detail lengkap film tertentu

**Parameters:**
- `movie_id` (required): ID film dari TMDb
- `language`: Bahasa hasil

**Response:** Detail lengkap (genres, budget, revenue, runtime, production companies, dll)

---

### 3. Get Recommendations
```
GET /movie/{movie_id}/recommendations?api_key={key}&language=id-ID
```

**Use Case:** Rekomendasi film serupa

**Parameters:**
- `movie_id` (required): ID film sumber
- `language`: Bahasa hasil
- `page`: Halaman hasil

**Response:** Array film rekomendasi

---

### 4. Discover Movies
```
GET /discover/movie?api_key={key}&with_genres={genre_ids}&sort_by=popularity.desc
```

**Use Case:** Cari film dengan filter advanced

**Parameters:**
- `with_genres`: Filter genre (pisahkan dengan koma)
- `sort_by`: Sorting (popularity.desc, vote_average.desc, dll)
- `primary_release_year`: Filter tahun
- `vote_average.gte`: Rating minimum
- `language`: Bahasa hasil

**Response:** Array film yang match dengan filter

---

### 5. Get Genre List
```
GET /genre/movie/list?api_key={key}&language=id-ID
```

**Use Case:** Daftar semua genre film

**Response:** Array genre dengan id dan name

---

### 6. Get Trending Movies
```
GET /trending/movie/{time_window}?api_key={key}&language=id-ID
```

**Use Case:** Film yang sedang trending/populer saat ini

**Parameters:**
- `time_window` (required): `day` atau `week`
- `language`: Bahasa hasil
- `page`: Nomor halaman

**Response:** Array film trending dengan popularity score

---

### 7. Get Top Rated Movies
```
GET /movie/top_rated?api_key={key}&language=id-ID
```

**Use Case:** Film dengan rating tertinggi sepanjang masa

**Parameters:**
- `language`: Bahasa hasil
- `page`: Nomor halaman
- `region`: Filter berdasarkan region (opsional)

**Response:** Array film dengan rating tertinggi (8.0+)

---

### 8. Get Popular Movies
```
GET /movie/popular?api_key={key}&language=id-ID
```

**Use Case:** Film populer saat ini

**Parameters:**
- `language`: Bahasa hasil
- `page`: Nomor halaman
- `region`: Filter berdasarkan region (opsional)

**Response:** Array film populer berdasarkan views dan interaksi

## 🖼️ Menampilkan Poster Sebagai Image di ChatGPT

### PENTING: Tampilkan Poster Langsung, Bukan Hanya Link!

Custom GPT Anda **HARUS** menampilkan poster sebagai **IMAGE langsung** di chat menggunakan markdown image syntax.

### ✅ Yang Benar - Tampilkan Image Langsung

GPT Instructions sudah dikonfigurasi untuk menampilkan poster seperti ini:

```markdown
🎬 **Inception** (2010)

![Poster Inception](https://image.tmdb.org/t/p/w500/9gk7adHYeDvHkCSEqAvQNLV5Uge.jpg)

⭐ Rating: 8.4/10
```

**Hasil:** Poster tampil langsung sebagai gambar di ChatGPT ✅

### ❌ Yang Salah - Hanya Link

```markdown
🎬 **Inception** (2010)
Poster: https://image.tmdb.org/t/p/w500/9gk7adHYeDvHkCSEqAvQNLV5Uge.jpg
```

**Hasil:** User harus klik link untuk lihat poster ❌

### Format URL Poster

TMDb menyediakan image dengan berbagai ukuran:

```
https://image.tmdb.org/t/p/{size}{poster_path}
```

**Ukuran Poster:**
- `w92` - Thumbnail kecil (92px)
- `w154` - Thumbnail sedang (154px)
- `w185` - Preview kecil (185px)
- `w342` - Preview sedang (342px)
- **`w500`** - **RECOMMENDED untuk ChatGPT** ✅ (500px)
- `w780` - Desktop besar (780px)
- `original` - Ukuran asli (file besar, loading lambat)

**Ukuran Backdrop:**
- `w300`, `w780`, `w1280`, `original`

### Contoh Markdown Image Syntax

```markdown
![Poster Inception](https://image.tmdb.org/t/p/w500/9gk7adHYeDvHkCSEqAvQNLV5Uge.jpg)
```

**Breakdown:**
- `!` - Prefix untuk image (WAJIB!)
- `[Poster Inception]` - Alt text
- `(URL)` - URL lengkap poster

### Untuk Panduan Lengkap

Lihat **DISPLAY_POSTER_GUIDE.md** untuk:
- Template response dengan poster image
- Format untuk trending, top rated, recommendations
- Troubleshooting jika poster tidak muncul
- Best practices design

## 🎯 Tips & Best Practices

### 1. Gunakan Language Parameter
```yaml
language: "id-ID"  # Bahasa Indonesia
language: "en-US"  # English
```

### 2. Handling Pagination
- TMDb mengembalikan 20 hasil per halaman
- Gunakan parameter `page` untuk navigasi
- Check `total_pages` untuk jumlah halaman

### 3. Filter Genre
Daftar Genre ID populer:
- 28: Aksi
- 12: Petualangan
- 16: Animasi
- 35: Komedi
- 80: Kriminal
- 18: Drama
- 14: Fantasi
- 27: Horror
- 10749: Romance
- 878: Science Fiction

### 4. Optimasi Query
- Gunakan `year` parameter untuk hasil lebih akurat
- Gunakan `vote_average.gte` untuk filter film berkualitas
- Gunakan `sort_by=vote_average.desc` untuk film rating tertinggi

### 5. Error Handling
- **401 Unauthorized**: API key salah/tidak valid
- **404 Not Found**: Film tidak ditemukan
- **429 Too Many Requests**: Rate limit exceeded (40 requests/10 seconds)

## 🔒 Keamanan & Rate Limits

### Rate Limits TMDb
- **40 requests** per 10 seconds
- **1000 requests** per day (free tier)

### Best Practices:
- Jangan share API key publik
- Gunakan environment variable untuk API key
- Implementasi caching untuk request yang sama
- Handle error 429 dengan retry logic

## 🛠️ Troubleshooting

### API Key tidak bekerja
✅ Pastikan API key sudah approved (cek email konfirmasi)
✅ Copy API key dengan benar (tidak ada spasi)
✅ Gunakan API key v3 (bukan v4 access token)

### Hasil pencarian kosong
✅ Cek spelling kata kunci
✅ Coba tanpa filter tahun terlebih dahulu
✅ Gunakan judul original (English) untuk hasil lebih akurat

### Image tidak muncul
✅ Pastikan poster_path tidak null
✅ Gunakan format URL lengkap: `https://image.tmdb.org/t/p/w500{poster_path}`
✅ Coba ukuran berbeda jika satu ukuran tidak ada

### GPT tidak memanggil action
✅ Pastikan OpenAPI schema valid
✅ Check authentication setup
✅ Test action di GPT editor sebelum publish
✅ Pastikan instructions menjelaskan kapan harus call action

## 📚 Resources

- [TMDb API Documentation](https://developer.themoviedb.org/docs)
- [TMDb API Reference](https://developer.themoviedb.org/reference/intro/getting-started)
- [OpenAPI 3.1 Specification](https://spec.openapis.org/oas/v3.1.0)
- [Custom GPT Documentation](https://platform.openai.com/docs/actions)

## 🤝 Support

Jika ada pertanyaan atau issue:
1. Cek TMDb API status: [https://status.themoviedb.org/](https://status.themoviedb.org/)
2. Baca dokumentasi TMDb: [https://developer.themoviedb.org/docs](https://developer.themoviedb.org/docs)
3. Test API dengan Postman/curl terlebih dahulu

## 📝 Changelog

**v1.2.0** (2025-11-14)
- ✅ **POSTER DISPLAY**: GPT instructions untuk menampilkan poster sebagai IMAGE
- ✅ Tambah DISPLAY_POSTER_GUIDE.md - panduan lengkap display poster
- ✅ Update GPT instructions dengan markdown image syntax
- ✅ Contoh response dengan poster image yang benar
- ✅ Best practices untuk multiple movies display

**v1.1.0** (2025-11-14)
- ✅ Trending movies endpoint (harian/mingguan)
- ✅ Top rated movies endpoint
- ✅ Popular movies endpoint
- ✅ Perbaikan dokumentasi API key authentication
- ✅ Update contoh penggunaan

**v1.0.0** (2025-11-14)
- ✅ Initial release
- ✅ Search movie endpoint
- ✅ Movie details endpoint
- ✅ Recommendations endpoint
- ✅ Discover movies endpoint
- ✅ Genre list endpoint
- ✅ Multi-language support
- ✅ Comprehensive documentation

## 📄 License

OpenAPI specification ini free untuk digunakan. TMDb API memiliki terms of service sendiri yang harus dipatuhi.

**TMDb Terms:**
- Attribution required (tampilkan "Data provided by TMDb")
- Non-commercial use (free tier)
- Respect rate limits

---

**Happy Coding! 🎬🍿**
