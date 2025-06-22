# 📱 WhatsApp Sales Bot AI

Bot WhatsApp canggih berbasis **Google Gemini**, dirancang untuk:

- Mengotomatiskan interaksi pelanggan  
- Menangani penjualan  
- Berpartisipasi dalam percakapan grup secara natural

Bot ini cerdas, kontekstual, dan **mudah dikonfigurasi**.

---

## 🚀 Fitur Utama

- **🤖 AI Kontekstual Ganda**  
  Gunakan prompt berbeda untuk:
  - **Chat pribadi** (riwayat 20 pesan, fokus penjualan)  
  - **Grup** (riwayat 10 pesan, gaya santai)

- **🛍️ Mode Jualan & Percakapan**  
  Deteksi otomatis:
  - Perintah penjualan (`menu`, `pesan A`, dll.)
  - Pertanyaan umum (dijawab AI)

- **📦 Manajemen Produk**  
  Tambah, hapus, dan kelola produk dari file konfigurasi

- **💳 Pemesanan & Pembayaran**  
  Dari permintaan hingga informasi transfer

- **👁️ Validasi Bukti Transfer (AI)** *(opsional)*  
  Validasi otomatis gambar bukti transfer

- **🛡️ Kontrol Akses Fleksibel**  
  - **Whitelist Grup**: aktif hanya di grup tertentu  
  - **Blacklist User**: abaikan nomor tertentu  
  - **Abaikan Kontak Tersimpan**: mencegah interaksi tidak perlu

- **⚙️ Konfigurasi Mudah**  
  Semua pengaturan bot dalam satu objek konfigurasi

- **📝 Logging Detail**  
  Catat semua aktivitas bot (proses dan yang diabaikan)

---

## 📦 Instalasi

Pastikan Anda telah menginstal **Node.js v16+**

```bash
npm install whatsapp-web.js qrcode-terminal node-fetch
```

---

## ⚙️ Cara Penggunaan

1. Salin kode `index.ts` ke proyek Anda  
2. Buat file baru, contoh: `app.js`

```js
const { WhatsAppSalesBot } = require('whatsapp-sales-bot-ai'); // Sesuaikan path Anda

const botConfig = {
  geminiApiKey: "YOUR_GEMINI_API_KEY",
  products: {
    'WD01': {
      kode: 'WD01',
      nama: 'Jasa Pembuatan Website',
      deskripsi: 'Website profesional untuk profil perusahaan atau toko online. Modern, responsif, dan cepat.',
      harga: 'Mulai dari Rp 3.000.000'
    },
    'DM01': {
      kode: 'DM01',
      nama: 'Paket Digital Marketing',
      deskripsi: 'Layanan SEO, Manajemen Media Sosial, dan Iklan Digital lengkap untuk bisnis Anda.',
      harga: 'Rp 2.500.000 / bulan'
    }
  },
  botName: 'Asisten Digital',
  companyDescription: 'sebuah agensi digital kreatif',
  enableAI: true,
  enableLogging: true,
  groupWhitelist: [
    '120363041234567890@g.us'
  ],
  userBlacklist: [
    '6281234567890@c.us'
  ],
  greetingMessage: '👋 Halo! Selamat datang di layanan kami. Ketik *menu* untuk melihat apa saja yang bisa kami bantu.',
  dailyGroupMessageLimit: 20,
  replyTimeout: { min: 1, max: 3 }
};

const bot = new WhatsAppSalesBot(botConfig);

bot.start().catch(err => console.error("Gagal menjalankan bot:", err));
```

---

## 🔧 Konfigurasi Lengkap (`BotConfig`)

