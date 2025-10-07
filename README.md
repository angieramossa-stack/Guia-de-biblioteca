<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>IA - Guías rápidas | Biblioteca CUN</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap" rel="stylesheet">
  <style>
    :root{
      --cun-blue:#003366;
      --cun-green:#00A86B;
      --bg:#f7fbfd;
      --card:#ffffff;
      --muted:#556;
      --radius:12px;
    }
    *{box-sizing:border-box;font-family:Inter,system-ui,Segoe UI,Roboto,'Helvetica Neue',Arial}
    body{margin:0;background:linear-gradient(180deg,var(--bg),#fff);color:var(--muted);padding:28px}
    .wrap{max-width:980px;margin:0 auto}
    header{display:flex;align-items:center;gap:16px}
    .logo{width:68px;height:68px;border-radius:10px;background:linear-gradient(135deg,var(--cun-blue),var(--cun-green));display:flex;align-items:center;justify-content:center;color:#fff;font-weight:700}
    h1{margin:0;font-size:20px;color:var(--cun-blue)}
    p.lead{margin:6px 0 18px;color:#234}

    .grid{display:grid;grid-template-columns:1fr 380px;gap:18px}
    .card{background:var(--card);padding:18px;border-radius:var(--radius);box-shadow:0 6px 18px rgba(10,20,30,0.04)}

    label{display:block;font-size:13px;color:#234;margin-top:10px}
    input[type=text],select,textarea{width:100%;padding:10px;border:1px solid #e3eef2;border-radius:8px;font-size:14px}
    textarea{min-height:120px}
    .row{display:flex;gap:8px}
    .btn{display:inline-block;padding:10px 14px;border-radius:10px;border:0;background:var(--cun-blue);color:#fff;cursor:pointer}
    .btn.secondary{background:var(--cun-green)}
    .muted{font-size:13px;color:#667}

    .preview{min-height:260px;padding:10px;border-radius:8px;background:linear-gradient(180deg,#fff,#f4fbf7)}
    .guide{padding:18px;border-radius:8px;background:#fff}
    .guide h2{color:var(--cun-blue);margin-top:0}
    .step{display:flex;gap:12px;align-items:flex-start;margin:12px 0}
    .step .num{background:var(--cun-blue);color:#fff;width:34px;height:34px;border-radius:8px;display:flex;align-items:center;justify-content:center;font-weight:700}
    footer.small{font-size:12px;color:#556;margin-top:8px}

    .controls{display:flex;gap:8px;flex-wrap:wrap}
    .qrwrap{display:flex;flex-direction:column;align-items:center;gap:8px}
    .qr{background:#fff;padding:10px;border-radius:8px}

    @media(max-width:880px){
      .grid{grid-template-columns:1fr}
      .logo{width:54px;height:54px}
    }
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <div class="logo">CUN</div>
      <div>
        <h1>IA para guías rápidas — Biblioteca CUN</h1>
        <p class="lead">Genera, comparte y descarga guías de búsqueda en bases de datos (colores: verde + azul CUN).</p>
      </div>
    </header>

    <main class="grid" style="margin-top:16px">
      <section class="card">
        <h3>Configura tu guía</h3>
        <label>Título de la guía</label>
        <input id="title" type="text" value="Cómo buscar un artículo académico en bases de datos" />

        <label>Audiencia</label>
        <select id="audience">
          <option>Estudiantes (principiantes)</option>
          <option>Estudiantes (avanzados)</option>
          <option>Docentes</option>
          <option selected>General</option>
        </select>

        <label>Nº de pasos</label>
        <select id="steps">
          <option>3</option>
          <option>4</option>
          <option selected>5</option>
          <option>6</option>
        </select>

        <label>Breve descripción / objetivo (opcional)</label>
        <textarea id="objective">Aprende en pasos sencillos cómo localizar y evaluar un artículo académico usando una base de datos institucional.</textarea>

        <label>Incluir sección: Buenas prácticas (privacidad y licencias)</label>
        <select id="includeBest"> 
          <option value="yes">Sí</option>
          <option value="no">No</option>
        </select>

        <div style="margin-top:12px" class="controls">
          <button class="btn" onclick="generateGuide()">Generar guía</button>
          <button class="btn secondary" onclick="clearPreview()">Limpiar</button>
          <button class="btn" id="downloadPdf" onclick="downloadPdf()" disabled>Descargar PDF</button>
        </div>

        <div style="margin-top:12px" class="muted">Consejo: usa el botón generar y luego comparte el enlace o el QR para pruebas con usuarios.</div>
      </section>

      <aside class="card">
        <h4>Vista previa</h4>
        <div id="preview" class="preview"><div style="color:#889;padding:14px">Aquí aparecerá la guía generada.</div></div>

        <div style="margin-top:14px;display:flex;gap:8px;align-items:center">
          <button class="btn" id="openLink" onclick="openShareLink()" disabled>Abrir enlace</button>
          <button class="btn secondary" id="copyLink" onclick="copyLink()" disabled>Copiar enlace</button>
        </div>

        <div style="margin-top:12px" class="qrwrap">
          <div id="qrcode" class="qr"></div>
          <div class="muted">QR del enlace (escanea para abrir la guía en otro dispositivo). <br><small style="color:#a44">Nota: el enlace es un data-URL largo; algunos lectores pueden no soportarlo.</small></div>
        </div>
      </aside>
    </main>

    <section style="margin-top:18px" class="card">
      <h3>Plantilla de ejemplo (editable)</h3>
      <p class="muted">Puedes editar los textos arriba y volver a generar. El diseño respeta los colores azul/verde CUN.</p>
    </section>

    <footer style="margin-top:10px" class="small">Hecho por Angie — Proyecto Guardianes Digitales. Evita incluir datos reales de usuarios al compartir guías.</footer>
  </div>

  <!-- Librerías: QRCode y html2pdf -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.9.3/html2pdf.bundle.min.js"></script>

  <script>
    let lastDataUrl = '';
    function escapeHtml(s){return s.replaceAll('&','&amp;').replaceAll('<','&lt;').replaceAll('>','&gt;')}

    function generateGuide(){
      const title = document.getElementById('title').value.trim();
      const audience = document.getElementById('audience').value;
      const n = parseInt(document.getElementById('steps').value,10);
      const objective = document.getElementById('objective').value.trim();
      const includeBest = document.getElementById('includeBest').value === 'yes';

      // Simple step templates (you can refine)
      const stepsTexts = [
        'Define palabras clave: piensa en sinónimos y términos técnicos.',
        'Usa filtros básicos: tipo de documento, fecha, idioma.',
        'Revisa el resumen (abstract) para confirmar relevancia.',
        'Accede al PDF o al enlace de texto completo desde la base de datos.',
        'Guarda la referencia en tu gestor o exporta la cita.'
      ];

      // If n differs, slice or repeat
      let steps = [];
      for(let i=0;i<n;i++){
        steps.push(stepsTexts[i] || stepsTexts[stepsTexts.length-1]);
      }

      const bestPractices = includeBest ? `
        <h3>Buenas prácticas</h3>
        <ul>
          <li>No subas documentos con datos personales identificables a herramientas públicas de IA.</li>
          <li>Verifica las licencias antes de compartir o reutilizar contenidos.</li>
          <li>Usa cuentas institucionales y guarda evidencia en repositorios de la CUN.</li>
        </ul>
      ` : '';

      const guideHtml = `
        <div class="guide" style="font-family:Inter,Arial;max-width:760px">
          <h2>${escapeHtml(title)}</h2>
          <p class="muted"><strong>Audiencia:</strong> ${escapeHtml(audience)} &nbsp; • &nbsp; <strong>Objetivo:</strong> ${escapeHtml(objective)}</p>
          <hr />
          <div>
            ${steps.map((s,i)=>`<div class="step"><div class="num">${i+1}</div><div style="flex:1"><strong>Paso ${i+1}:</strong> ${escapeHtml(s)}</div></div>`).join('')}
          </div>
          ${bestPractices}
          <hr />
          <footer style="color:#556;font-size:13px">Biblioteca CUN — Guía rápida generada con asistencia de IA (plantilla). Evita datos sensibles.</footer>
        </div>
      `;

      document.getElementById('preview').innerHTML = guideHtml;

      // Build a shareable data URL page
      const page = `<!doctype html><html><head><meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1"><title>${escapeHtml(title)}</title>
        <style>body{font-family:Inter,Arial;padding:20px;background:#f6fbff;color:#123} .guide{max-width:760px}</style></head><body>${guideHtml}</body></html>`;

      const dataUrl = 'data:text/html;charset=utf-8,' + encodeURIComponent(page);
      lastDataUrl = dataUrl;

      // Enable buttons
      document.getElementById('openLink').disabled = false;
      document.getElementById('copyLink').disabled = false;
      document.getElementById('downloadPdf').disabled = false;

      // Generate QR
      const qrContainer = document.getElementById('qrcode');
      qrContainer.innerHTML = '';
      try{
        // QR code library sometimes fails with extremely long data URLs; we catch errors
        new QRCode(qrContainer, {text: dataUrl, width:140, height:140});
      }catch(e){
        qrContainer.innerHTML = '<div style="color:#b33">No se pudo generar QR (URL demasiado larga)</div>';
      }
    }

    function openShareLink(){
      if(!lastDataUrl) return alert('Genera la guía primero');
      window.open(lastDataUrl,'_blank');
    }

    function copyLink(){
      if(!lastDataUrl) return alert('Genera la guía primero');
      navigator.clipboard.writeText(lastDataUrl).then(()=>{
        alert('Enlace copiado. Atención: puede ser una URL larga (data-URL).')
      }).catch(()=>alert('No fue posible copiar.'));
    }

    function downloadPdf(){
      const el = document.querySelector('#preview .guide');
      if(!el) return alert('Genera la guía primero');
      const opt = {margin:0.5,filename: (document.getElementById('title').value||'guia')+'.pdf',image:{type:'jpeg',quality:0.98},html2canvas:{scale:2},jsPDF:{unit:'in',format:'letter',orientation:'portrait'}};
      html2pdf().set(opt).from(el).save();
    }

    function clearPreview(){
      document.getElementById('preview').innerHTML = '<div style="color:#889;padding:14px">Aquí aparecerá la guía generada.</div>';
      document.getElementById('qrcode').innerHTML = '';
      lastDataUrl = '';
      document.getElementById('openLink').disabled = true;
      document.getElementById('copyLink').disabled = true;
      document.getElementById('downloadPdf').disabled = true;
    }

    // Auto-generate an example on load
    window.addEventListener('DOMContentLoaded', ()=>{generateGuide()});
  </script>
</body>
</html>
