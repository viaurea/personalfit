<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Personal Fit · Clientes</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800;900&display=swap');
    * { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
    :root {
      --bg: #0a0a0a; --card: #141414; --border: #1e1e1e;
      --accent: #e8ff00; --green: #22c55e; --orange: #f97316; --red: #ef4444;
      --dim: #444; --mid: #777; --text: #f0f0f0;
    }
    body { font-family: 'Inter', sans-serif; background: var(--bg); color: var(--text); min-height: 100vh; max-width: 480px; margin: 0 auto; }

    /* ── HEADER ── */
    .header {
      position: sticky; top: 0; z-index: 10;
      background: rgba(10,10,10,0.95); backdrop-filter: blur(12px);
      border-bottom: 1px solid var(--border); padding: 14px 16px 12px;
    }
    .header-row { display: flex; align-items: center; justify-content: space-between; margin-bottom: 12px; }
    .header-title { font-size: 18px; font-weight: 900; letter-spacing: -0.5px; color: #fff; }
    .header-title em { font-style: normal; color: var(--accent); }
    .header-btns { display: flex; gap: 8px; align-items: center; }
    .btn-add { background: var(--accent); color: #000; font-size: 13px; font-weight: 700; padding: 8px 16px; border-radius: 10px; border: none; cursor: pointer; }
    .btn-backup { background: var(--card); color: var(--mid); font-size: 12px; font-weight: 600; padding: 8px 12px; border-radius: 10px; border: 1px solid var(--border); cursor: pointer; }
    .btn-backup:hover { border-color: var(--accent); color: var(--accent); }
    .search { width: 100%; background: var(--card); border: 1px solid var(--border); border-radius: 10px; padding: 10px 14px; color: var(--text); font-size: 14px; font-family: 'Inter', sans-serif; }
    .search::placeholder { color: var(--dim); }
    .search:focus { outline: none; border-color: rgba(232,255,0,0.3); }

    /* ── RESUMEN GLOBAL ── */
    .global-stats { display: grid; grid-template-columns: repeat(3,1fr); gap: 1px; background: var(--border); border-bottom: 1px solid var(--border); }
    .global-stat { background: var(--bg); padding: 12px 8px; text-align: center; }
    .global-stat-label { font-size: 9px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.8px; color: var(--dim); margin-bottom: 4px; }
    .global-stat-value { font-size: 17px; font-weight: 900; letter-spacing: -0.5px; }

    /* ── ALERTAS ── */
    .alertas { padding: 12px 16px 0; display: flex; flex-direction: column; gap: 6px; }
    .alerta { display: flex; align-items: center; gap: 10px; padding: 10px 14px; border-radius: 10px; cursor: pointer; }
    .alerta.red { background: rgba(239,68,68,0.08); border: 1px solid rgba(239,68,68,0.2); }
    .alerta.orange { background: rgba(249,115,22,0.08); border: 1px solid rgba(249,115,22,0.2); }
    .alerta-dot { width: 8px; height: 8px; border-radius: 50%; flex-shrink: 0; }
    .alerta.red .alerta-dot { background: var(--red); }
    .alerta.orange .alerta-dot { background: var(--orange); }
    .alerta-text { font-size: 12px; color: var(--text); flex: 1; }
    .alerta-text strong { font-weight: 700; }
    .alerta-badge { font-size: 10px; font-weight: 700; padding: 2px 8px; border-radius: 100px; }
    .alerta.red .alerta-badge { background: rgba(239,68,68,0.15); color: var(--red); }
    .alerta.orange .alerta-badge { background: rgba(249,115,22,0.15); color: var(--orange); }

    /* ── SECCIÓN ── */
    .section-header { padding: 16px 16px 8px; display: flex; align-items: center; justify-content: space-between; }
    .section-label { font-size: 10px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.8px; color: var(--dim); }

    /* ── CLIENTE CARD ── */
    .clients { display: flex; flex-direction: column; gap: 6px; padding: 0 16px 24px; }
    .client-row {
      background: var(--card); border: 1px solid var(--border); border-radius: 14px;
      padding: 14px 16px; display: flex; align-items: center; gap: 14px; cursor: pointer; transition: border-color 0.15s;
    }
    .client-row:active { background: #1a1a1a; }
    .client-row.urgent-red { border-color: rgba(239,68,68,0.3); }
    .client-row.urgent-orange { border-color: rgba(249,115,22,0.25); }

    .sessions-bubble { width: 44px; height: 44px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 16px; font-weight: 900; flex-shrink: 0; }
    .sessions-bubble.green { background: rgba(34,197,94,0.12); color: var(--green); border: 1.5px solid rgba(34,197,94,0.25); }
    .sessions-bubble.orange { background: rgba(249,115,22,0.12); color: var(--orange); border: 1.5px solid rgba(249,115,22,0.3); }
    .sessions-bubble.red { background: rgba(239,68,68,0.12); color: var(--red); border: 1.5px solid rgba(239,68,68,0.3); }

    .client-info { flex: 1; min-width: 0; }
    .client-name { font-size: 14px; font-weight: 700; color: #fff; margin-bottom: 3px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
    .client-meta { font-size: 12px; color: var(--mid); }
    .client-meta span { color: var(--text); font-weight: 500; }
    .client-arrow { color: var(--dim); font-size: 16px; }
    .empty { text-align: center; padding: 56px 20px; color: var(--dim); font-size: 13px; line-height: 1.6; }

    /* ── FICHA ── */
    .ficha-overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.9); z-index: 50; align-items: flex-end; justify-content: center; }
    .ficha-overlay.open { display: flex; }
    .ficha { background: var(--bg); border: 1px solid var(--border); border-radius: 20px 20px 0 0; width: 100%; max-width: 480px; max-height: 92vh; overflow-y: auto; padding-bottom: 32px; }
    .ficha-handle { width: 36px; height: 4px; background: var(--border); border-radius: 2px; margin: 14px auto 0; }
    .ficha-header { padding: 16px 20px 14px; border-bottom: 1px solid var(--border); display: flex; align-items: center; gap: 14px; }
    .ficha-bubble { width: 56px; height: 56px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 22px; font-weight: 900; flex-shrink: 0; }
    .ficha-bubble.green { background: rgba(34,197,94,0.1); color: var(--green); border: 2px solid rgba(34,197,94,0.25); }
    .ficha-bubble.orange { background: rgba(249,115,22,0.1); color: var(--orange); border: 2px solid rgba(249,115,22,0.3); }
    .ficha-bubble.red { background: rgba(239,68,68,0.1); color: var(--red); border: 2px solid rgba(239,68,68,0.3); }
    .ficha-info { flex: 1; }
    .ficha-name { font-size: 18px; font-weight: 900; color: #fff; letter-spacing: -0.3px; margin-bottom: 3px; }
    .ficha-sub { font-size: 12px; color: var(--mid); }
    .ficha-header-btns { display: flex; gap: 8px; }
    .ficha-icon-btn { width: 32px; height: 32px; border-radius: 50%; background: var(--card); border: 1px solid var(--border); color: var(--mid); font-size: 14px; cursor: pointer; display: flex; align-items: center; justify-content: center; }
    .ficha-icon-btn.wa { background: rgba(37,211,102,0.1); border-color: rgba(37,211,102,0.3); color: #25d366; }
    .ficha-icon-btn.wa:hover { background: rgba(37,211,102,0.2); }

    .ficha-stats { display: grid; grid-template-columns: repeat(3,1fr); gap: 1px; background: var(--border); }
    .ficha-stat { background: var(--bg); padding: 14px 8px; text-align: center; }
    .ficha-stat-label { font-size: 9px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.8px; color: var(--dim); margin-bottom: 5px; }
    .ficha-stat-value { font-size: 20px; font-weight: 900; letter-spacing: -0.5px; }

    .ficha-actions { display: flex; gap: 8px; padding: 14px 16px; border-bottom: 1px solid var(--border); flex-wrap: wrap; }
    .action-btn { flex: 1; min-width: 80px; padding: 12px 8px; border-radius: 12px; font-size: 13px; font-weight: 700; border: none; cursor: pointer; text-align: center; }
    .action-btn.primary { background: var(--accent); color: #000; }
    .action-btn.secondary { background: var(--card); color: var(--text); border: 1px solid var(--border); }
    .action-btn.warning { background: rgba(249,115,22,0.1); color: var(--orange); border: 1px solid rgba(249,115,22,0.2); }
    .action-btn.danger { background: rgba(239,68,68,0.08); color: var(--red); border: 1px solid rgba(239,68,68,0.2); }

    .ficha-section { padding: 14px 16px; }
    .ficha-section-title { font-size: 10px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.8px; color: var(--dim); margin-bottom: 10px; }
    .log-item { display: flex; align-items: center; gap: 10px; padding: 9px 0; border-bottom: 1px solid var(--border); }
    .log-item:last-child { border-bottom: none; }
    .log-icon { width: 28px; height: 28px; border-radius: 8px; display: flex; align-items: center; justify-content: center; font-size: 13px; flex-shrink: 0; }
    .log-icon.sesion { background: rgba(59,130,246,0.1); }
    .log-icon.pago { background: rgba(34,197,94,0.1); }
    .log-text { flex: 1; font-size: 13px; color: var(--text); line-height: 1.4; }
    .log-date { font-size: 11px; color: var(--dim); }
    .log-amount { font-size: 13px; font-weight: 700; color: var(--green); }
    .log-del { background: none; border: none; color: var(--dim); cursor: pointer; font-size: 14px; padding: 4px; border-radius: 6px; }
    .log-del:hover { color: var(--red); }
    .log-empty { font-size: 12px; color: var(--dim); padding: 8px 0; }

    /* ── MODALES ── */
    .mini-overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.85); z-index: 100; align-items: flex-end; justify-content: center; }
    .mini-overlay.open { display: flex; }
    .mini-modal { background: var(--card); border: 1px solid var(--border); border-radius: 20px 20px 0 0; width: 100%; max-width: 480px; padding: 24px 20px 40px; }
    .mini-title { font-size: 15px; font-weight: 800; color: #fff; margin-bottom: 16px; }
    .field { margin-bottom: 12px; }
    .field label { display: block; font-size: 11px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.5px; color: var(--mid); margin-bottom: 6px; }
    .field input { width: 100%; background: var(--bg); border: 1px solid var(--border); border-radius: 10px; padding: 11px 14px; color: var(--text); font-size: 14px; font-family: 'Inter', sans-serif; }
    .field input:focus { outline: none; border-color: rgba(232,255,0,0.4); }
    .field-row { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
    .mini-actions { display: flex; gap: 8px; margin-top: 16px; }
    .btn-primary { flex: 1; background: var(--accent); color: #000; font-weight: 700; font-size: 14px; padding: 13px; border-radius: 12px; border: none; cursor: pointer; }
    .btn-cancel { padding: 13px 18px; border-radius: 12px; border: 1px solid var(--border); background: transparent; color: var(--mid); font-size: 14px; cursor: pointer; }
  </style>
</head>
<body>

<!-- HEADER -->
<div class="header">
  <div class="header-row">
    <div class="header-title">Personal <em>Fit</em></div>
    <div class="header-btns">
      <button class="btn-backup" onclick="exportarDatos()" title="Backup">💾</button>
      <button class="btn-backup" onclick="importarDatos()" title="Cargar backup">📂</button>
      <button class="btn-add" onclick="openNuevoCliente()">+ Cliente</button>
    </div>
  </div>
  <input class="search" type="text" id="search" placeholder="Buscar cliente..." oninput="renderList()">
  <input type="file" id="fileInput" accept=".json" style="display:none" onchange="cargarArchivo(event)">
</div>

<!-- RESUMEN GLOBAL -->
<div class="global-stats">
  <div class="global-stat">
    <div class="global-stat-label">Clientes</div>
    <div class="global-stat-value" id="gClientes" style="color:#fff">0</div>
  </div>
  <div class="global-stat">
    <div class="global-stat-label">Total cobrado</div>
    <div class="global-stat-value" id="gCobrado" style="color:var(--green)">0€</div>
  </div>
  <div class="global-stat">
    <div class="global-stat-label">Sin sesiones</div>
    <div class="global-stat-value" id="gSinSesiones" style="color:var(--red)">0</div>
  </div>
</div>

<!-- ALERTAS -->
<div class="alertas" id="alertas"></div>

<!-- LISTA -->
<div class="section-header">
  <div class="section-label" id="sectionLabel">Todos los clientes</div>
</div>
<div class="clients" id="clientList"></div>
<div class="empty" id="emptyState" style="display:none">Sin clientes todavía.<br>Pulsa "+ Cliente" para añadir el primero.</div>

<!-- ═══ FICHA CLIENTE ═══ -->
<div class="ficha-overlay" id="fichaOverlay" onclick="closeFichaIfBg(event)">
  <div class="ficha" id="fichaPanel">
    <div class="ficha-handle"></div>
    <div class="ficha-header">
      <div class="ficha-bubble" id="fichaBubble">0</div>
      <div class="ficha-info">
        <div class="ficha-name" id="fichaNombre">—</div>
        <div class="ficha-sub" id="fichaSub">—</div>
      </div>
      <div class="ficha-header-btns">
        <button class="ficha-icon-btn wa" id="btnWA" onclick="abrirWhatsApp()" title="WhatsApp" style="display:none">💬</button>
        <button class="ficha-icon-btn" onclick="openEditarCliente()" title="Editar">✏️</button>
        <button class="ficha-icon-btn" onclick="closeFicha()">✕</button>
      </div>
    </div>

    <div class="ficha-stats">
      <div class="ficha-stat">
        <div class="ficha-stat-label">Pagado</div>
        <div class="ficha-stat-value" id="fStatPagado" style="color:var(--green)">0€</div>
      </div>
      <div class="ficha-stat">
        <div class="ficha-stat-label">Sesiones usadas</div>
        <div class="ficha-stat-value" id="fStatUsadas" style="color:#fff">0</div>
      </div>
      <div class="ficha-stat">
        <div class="ficha-stat-label">Quedan</div>
        <div class="ficha-stat-value" id="fStatQuedan" style="color:var(--accent)">0</div>
      </div>
    </div>

    <div class="ficha-actions">
      <button class="action-btn primary" onclick="registrarSesion()">✓ Sesión hoy</button>
      <button class="action-btn secondary" onclick="openPago()">+ Pago</button>
      <button class="action-btn warning" onclick="deshacerUltimaSesion()">↩ Deshacer</button>
      <button class="action-btn danger" onclick="eliminarCliente()">🗑</button>
    </div>

    <div class="ficha-section">
      <div class="ficha-section-title">Historial</div>
      <div id="fichaHistorial"></div>
    </div>
  </div>
</div>

<!-- MODAL NUEVO CLIENTE -->
<div class="mini-overlay" id="overlayNuevo">
  <div class="mini-modal">
    <div class="mini-title">Nuevo cliente</div>
    <div class="field"><label>Nombre</label><input id="nNombre" placeholder="Nombre del cliente"></div>
    <div class="field-row">
      <div class="field"><label>Precio/sesión (€)</label><input id="nPrecio" type="number" placeholder="26" value="26"></div>
      <div class="field"><label>Teléfono</label><input id="nTelefono" type="tel" placeholder="612345678"></div>
    </div>
    <div class="field"><label>Pago inicial (€) — opcional</label><input id="nPago" type="number" placeholder="0"></div>
    <div class="mini-actions">
      <button class="btn-cancel" onclick="closeOverlay('overlayNuevo')">Cancelar</button>
      <button class="btn-primary" onclick="guardarNuevoCliente()">Añadir</button>
    </div>
  </div>
</div>

<!-- MODAL EDITAR CLIENTE -->
<div class="mini-overlay" id="overlayEditar">
  <div class="mini-modal">
    <div class="mini-title">Editar cliente</div>
    <div class="field"><label>Nombre</label><input id="eNombre" placeholder="Nombre"></div>
    <div class="field-row">
      <div class="field"><label>Precio/sesión (€)</label><input id="ePrecio" type="number"></div>
      <div class="field"><label>Teléfono</label><input id="eTelefono" type="tel" placeholder="612345678"></div>
    </div>
    <div class="mini-actions">
      <button class="btn-cancel" onclick="closeOverlay('overlayEditar')">Cancelar</button>
      <button class="btn-primary" onclick="guardarEdicion()">Guardar</button>
    </div>
  </div>
</div>

<!-- MODAL AÑADIR PAGO -->
<div class="mini-overlay" id="overlayPago">
  <div class="mini-modal">
    <div class="mini-title">Registrar pago</div>
    <div class="field"><label>Cantidad (€)</label><input id="pCantidad" type="number" placeholder="0"></div>
    <div class="field"><label>Nota — opcional</label><input id="pNota" placeholder="ej: Bono 10 sesiones"></div>
    <div class="mini-actions">
      <button class="btn-cancel" onclick="closeOverlay('overlayPago')">Cancelar</button>
      <button class="btn-primary" onclick="guardarPago()">Guardar</button>
    </div>
  </div>
</div>

<script>
  let clientes = JSON.parse(localStorage.getItem('pf_clientes') || '[]');
  let activeId = null;

  function save() { localStorage.setItem('pf_clientes', JSON.stringify(clientes)); }
  function getCliente(id) { return clientes.find(c => c.id === id); }

  function calcStats(c) {
    const totalPagado = c.pagos.reduce((s, p) => s + p.cantidad, 0);
    const sesionesCompradas = Math.floor(totalPagado / c.precio);
    const sesionesUsadas = c.sesiones.length;
    const sesionesRestantes = sesionesCompradas - sesionesUsadas;
    return { totalPagado, sesionesCompradas, sesionesUsadas, sesionesRestantes };
  }

  function colorClass(r) { return r <= 0 ? 'red' : r <= 2 ? 'orange' : 'green'; }

  function today() {
    return new Date().toLocaleDateString('es-ES', { day: '2-digit', month: '2-digit', year: 'numeric' });
  }

  function renderList() {
    const q = document.getElementById('search').value.toLowerCase();
    const filtered = clientes.filter(c => c.nombre.toLowerCase().includes(q));

    const totalCobrado = clientes.reduce((s, c) => s + c.pagos.reduce((a, p) => a + p.cantidad, 0), 0);
    const sinSesiones = clientes.filter(c => calcStats(c).sesionesRestantes <= 0).length;
    document.getElementById('gClientes').textContent = clientes.length;
    document.getElementById('gCobrado').textContent = totalCobrado + '€';
    document.getElementById('gSinSesiones').textContent = sinSesiones;

    // Alertas
    const alertasEl = document.getElementById('alertas');
    const sinSes = clientes.filter(c => calcStats(c).sesionesRestantes <= 0);
    const pocasSes = clientes.filter(c => { const r = calcStats(c).sesionesRestantes; return r > 0 && r <= 2; });
    let alertasHTML = '';
    sinSes.forEach(c => {
      const wa = c.telefono ? `onclick="openFicha('${c.id}')"` : `onclick="openFicha('${c.id}')"`;
      alertasHTML += `<div class="alerta red" ${wa}>
        <div class="alerta-dot"></div>
        <div class="alerta-text"><strong>${c.nombre}</strong> — sin sesiones disponibles</div>
        <span class="alerta-badge">Avisar</span>
      </div>`;
    });
    pocasSes.forEach(c => {
      const r = calcStats(c).sesionesRestantes;
      alertasHTML += `<div class="alerta orange" onclick="openFicha('${c.id}')">
        <div class="alerta-dot"></div>
        <div class="alerta-text"><strong>${c.nombre}</strong> — le quedan ${r} sesión${r > 1 ? 'es' : ''}</div>
        <span class="alerta-badge">Pronto</span>
      </div>`;
    });
    alertasEl.innerHTML = alertasHTML;

    const sorted = [...filtered].sort((a, b) => calcStats(a).sesionesRestantes - calcStats(b).sesionesRestantes);
    const list = document.getElementById('clientList');
    const empty = document.getElementById('emptyState');
    document.getElementById('sectionLabel').textContent = `Todos los clientes (${filtered.length})`;

    if (sorted.length === 0) { list.innerHTML = ''; empty.style.display = 'block'; return; }
    empty.style.display = 'none';
    list.innerHTML = sorted.map(c => {
      const { totalPagado, sesionesRestantes, sesionesUsadas } = calcStats(c);
      const col = colorClass(sesionesRestantes);
      const urgentClass = col === 'red' ? 'urgent-red' : col === 'orange' ? 'urgent-orange' : '';
      return `<div class="client-row ${urgentClass}" onclick="openFicha('${c.id}')">
        <div class="sessions-bubble ${col}">${sesionesRestantes}</div>
        <div class="client-info">
          <div class="client-name">${c.nombre}</div>
          <div class="client-meta">${sesionesUsadas} usadas · <span>${totalPagado}€</span> pagado · <span>${c.precio}€</span>/sesión</div>
        </div>
        <div class="client-arrow">›</div>
      </div>`;
    }).join('');
  }

  function openFicha(id) {
    activeId = id;
    renderFicha();
    document.getElementById('fichaOverlay').classList.add('open');
  }

  function renderFicha() {
    const c = getCliente(activeId);
    if (!c) return;
    const { totalPagado, sesionesRestantes, sesionesUsadas, sesionesCompradas } = calcStats(c);
    const col = colorClass(sesionesRestantes);

    const bubble = document.getElementById('fichaBubble');
    bubble.textContent = sesionesRestantes;
    bubble.className = `ficha-bubble ${col}`;

    document.getElementById('fichaNombre').textContent = c.nombre;
    document.getElementById('fichaSub').textContent = `${c.precio}€/sesión · ${sesionesCompradas} sesiones compradas${c.telefono ? ' · ' + c.telefono : ''}`;
    document.getElementById('fStatPagado').textContent = totalPagado + '€';
    document.getElementById('fStatUsadas').textContent = sesionesUsadas;
    document.getElementById('fStatQuedan').textContent = sesionesRestantes;
    document.getElementById('fStatQuedan').style.color = col === 'green' ? 'var(--accent)' : col === 'orange' ? 'var(--orange)' : 'var(--red)';

    // Botón WhatsApp — solo visible si tiene teléfono
    const btnWA = document.getElementById('btnWA');
    btnWA.style.display = c.telefono ? 'flex' : 'none';

    // Historial
    const eventos = [
      ...c.pagos.map((p, i) => ({ tipo: 'pago', idx: i, fecha: p.fecha, cantidad: p.cantidad, nota: p.nota || '' })),
      ...c.sesiones.map((s, i) => ({ tipo: 'sesion', idx: i, fecha: s.fecha, nota: s.nota || '' }))
    ].sort((a, b) => b.fecha.localeCompare(a.fecha));

    const hist = document.getElementById('fichaHistorial');
    if (eventos.length === 0) { hist.innerHTML = '<div class="log-empty">Sin actividad todavía.</div>'; return; }
    hist.innerHTML = eventos.map(e => {
      if (e.tipo === 'pago') return `<div class="log-item">
        <div class="log-icon pago">💳</div>
        <div class="log-text">Pago${e.nota ? ' · <span style="color:var(--mid);font-size:12px">' + e.nota + '</span>' : ''}<br><span class="log-date">${e.fecha}</span></div>
        <div class="log-amount">+${e.cantidad}€</div>
      </div>`;
      return `<div class="log-item">
        <div class="log-icon sesion">🏋️</div>
        <div class="log-text">Sesión<br><span class="log-date">${e.fecha}</span></div>
        <button class="log-del" onclick="eliminarSesion(${e.idx})" title="Eliminar esta sesión">✕</button>
      </div>`;
    }).join('');
  }

  function closeFicha() { document.getElementById('fichaOverlay').classList.remove('open'); }
  function closeFichaIfBg(e) { if (e.target === document.getElementById('fichaOverlay')) closeFicha(); }

  function registrarSesion() {
    const c = getCliente(activeId);
    if (calcStats(c).sesionesRestantes <= 0) {
      if (confirm(`${c.nombre} no tiene sesiones disponibles.\n¿Quieres abrirle WhatsApp para avisarle?`)) abrirWhatsApp();
      return;
    }
    c.sesiones.push({ fecha: today() });
    save(); renderFicha(); renderList();
  }

  function deshacerUltimaSesion() {
    const c = getCliente(activeId);
    if (c.sesiones.length === 0) { alert('No hay sesiones que deshacer.'); return; }
    const ultima = c.sesiones[c.sesiones.length - 1];
    if (!confirm(`¿Eliminar la última sesión (${ultima.fecha})?`)) return;
    c.sesiones.pop();
    save(); renderFicha(); renderList();
  }

  function eliminarSesion(idx) {
    const c = getCliente(activeId);
    if (!confirm('¿Eliminar esta sesión?')) return;
    c.sesiones.splice(idx, 1);
    save(); renderFicha(); renderList();
  }

  function abrirWhatsApp() {
    const c = getCliente(activeId);
    if (!c.telefono) return;
    const tel = c.telefono.replace(/\s/g, '').replace(/^0/, '34');
    const msg = encodeURIComponent(`Hola ${c.nombre.split(' ')[0]} 👋, te escribo desde Personal Fit. Ya has agotado tus sesiones, cuando quieras renovamos el bono 💪`);
    window.open(`https://wa.me/34${tel}?text=${msg}`, '_blank');
  }

  function openPago() {
    document.getElementById('pCantidad').value = '';
    document.getElementById('pNota').value = '';
    document.getElementById('overlayPago').classList.add('open');
    setTimeout(() => document.getElementById('pCantidad').focus(), 100);
  }

  function guardarPago() {
    const cantidad = parseFloat(document.getElementById('pCantidad').value);
    if (!cantidad || cantidad <= 0) return;
    const c = getCliente(activeId);
    c.pagos.push({ fecha: today(), cantidad, nota: document.getElementById('pNota').value.trim() });
    save(); closeOverlay('overlayPago'); renderFicha(); renderList();
  }

  function openEditarCliente() {
    const c = getCliente(activeId);
    document.getElementById('eNombre').value = c.nombre;
    document.getElementById('ePrecio').value = c.precio;
    document.getElementById('eTelefono').value = c.telefono || '';
    document.getElementById('overlayEditar').classList.add('open');
    setTimeout(() => document.getElementById('eNombre').focus(), 100);
  }

  function guardarEdicion() {
    const nombre = document.getElementById('eNombre').value.trim();
    if (!nombre) return;
    const c = getCliente(activeId);
    c.nombre = nombre;
    c.precio = parseFloat(document.getElementById('ePrecio').value) || c.precio;
    c.telefono = document.getElementById('eTelefono').value.trim();
    save(); closeOverlay('overlayEditar'); renderFicha(); renderList();
  }

  function eliminarCliente() {
    if (!confirm(`¿Eliminar a ${getCliente(activeId).nombre}?`)) return;
    clientes = clientes.filter(c => c.id !== activeId);
    save(); closeFicha(); renderList();
  }

  function openNuevoCliente() {
    ['nNombre','nPago','nTelefono'].forEach(id => document.getElementById(id).value = '');
    document.getElementById('nPrecio').value = '26';
    document.getElementById('overlayNuevo').classList.add('open');
    setTimeout(() => document.getElementById('nNombre').focus(), 100);
  }

  function guardarNuevoCliente() {
    const nombre = document.getElementById('nNombre').value.trim();
    if (!nombre) return;
    const precio = parseFloat(document.getElementById('nPrecio').value) || 26;
    const pagoInicial = parseFloat(document.getElementById('nPago').value) || 0;
    const telefono = document.getElementById('nTelefono').value.trim();
    const cliente = { id: Date.now().toString(), nombre, precio, telefono, pagos: [], sesiones: [] };
    if (pagoInicial > 0) cliente.pagos.push({ fecha: today(), cantidad: pagoInicial, nota: 'Pago inicial' });
    clientes.push(cliente);
    save(); closeOverlay('overlayNuevo'); renderList();
  }

  function closeOverlay(id) { document.getElementById(id).classList.remove('open'); }

  function exportarDatos() {
    const datos = { version: 1, fecha: today(), clientes };
    const blob = new Blob([JSON.stringify(datos, null, 2)], { type: 'application/json' });
    const a = document.createElement('a');
    a.href = URL.createObjectURL(blob);
    a.download = `personalfit-backup-${new Date().toISOString().slice(0,10)}.json`;
    a.click();
  }

  function importarDatos() { document.getElementById('fileInput').click(); }

  function cargarArchivo(e) {
    const file = e.target.files[0];
    if (!file) return;
    const reader = new FileReader();
    reader.onload = ev => {
      try {
        const datos = JSON.parse(ev.target.result);
        if (!datos.clientes) throw new Error();
        if (!confirm(`¿Cargar backup del ${datos.fecha}? Se reemplazarán los datos actuales.`)) return;
        clientes = datos.clientes;
        save(); renderList();
      } catch { alert('Archivo no válido.'); }
    };
    reader.readAsText(file);
    e.target.value = '';
  }

  ['overlayNuevo','overlayEditar','overlayPago'].forEach(id => {
    document.getElementById(id).addEventListener('click', e => { if (e.target === e.currentTarget) closeOverlay(id); });
  });

  renderList();
</script>
</body>
</html>