| Properti                 | Tipe                          | Deskripsi                                                                 | Wajib?                   |
|--------------------------|-------------------------------|--------------------------------------------------------------------------|--------------------------|
| `geminiApiKey`           | `string`                      | Kunci API Google Gemini                                                  | Ya (jika `enableAI = true`) |
| `products`               | `Record<string, ProductData>` | Katalog produk/layanan                                                   | Ya                       |
| `enableAI`               | `boolean`                     | Aktifkan atau nonaktifkan AI                                             | Tidak (default: `true`) |
| `botName`                | `string`                      | Nama bot saat memperkenalkan diri                                        | Tidak                    |
| `companyDescription`     | `string`                      | Deskripsi singkat perusahaan untuk konteks AI                            | Tidak                    |
| `groupWhitelist`         | `string[]`                    | Daftar ID grup yang diizinkan                                            | Tidak                    |
| `userBlacklist`          | `string[]`                    | Daftar nomor pengguna yang akan diabaikan                                | Tidak                    |
| `rekeningInfo`           | `RekeningData[]`              | Informasi rekening untuk proses pembayaran                               | Tidak                    |
| `greetingMessage`        | `string`                      | Pesan pembuka saat pengguna memicu menu                                  | Tidak                    |
| `fallbackMessage`        | `string`                      | Balasan jika AI nonaktif & tidak ada kata kunci cocok                    | Tidak                    |
| `menuTriggers`           | `string[]`                    | Daftar kata kunci untuk memicu menu (`menu`, `halo`, `start`, dll.)      | Tidak                    |
| `dailyGroupMessageLimit` | `number`                      | Jumlah maksimum pesan AI yang dijawab per grup per hari *(fitur baru)*   | Tidak (default: `20`)    |
| `replyTimeout`           | `{ min: number, max: number }`| Jeda acak (dalam detik) sebelum bot membalas pesan *(simulasi natural)*  | Tidak (default: `{ min: 1, max: 3 }`) |

---

## ⚙️ Cara Kerja Bot

**1. Mode Jualan**  
Jika pesan cocok dengan keyword:

- `menu`, `halo`, `start`: tampilkan produk  
- `WD01`: tampilkan detail produk  
- `pesan WD01`: mulai alur pemesanan  

**2. Mode Percakapan (AI)**  
Jika tidak cocok dengan keyword, serahkan ke AI:

- **Chat pribadi**: riwayat 20 pesan  
- **Grup**: riwayat 10 pesan

---

## Advanced Konfigurasi Bot Untuk Personalisasi Cerdas untuk Respon Kontekstual 

Bot menggunakan dua jenis **prompt template yang dirancang khusus**, untuk memberikan pengalaman komunikasi yang cerdas dan relevan.

