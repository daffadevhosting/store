+62 STORE — AI-Powered Streetwear Distro


International Streetwear Distro dengan AI Assistant berbasis Llama 3.3 70B + ElevenLabs TTS v2 Multi-language.

---

📱 Tentang Proyek

+62 STORE adalah platform e-commerce distro streetwear modern yang dilengkapi dengan AI Assistant canggih. Dibangun di atas Cloudflare Workers dan menggunakan Llama 3.3 70B untuk pemrosesan bahasa alami serta ElevenLabs untuk Text-to-Speech multi-language.

✨ Fitur Utama

Fitur Deskripsi

🤖 AI Assistant Chatbot pintar berbasis Llama 3.3 70B dengan konteks toko

🎤 Voice Mode Input suara & output audio natural menggunakan ElevenLabs

🛒 Smart Cart Tambah produk ke keranjang via chat atau antarmuka visual

📸 Product Preview Tampilkan gambar produk sesuai warna yang dipilih

📍 Store Locator Informasi alamat & link Google Maps toko offline

💬 WhatsApp Checkout Checkout langsung via WhatsApp dengan format otomatis

🎨 Real-time Catalog Data produk diambil real-time dari JSON statis

---

🏗️ Arsitektur

```
┌─────────────────────────────────────────────────────────────┐
│                         Client (HTML)                       │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────┐  │
│  │  Product UI  │  │  AI Chat UI  │  │  Cart Management │  │
│  └──────┬───────┘  └──────┬───────┘  └────────┬─────────┘  │
└─────────┼──────────────────┼───────────────────┼────────────┘
          │                  │                   │
          ▼                  ▼                   ▼
┌─────────────────────────────────────────────────────────────┐
│                   Cloudflare Worker AI                       │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  Llama 3.3 70B (Cloudflare AI)                     │   │
│  │  + System Prompt + Chat History                    │   │
│  └──────────────────┬───────────────────────────────────┘   │
│                     ▼                                       │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  ElevenLabs TTS (eleven_multilingual_v2)           │   │
│  │  + Preprocessing Text                              │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
          │
          ▼
┌─────────────────────────────────────────────────────────────┐
│                  Data Sources                               │
│  ┌────────────────────┐  ┌─────────────────────────────┐  │
│  │ data_products.json │  │  Store Knowledge Base       │  │
│  │ (Statis di Pages)  │  │  (WA, Alamat, Maps)        │  │
│  └────────────────────┘  └─────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

---

🚀 Tech Stack

Frontend

- Ionic Framework — Komponen UI mobile-first
- Tailwind CSS — Styling cepat & responsif
- IonIcons — Icon set modern
- Web Speech API — Speech-to-Text fallback
- Vanilla JavaScript — Tanpa framework tambahan

Backend

- Cloudflare Workers — Serverless edge computing
- Llama 3.3 70B — Model AI (via Cloudflare AI)
- ElevenLabs API — Text-to-Speech v2 Multi-language
- Cloudflare Pages — Hosting static assets

Integrasi

- WhatsApp API — Checkout via chat
- Google Maps — Lokasi toko offline

---

📁 Struktur File

```
.
├── ai_distro.js              # Cloudflare Worker (Backend AI)
├── distro.html               # Frontend Single Page Application
└── README.md                 # Dokumentasi ini
```

---

🔧 Environment Variables

Cloudflare Worker

```bash
ELEVENLABS_API_KEY = your_elevenlabs_api_key
ELEVENLABS_VOICE_ID = icSyT3QSBd5FSMNddi1m
```

Cloudflare Pages

```bash
# Tidak ada environment variable khusus
# Worker URL di-hardcode di client
CF_WORKER_AI_URL = https://plus62ai.warpzone.workers.dev/api/chat
```

---

🎯 Fitur Detail

1. AI Assistant

System Prompt berisi:

- Knowledge base toko (WA, alamat, maps)
- Katalog produk real-time
- Aturan function calling
- Konteks keranjang belanja

Contoh Interaksi:

```
User: "Tampilkan Gavin the Tiger"
AI: "Mau warna apa bro? Tersedia: Hitam, Putih, Merah"

