# Panduan Deploy ke Railway

## Langkah 1 — Upload ke GitHub

```bash
cd meridian-main
git init
git add .
git commit -m "initial commit"
git branch -M main
git remote add origin https://github.com/USERNAME/meridian.git
git push -u origin main
```

---

## Langkah 2 — Deploy di Railway

1. Buka [railway.app](https://railway.app) → Login
2. Klik **New Project** → **Deploy from GitHub repo**
3. Pilih repo `meridian` yang sudah diupload
4. Railway akan otomatis detect konfigurasi dari `railway.json`

---

## Langkah 3 — Set Environment Variables

Di Railway dashboard → tab **Variables** → tambahkan satu per satu:

| Variable | Nilai | Wajib? |
|---|---|---|
| `WALLET_PRIVATE_KEY` | Private key Solana kamu (format base58) | ✅ |
| `RPC_URL` | `https://mainnet.helius-rpc.com/?api-key=KAMU` | ✅ |
| `OPENROUTER_API_KEY` | API key dari openrouter.ai | ✅ |
| `HELIUS_API_KEY` | API key dari helius.xyz | ✅ |
| `LPAGENT_API_KEY` | API key dari LPAgent | ⬜ |
| `TELEGRAM_BOT_TOKEN` | Token dari @BotFather | ⬜ |
| `TELEGRAM_CHAT_ID` | Chat ID Telegram kamu | ⬜ |
| `TELEGRAM_ALLOWED_USER_IDS` | User ID Telegram kamu | ⬜ |
| `DRY_RUN` | `true` untuk testing, `false` untuk live | ✅ |
| `LOG_LEVEL` | `info` | ⬜ |

> ⚠️ **JANGAN** taruh private key atau API key di dalam file `user-config.json` atau file lain yang dicommit ke GitHub!

---

## Langkah 4 — Tambah Volume (Persistent Disk)

Agar data posisi dan lessons tidak hilang saat redeploy:

1. Di Railway project → klik **+ Add Service** → **Volume**
2. Mount path: `/app`
3. Hubungkan volume ke service Meridian

> Tanpa volume, file `state.json`, `lessons.json`, dll akan hilang setiap kali Railway restart.

---

## Langkah 5 — Deploy & Monitor

- Railway otomatis deploy setelah environment variables di-set
- Pantau logs di tab **Deployments → View Logs**
- Pastikan tidak ada error sebelum ubah `DRY_RUN=false`

---

## Cara Update Kode

```bash
git add .
git commit -m "update"
git push
```

Railway akan otomatis redeploy setiap kali kamu push ke GitHub.

---

## Troubleshooting

| Problem | Solusi |
|---|---|
| Build gagal | Pastikan `nixpacks.toml` ada di root repo |
| App crash saat start | Cek environment variables sudah lengkap |
| Posisi hilang setelah restart | Tambahkan Railway Volume |
| `DRY_RUN` tidak berubah | Update di Railway Variables, bukan di file |