---
### DEMO KONFIGURASI
```javascript
const botConfig = {
    enableAI: true,
    geminiApiKey: "APIKEY_GEMINI", // PENTING: Ganti dengan API Key Anda
    groupWhitelist: [
        '120363418699892647@g.us',
        '120363401047245879@g.us'
    ],
    userBlacklist: [
    ],
    products: {
        'PKG001': {
            kode: 'PKG001',
            nama: 'Paket Branding Digital',
            deskripsi: 'Desain logo, identitas visual, dan template media sosial profesional.',
            harga: 'Rp 1.750.000'
        },
        'PKG002': {
            kode: 'PKG002',
            nama: 'Pembuatan Website UMKM',
            deskripsi: 'Website landing page untuk UMKM. Mobile friendly, SEO-ready, termasuk domain & hosting 1 tahun.',
            harga: 'Rp 2.999.000'
        },
        'PKG003': {
            kode: 'PKG003',
            nama: 'Iklan Instagram & Facebook',
            deskripsi: 'Setup iklan FB/IG untuk promosi produk/jasa Anda. Target audience + copywriting termasuk.',
            harga: 'Mulai dari Rp 1.000.000 / kampanye'
        }
    },
    rekeningInfo: [
        { bank: 'BCA', nomor: '01923189132', nama: 'Alam Wibowo' },
        { bank: 'BRI', nomor: '0185-01-02183-509', nama: 'Alam Wibowo' }
    ],
    botName: 'Luna',
    greetingMessage: `👋 Halo kak, Selamat datang di *Arkana Tech Solution*.\n\nSaya bisa bantu Anda menemukan layanan terbaik kami untuk kebutuhan digital bisnis Anda.`,
    fallbackMessage: 'Maaf, saya tidak mengerti. Ketik *menu* untuk melihat semua layanan.',
    enableLogging: true,
    menuTriggers: ['menu', 'halo', 'list', 'bantuan', 'hai'],
    dailyGroupMessageLimit: 5,
    replyTimeout: { min: 1, max: 5 },
    privateChatPromptTemplate: `
                                [PERAN UTAMA]
                                Anda adalah '{botName}', asisten customer service yang profesional, ramah, dan sangat membantu dari perusahaan kami.

                                [TUGAS UTAMA]
                                Tugas Anda adalah menjawab pertanyaan pengguna, memberikan informasi produk, dan membantu proses pemesanan. Fokus utama Anda adalah pada layanan yang ditawarkan perusahaan.

                                [ATURAN PENTING]
                                1.  **FOKUS**: Jawab pertanyaan HANYA berdasarkan konteks yang diberikan. Jangan pernah mengarang informasi.
                                2.  **BATASAN TOPIK**: JANGAN menjawab pertanyaan yang sama sekali tidak berhubungan dengan produk atau perusahaan (misalnya: pertanyaan pribadi, politik, cuaca, topik umum).
                                3.  **STRATEGI PENGALIHAN**: Jika pengguna bertanya di luar topik, TOLAK dengan sopan dan segera ARAHKAN kembali percakapan ke produk/layanan. Contoh: "Maaf, saya hanya bisa membantu terkait produk dan layanan kami. Apakah ada yang bisa saya bantu seputar itu?".
                                4.  **TONE BAHASA**: Gunakan bahasa yang jelas, sopan, dan profesional.

                                ---
                                [INFO PRODUK]
                                {productInfo}
                                ---
                                [KONTEKS PERCAKAPAN SEBELUMNYA]
                                {chatHistory}
                                {replyContext}
                                ---

                                [WAKTU SAAT INI]
                                {currentTime}

                                [PESAN BARU DARI PENGGUNA]
                                "{userMessage}"

                                [JAWABAN ANDA]`,
    groupChatPromptTemplate: `
                                [PERAN UTAMA]
                                Anda adalah '{botName}', seorang partisipan yang berpengetahuan dan ramah di grup WhatsApp ini.
                                
                                [TUGAS UTAMA]
                                Tugas Anda adalah berpartisipasi secara natural. Jika ada pertanyaan yang relevan dengan keahlian Anda (seputar produk dan layanan perusahaan), jawablah dengan informatif. Jaga agar percakapan tetap positif dan relevan.
                                
                                [ATURAN PENTING]
                                1.  **RELEVANSI**: Gunakan [INFO PRODUK] sebagai referensi HANYA JIKA ada anggota grup lain yang bertanya secara spesifik tentang hal itu. JANGAN proaktif menawarkan produk kecuali jika sangat relevan dengan topik diskusi.
                                2.  **BATASAN TOPIK**: Sama sekali JANGAN terlibat dalam percakapan di luar topik keahlian Anda (misalnya: gosip, politik, topik umum yang tidak relevan).
                                3.  **STRATEGI PENGALIHAN DI GRUP**: Jika ada yang bertanya hal di luar topik kepada Anda, jawab dengan singkat dan netral. Contoh: "Untuk hal tersebut saya kurang tahu informasinya." Jangan mengalihkan secara paksa seperti di chat pribadi, cukup berhenti merespons topik itu.
                                4.  **TONE BAHASA**: Gunakan bahasa yang santai, natural, dan sopan, layaknya anggota grup lainnya. Hindari bahasa yang terlalu kaku atau seperti robot.
                                
                                ---
                                [INFO PRODUK UNTUK REFERENSI ANDA]
                                {productInfo}
                                ---
                                [KONTEKS PERCAKAPAN GRUP SEBELUMNYA]
                                {chatHistory}
                                {replyContext}
                                ---
                                
                                [WAKTU SAAT INI]
                                {currentTime}
                                
                                [PESAN BARU UNTUK ANDA RESPONS]
                                "{userMessage}"
                                
                                [JAWABAN ANDA]`,
};

```

### ✉️ `privateChatPromptTemplate` – Mode Chat Pribadi