User: "Hitam"
AI: [Menampilkan gambar Gavin the Tiger warna Hitam]
```

2. Voice Mode

- Input: Web Speech API (id-ID)
- Output: ElevenLabs TTS dengan preprocessing:
  - +62 → "plus enam dua"
  - Nomor telepon → per-digit (pelan & jelas)
  - XL → "eks el"
  - WA → "W A"

3. Smart Cart

Add to Cart via Chat:

```
User: "Masukin Gavin the Tiger size L warna Hitam"
AI: "Sudah ditambahkan bro!" + Action ADD_TO_CART
```

Checkout via WhatsApp:

- Otomatis format pesan dengan detail pesanan
- Data pembeli (nama, WA, alamat)
- Rincian produk & total estimasi

4. Product Gallery

- Filter berdasarkan style/warna
- Search produk
- Detail produk (gambar, harga, stok, ukuran)
- Pilihan variasi warna dengan preview gambar

---

🚀 Deployment

1. Cloudflare Worker

```bash
# Deploy worker
wrangler deploy ai_distro.js --name plus62ai

# Set environment variables
wrangler secret put ELEVENLABS_API_KEY
wrangler secret put ELEVENLABS_VOICE_ID
```

2. Frontend

```bash
# Upload ke Cloudflare Pages atau hosting static
# Update CF_WORKER_AI_URL di distro.html jika berbeda
```

3. Data Products

Upload data_products.json ke:

```
https://j-static-webshop.pages.dev/data_products.json
```

Format JSON:

```json
{
  "product": [
    {
      "slug": "gavin-the-tiger",
      "title": "Gavin the Tiger",
      "price": 16000,
      "stock": "Ready",
      "image": "/images/gavin-tiger.jpg",
      "styles": [
        { "name": "Hitam", "color": "#000000", "image": "/images/gavin-tiger-black.jpg" },
        { "name": "Putih", "color": "#FFFFFF", "image": "/images/gavin-tiger-white.jpg" }
      ]
    }
  ]
}
```

---

🧪 Testing

Manual Test Cases

- Test Case Expected Result
- Chat tanpa voice AI merespon dengan teks
- Chat dengan voice AI merespon dengan suara
- Tanya WA Admin Dibaca per-digit pelan
- Tampilkan gambar produk AI tanya warna jika belum disebut
- Tampilkan gambar dengan warna spesifik Langsung tampilkan gambar
- Warna tidak tersedia Respon sopan dengan daftar warna tersedia
- Tambah ke keranjang Cart badge update & toast notifikasi
Buka keranjang Modal keranjang muncul
Checkout WA Format pesan sesuai & buka WhatsApp

---

🐛 Known Issues & Troubleshooting

1. ElevenLabs TTS tidak bersuara

- Cek environment variables di Cloudflare Worker
- Pastikan API key masih aktif
- Cek console worker untuk error log

2. Voice Mode tidak jalan

- Pastikan browser support Web Speech API
- Ijinkan akses mikrofon
- Coba di Chrome/Edge (Safari limited support)

3. Gambar produk tidak tampil

- Cek URL data_products.json bisa diakses
- Pastikan path gambar benar (relatif/absolut)
- Cek CORS headers di response

4. AI tidak merespon

- Cek koneksi internet
- Cek Cloudflare Worker status
- Cek console untuk error detail

---

📈 Performance Optimization

Worker

- Caching: Gunakan ctx.waitUntil() untuk async tasks
- Token Optimization: Kirim maksimal 10 history terakhir
- Edge Computing: Response time < 500ms (tanpa TTS)

Client

- Lazy Loading: Gambar produk dengan loading="lazy"
- Virtual Scroll: Chat auto-scroll dengan scrollIntoView
- Image Optimization: WebP support jika tersedia

---

🔒 Security

- CORS headers terbatas untuk origin yang diizinkan
- API key disimpan di environment variables (tidak terekspos)
- Input sanitasi di worker sebelum diproses AI
- Rate limiting di Worker (opsional)

---

🚧 Roadmap

☐ Multi-language support (Inggris, Sunda)

☐ Payment gateway integration

☐ Order tracking system

☐ User authentication

☐ Admin dashboard

☐ Mobile app (Ionic React Native)

☐ Real-time stock update

☐ Recommendation engine

---

🤝 Kontribusi

1. Fork repository
2. Buat branch fitur (git checkout -b feature/AmazingFeature)
3. Commit perubahan (git commit -m 'Add some AmazingFeature')
4. Push ke branch (git push origin feature/AmazingFeature)
5. Open Pull Request

---

📝 License

Distributed under MIT License. See LICENSE for more information.

---

📞 Contact

+62 STORE

- 📍 Alamat: Tasikmalaya Regency, West Java
- 📱 WA: 0811-1111-1111
- 🗺️ Google Maps

---

🙏 Credits

- AI Model: Meta Llama 3.3 70B via Cloudflare AI
- TTS: ElevenLabs (eleven_multilingual_v2)
- Framework: Ionic & Tailwind CSS
- Hosting: Cloudflare Workers & Pages

---

<div align="center">
  <sub>Built with ❤️ by +62 STORE Team</sub>
</div>