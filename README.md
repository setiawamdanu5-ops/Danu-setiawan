# Danu-setiawan
chek up kesehatan
<!doctype html>
<html lang="id">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>HealthCheck — Pemeriksaan Kesehatan Singkat</title>
  <meta name="description" content="Alat pemeriksaan awal yang mendeteksi kemungkinan hipertensi, kadar kolesterol total, dan kadar asam urat." />
  <style>
    :root{--bg:#f7fafc;--card:#ffffff;--muted:#6b7280;--accent:#2563eb;--danger:#dc2626;--success:#16a34a}
    *{box-sizing:border-box}
    body{font-family:Inter, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial; background:var(--bg); margin:0; padding:24px;color:#111827}
    .container{max-width:920px;margin:0 auto}
    header h1{margin:0;font-size:1.75rem}
    header p{color:var(--muted);margin-top:6px}
    .card{background:var(--card);border-radius:12px;padding:18px;box-shadow:0 6px 18px rgba(15,23,42,0.06);margin-top:18px}
    .grid{display:grid;grid-template-columns:repeat(2,1fr);gap:12px}
    label{display:block;font-size:0.9rem;margin-bottom:6px}
    input[type=number], select, textarea{width:100%;padding:10px;border:1px solid #e5e7eb;border-radius:8px;font-size:0.95rem}
    textarea{resize:vertical}
    .actions{margin-top:12px;display:flex;gap:10px}
    button{padding:10px 14px;border-radius:8px;border:0;cursor:pointer;font-weight:600}
    .btn-primary{background:var(--accent);color:white}
    .btn-secondary{background:white;border:1px solid #d1d5db}
    .result{margin-top:12px;padding:12px;border-radius:8px;background:#fbfdff;border:1px solid #e6f0ff}
    .muted{color:var(--muted)}
    .small{font-size:0.9rem}
    ul.tips{margin:8px 0 0 18px;color:var(--muted)}
    .badge{display:inline-block;padding:6px 8px;border-radius:999px;font-weight:700}
    .danger{background:rgba(220,38,38,0.08);color:var(--danger);border:1px solid rgba(220,38,38,0.12)}
    .success{background:rgba(22,163,74,0.08);color:var(--success);border:1px solid rgba(22,163,74,0.12)}

    /* responsive */
    @media (max-width:820px){.grid{grid-template-columns:1fr}}
    .history-list{max-height:220px;overflow:auto;margin-top:8px;border-top:1px dashed #e5e7eb;padding-top:8px}
    .history-item{padding:8px;border-radius:8px;margin-bottom:8px;background:#fff}
    .footer{font-size:0.85rem;color:var(--muted);margin-top:10px}
  </style>
</head>
<body>
  <div class="container">
    <header>
      <h1>HealthCheck — Pemeriksaan Kesehatan Singkat</h1>
      <p>Alat pemeriksaan awal untuk semua usia. Memberikan interpretasi singkat untuk hipertensi, kolesterol total, dan asam urat (hiperurisemia).</p>
    </header>

    <main>
      <section class="card">
        <h2 style="margin-top:0">Form Pemeriksaan</h2>
        <div class="grid">
          <div>
            <label>Usia</label>
            <input id="age" type="number" min="0" value="30" />
          </div>
          <div>
            <label>Jenis Kelamin</label>
            <select id="sex">
              <option value="male">Laki-laki</option>
              <option value="female">Perempuan</option>
            </select>
          </div>

          <div>
            <label>Sistolik (mmHg)</label>
            <input id="systolic" type="number" value="120" />
          </div>
          <div>
            <label>Diastolik (mmHg)</label>
            <input id="diastolic" type="number" value="80" />
          </div>

          <div>
            <label>Total Kolesterol (mg/dL)</label>
            <input id="cholesterol" type="number" value="180" />
          </div>
          <div>
            <label>Asam Urat (mg/dL)</label>
            <input id="uric" type="number" step="0.1" value="5.5" />
          </div>

          <div style="grid-column:1 / -1">
            <label>Catatan / Keluhan (opsional)</label>
            <textarea id="notes" rows="3" placeholder="Keluhan singkat atau riwayat..."></textarea>
          </div>
        </div>

        <div class="actions">
          <button id="checkBtn" class="btn-primary">Periksa</button>
          <button id="resetBtn" class="btn-secondary">Reset</button>
          <button id="printBtn" class="btn-secondary">Cetak Hasil</button>
        </div>

        <div id="resultArea" class="result" aria-live="polite" style="display:none"></div>

        <div class="footer">
          <strong>Catatan:</strong> Alat ini hanya memberikan interpretasi awal berdasarkan angka yang dimasukkan. BUKAN pengganti konsultasi dokter. Untuk penegakan diagnosa dan pengobatan, kunjungi fasilitas kesehatan.
        </div>
      </section>

      <section class="card" style="margin-top:14px">
        <h3 style="margin-top:0">Riwayat Pemeriksaan (disimpan lokal)</h3>
        <p class="muted small">Riwayat disimpan di penyimpanan browser (localStorage) pada perangkat ini. Kamu dapat menghapus riwayat kapan saja.</p>
        <div style="display:flex;gap:10px;margin-top:8px">
          <button id="clearHistory" class="btn-secondary">Hapus Riwayat</button>
        </div>
        <div id="history" class="history-list"></div>
      </section>

      <section class="card" style="margin-top:14px">
        <h3 style="margin-top:0">Sumber & Ambang Interpretasi (ringkas)</h3>
        <ul class="tips">
          <li><strong>Hipertensi:</strong> ambang deteksi singkat pada alat ini menggunakan <strong>≥ 140/90 mmHg</strong> (nilai yang sering dipakai untuk diagnosis klinik oleh banyak pedoman internasional).</li>
          <li><strong>Kolesterol total:</strong> &lt;200 mg/dL (Desirable), 200–239 mg/dL (Borderline), ≥240 mg/dL (High).</li>
          <li><strong>Asam urat:</strong> ambang hiperurisemia dapat berbeda antar laboratorium; alat ini menggunakan contoh ambang <strong>> 8.0 mg/dL (pria)</strong> dan <strong>> 6.1 mg/dL (wanita)</strong> sebagai referensi awal.</li>
        </ul>
        <p class="muted small">(Sumber: pedoman kesehatan dan referensi laboratorium klinik umum — untuk edukasi. Pastikan memverifikasi kebijakan lokal atau pedoman terbaru jika diperlukan.)</p>
      </section>

    </main>
  </div>

  <script>
    // Helper
    function el(id){return document.getElementById(id)}

    const checkBtn = el('checkBtn')
    const resetBtn = el('resetBtn')
    const printBtn = el('printBtn')
    const resultArea = el('resultArea')
    const historyEl = el('history')
    const clearHistory = el('clearHistory')

    function interpret(values){
      const {systolic, diastolic, cholesterol, uric, sex} = values
      const isHypertensive = systolic >= 140 || diastolic >= 90
      let cholCategory = 'Desirable (<200)'
      if(cholesterol >= 240) cholCategory = 'Tinggi (>=240)'
      else if(cholesterol >=200) cholCategory = 'Batas (200-239)'

      const uricThreshold = sex === 'male' ? 8.0 : 6.1
      const isHyperuricemic = uric > uricThreshold

      return {isHypertensive, cholCategory, isHyperuricemic, uricThreshold}
    }

    function showResult(values, interp){
      const time = new Date().toLocaleString('id-ID')
      let html = ''
      html += `<p class="small"><strong>Waktu pemeriksaan:</strong> ${time}</p>`

      html += `<div style="margin-top:8px"><h4 style="margin:6px 0">Hipertensi</h4>`
      if(interp.isHypertensive) html += `<div class="badge danger">Kemungkinan hipertensi</div>`
      else html += `<div class="badge success">Tekanan darah dalam kisaran normal</div>`
      html += `<p class="muted small" style="margin-top:8px">Kriteria deteksi singkat: Sistolik ≥ 140 mmHg atau Diastolik ≥ 90 mmHg.</p></div>`

      html += `<div style="margin-top:10px"><h4 style="margin:6px 0">Kolesterol Total</h4>`
      html += `<p><strong>Kategori:</strong> ${interp.cholCategory}</p>`
      html += `<p class="muted small">Desirable: &lt;200 mg/dL · Borderline: 200–239 mg/dL · High: ≥240 mg/dL.</p></div>`

      html += `<div style="margin-top:10px"><h4 style="margin:6px 0">Asam Urat</h4>`
      html += `<p>Ambang yang dipakai: &gt; ${interp.uricThreshold} mg/dL untuk mendeteksi hiperurisemia pada jenis kelamin yang dipilih.</p>`
      if(interp.isHyperuricemic) html += `<div class="badge danger">Kadar asam urat di atas ambang (hiperurisemia)</div>`
      else html += `<div class="badge success">Kadar asam urat dalam kisaran yang biasa</div>`
      html += `</div>`

      html += `<div style="margin-top:10px"><h4 style="margin:6px 0">Catatan penting</h4>`
      html += `<ol class="small muted"><li>Ini hanya interpretasi awal, bukan diagnosa resmi.</li><li>Jika terdeteksi risiko, segera konsultasi ke fasilitas kesehatan.</li></ol></div>`

      resultArea.innerHTML = html
      resultArea.style.display = 'block'
    }

    function loadHistory(){
      const raw = localStorage.getItem('healthcheck_history')
      if(!raw){ historyEl.innerHTML = '<p class="muted small">Belum ada riwayat pemeriksaan.</p>'; return }
      try{
        const arr = JSON.parse(raw)
        if(!arr.length){ historyEl.innerHTML = '<p class="muted small">Belum ada riwayat pemeriksaan.</p>'; return }
        historyEl.innerHTML = ''
        arr.slice().reverse().forEach(item => {
          const d = document.createElement('div')
          d.className = 'history-item'
          d.innerHTML = `<div style="display:flex;justify-content:space-between;align-items:center"><div class="small"><strong>${item.time}</strong><div class="muted">Usia ${item.age} · ${item.sex === 'male' ? 'L' : 'P'}</div></div><div class="small">S:${item.systolic}/D:${item.diastolic}</div></div>
            <div class="small muted" style="margin-top:6px">Kol:${item.cholesterol} mg/dL · Urat:${item.uric} mg/dL</div>`
          historyEl.appendChild(d)
        })
      }catch(e){ console.error(e); historyEl.innerHTML = '<p class="muted small">Tidak dapat memuat riwayat.</p>' }
    }

    function pushHistory(entry){
      const raw = localStorage.getItem('healthcheck_history')
      let arr = raw ? JSON.parse(raw) : []
      arr.push(entry)
      // keep up to 200 records
      if(arr.length > 200) arr = arr.slice(arr.length-200)
      localStorage.setItem('healthcheck_history', JSON.stringify(arr))
      loadHistory()
    }

    checkBtn.addEventListener('click', ()=>{
      const values = {
        age: Number(el('age').value) || 0,
        sex: el('sex').value,
        systolic: Number(el('systolic').value) || 0,
        diastolic: Number(el('diastolic').value) || 0,
        cholesterol: Number(el('cholesterol').value) || 0,
        uric: Number(el('uric').value) || 0,
        notes: el('notes').value || ''
      }

      // Basic validation
      if(values.systolic <= 0 || values.diastolic <= 0){
        alert('Masukkan nilai tekanan darah yang valid.');
        return
      }

      const interp = interpret(values)
      showResult(values, interp)

      const entry = {
        time: new Date().toLocaleString('id-ID'),
        ...values
      }
      pushHistory(entry)
    })

    resetBtn.addEventListener('click', ()=>{
      el('age').value = 30
      el('sex').value = 'male'
      el('systolic').value = 120
      el('diastolic').value = 80
      el('cholesterol').value = 180
      el('uric').value = 5.5
      el('notes').value = ''
      resultArea.style.display = 'none'
    })

    printBtn.addEventListener('click', ()=>{
      window.print()
    })

    clearHistory.addEventListener('click', ()=>{
      if(confirm('Hapus seluruh riwayat pemeriksaan yang tersimpan di perangkat ini?')){
        localStorage.removeItem('healthcheck_history')
        loadHistory()
      }
    })

    // init
    loadHistory()
  </script>
</body>
</html>
