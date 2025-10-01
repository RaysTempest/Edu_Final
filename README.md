Edu Adventure - Patch Additions

Apa yang saya tambahkan:

- Fitur "Belajar dengan AI" (modal di header): AI rule-based offline yang bisa memberikan ringkasan, membuat kuis cepat, atau menjawab pertanyaan sederhana.
- Perbaikan logika Toko: pembelian disimpan di localStorage (key: `purchases`), item tidak dapat digunakan sebelum dibeli.
- AudioManager yang lebih toleran: WebAudio fallback dan oscillator preview jika file audio tidak tersedia.
- 10 fitur kecil: Quick Notes, Flashcards generator, Pomodoro, Export/Import data, Dark mode, TTS, Font size controls, Mini translator, Study minutes tracker, Exam simulator.
- Riwayat AI yang tersimpan (localStorage `edu_ai_history`) dan UI riwayat di modal AI.

File yang diubah/ditambahkan:
- `index.html` - menambahkan tombol AI dan modal, serta memuat `edu-patch.js`.
- `edu-patch.js` - file baru dengan semua logika tambahan.
- `README.md` - instruksi singkat ini.

Cara coba cepat:
1. Buka `index.html` di browser (double click atau drag ke browser).
2. Di header, klik "🤖 Belajar dengan AI" untuk membuka modal AI.
   - Pilih topik (jika tersedia), masukkan prompt, klik "Minta AI" atau "Buat Kuis Cepat".
   - Klik "Bacakan" untuk TTS.
   - Klik "Simpan Catatan" untuk menyimpan jawaban AI.
3. Buka Toko lewat tombol "🛍️ Toko" dan pilih salah satu toko. Untuk menguji pembelian, set poin di console terlebih dahulu:

```powershell
# di console browser (Developer Tools)
localStorage.setItem('player_points', '1000');
```

Lalu klik item dan konfirmasi pembelian.

4. Coba tombol cepat di kanan atas layar (flashcards, pomodoro, export/import, dark mode, TTS, ukuran teks, translator, simulasi).

Hal-hal yang perlu diketahui / langkah selanjutnya yang direkomendasikan:
- AI adalah offline dan rule-based. Untuk integrasi model AI sebenarnya, butuh API dan backend.
- Jika Anda ingin paket audio khusus (bgm/sfx), beri saya file URL atau folder, saya akan menghubungkannya ke `shopData`.
- Uji di HP: buka `index.html` di browser HP (file://) atau jalankan melalui server lokal (mis. `python -m http.server`) untuk menghindari beberapa pembatasan browser.

Jika mau, saya bisa:
- Menyempurnakan AI (lebih kaya template jawaban, scoring kuis).
- Menautkan file audio lokal dan menambahkan preview/streaming.
- Menulis test smoke (script kecil) untuk memastikan tidak ada error console pada load.

Beritahu prioritas Anda, atau saya akan lanjutkan: selesaikan Todo #2 (selesai), kemudian lanjutkan Todo #3 (selesaikan 10 fitur lebih lengkap dan polish mobile).

How to test the Achievements UI changes
--------------------------------------

1. Open `index.html` in a modern browser (double-click or serve via `python -m http.server`).
2. Create a user or select an existing user. Make sure the user has a `jenjang` set (SD/SMP/SMA/Kuliah).
3. Click the header button "🏆 Prestasi" to open the achievements modal.
4. Use the "Jenjang" dropdown to select SD / SMP / SMA / Kuliah — only achievements for the chosen jenjang should be visible.
5. In the modal's debug area (bottom), choose a jenjang and one achievement then click "Award" — you should see:
   - Confetti and badge pop animation,
   - The specific achievement card will animate (pop) and change to the unlocked color,
   - A green checkmark badge appears on the card and the pill on the right shows "Terklaim".
6. Close and re-open the modal to verify the unlocked state persists.

If you want any further polish (exact spacing, fonts, or mobile-specific adjustments), tell me which elements to match exactly from your screenshot and I'll refine them.

Automated smoke test (quick)
----------------------------

I added a tiny smoke test file `test_achievements.js` to the project root. It is a small script you can load in the browser console to verify the award animation and DOM changes.

How to run the smoke test:
1. Serve the project folder (recommended) or open `index.html` directly in the browser.

   Example using Python's simple server (from project folder):

```powershell
python -m http.server 8000
# Open http://localhost:8000 in your browser
```

2. Open DevTools -> Console while the app is running and the user is active.
3. Load the test script (one of the options below):

- Option A: Paste the contents of `test_achievements.js` into the console and press Enter.
- Option B: Run the loader helper in Console (when served):

```javascript
// loads and executes the test file from the server
fetch('/test_achievements.js').then(r=>r.text()).then(eval);
```

4. The script will open the Achievements modal, force-award the first achievement for the active user's jenjang, and log PASS/FAIL in the console.

If you see a PASS message and the card animation + green check appear, the feature is working.