Bot bertindak sebagai **customer service profesional**.
Prompt ini berisi peran, tugas, batasan, dan struktur konteks:

| Elemen Prompt      | Penjelasan                                                      |
| ------------------ | --------------------------------------------------------------- |
| **PERAN UTAMA**    | Asisten ramah dan profesional dari perusahaan                   |
| **TUGAS UTAMA**    | Menjawab pertanyaan, menjelaskan produk, dan membantu pemesanan |
| **ATURAN PENTING** | 4 aturan penting:                                               |

1. Fokus pada konteks
2. Larangan topik umum/nonproduk
3. Strategi pengalihan jika out-of-topic
4. Gunakan bahasa profesional dan sopan |
   \| **KONTEKS DINAMIS** | Bot menerima:

* `{productInfo}` = daftar produk
* `{chatHistory}` = 20 pesan sebelumnya
* `{replyContext}` = percakapan spesifik jika ada
* `{userMessage}` = pesan terbaru
* `{currentTime}` = waktu saat ini |

> 🎯 **Tujuan**: Menciptakan jawaban cerdas, fokus, dan terhindar dari “halusinasi AI”.

---

### 👥 `groupChatPromptTemplate` – Mode Grup WhatsApp

Bot berperan sebagai **anggota grup** yang informatif, tidak mengganggu, dan hanya merespons jika topik relevan.

| Elemen Prompt      | Penjelasan                                                                 |
| ------------------ | -------------------------------------------------------------------------- |
| **PERAN UTAMA**    | Partisipan santai, berpengetahuan, dan tidak menonjol                      |
| **TUGAS UTAMA**    | Menjawab pertanyaan seputar produk *jika ditanya*, tidak menawarkan duluan |
| **ATURAN PENTING** | 4 aturan penting:                                                          |

1. Jawab hanya jika relevan
2. Abaikan gosip/topik umum
3. Hindari mengarahkan seperti chat pribadi
4. Gunakan gaya bahasa santai dan manusiawi |
   \| **KONTEKS DINAMIS** | Template menyisipkan:

* `{productInfo}`
* `{chatHistory}`
* `{replyContext}`
* `{userMessage}`
* `{currentTime}` |

> 🎯 **Tujuan**: Bot terasa seperti “teman” di grup, bukan spammer atau sales agresif.

---

## 🧩 Daftar Placeholder Template

| Placeholder      | Keterangan                                      |
| ---------------- | ----------------------------------------------- |
| `{botName}`      | Nama bot (dari konfigurasi `botName`)           |
| `{productInfo}`  | Informasi daftar produk dari `products`         |
| `{chatHistory}`  | Riwayat 10–20 pesan sebelumnya                  |
| `{replyContext}` | Pesan tertentu yang sedang dijawab (jika ada)   |
| `{currentTime}`  | Waktu saat ini                                  |
| `{userMessage}`  | Pesan baru dari pengguna yang memicu respons AI |

---

## 🔒 Keamanan dan Kualitas Respons

Template ini dibuat untuk **mencegah AI menjawab di luar konteks** dan mengarahkan percakapan kembali ke:

* Produk atau layanan
* Pertanyaan relevan
* Proses pemesanan

> 🧠 Ini adalah strategi mitigasi **hallucination AI** yang umum terjadi pada LLM.

---

## 🔌 API (Metode Publik)

| Metode                                     | Deskripsi                                                                 |
|-------------------------------------------|---------------------------------------------------------------------------|
| `bot.start()`                              | Menjalankan bot                                                           |
| `bot.stop()`                               | Menghentikan bot dan koneksi WhatsApp                                     |
| `bot.updateConfig(config: Partial<BotConfig>)` | Update konfigurasi saat runtime                                      |
| `bot.addProduct(kode, data)`               | Tambah produk ke katalog                                                  |
| `bot.removeProduct(kode)`                  | Hapus produk berdasarkan kode                                             |
| `bot.addGroupToWhitelist(groupId)`         | Tambah ID grup ke whitelist                                               |
| `bot.addUserToBlacklist(userId)`           | Tambah user ke daftar blacklist                                           |

---
