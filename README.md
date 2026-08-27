[frota-ibgp_7.html](https://github.com/user-attachments/files/31517233/frota-ibgp_7.html)
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Frota IBGP — Controle de Saída e Retorno</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Barlow+Semi+Condensed:wght@600;700;800&family=Inter:wght@400;500;600&family=IBM+Plex+Mono:wght@400;500;600&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2/dist/umd/supabase.min.js"></script>
<style>
  :root{
    --bg: #14161a;
    --panel: #1d2027;
    --panel-2: #242830;
    --border: #343a44;
    --accent: #f2a33d;
    --accent-2: #ffcf5c;
    --good: #4fae7e;
    --good-bg: rgba(79,174,126,0.12);
    --warn: #e2574c;
    --text: #ece9e3;
    --muted: #8d93a0;
    --display: 'Barlow Semi Condensed', sans-serif;
    --body: 'Inter', sans-serif;
    --mono: 'IBM Plex Mono', monospace;
  }
  *{box-sizing:border-box;}
  body{
    margin:0;
    background:var(--bg);
    color:var(--text);
    font-family:var(--body);
    -webkit-font-smoothing:antialiased;
  }
  .wrap{
    max-width:920px;
    margin:0 auto;
    padding:28px 20px 64px;
  }

  /* ---- header / dispatch board ---- */
  .config-banner{
    background:rgba(226,87,76,0.12);
    border:1px solid rgba(226,87,76,0.4);
    color:#ff9188;
    border-radius:8px;
    padding:12px 16px;
    font-size:13px;
    line-height:1.6;
    margin-bottom:16px;
  }
  .config-banner code{
    background:rgba(0,0,0,0.25);
    padding:1px 6px;
    border-radius:4px;
    font-family:var(--mono);
    font-size:12px;
  }

  .board-head{
    border:1px solid var(--border);
    background:
      repeating-linear-gradient(135deg, rgba(242,163,61,0.035) 0 2px, transparent 2px 8px),
      var(--panel);
    border-radius:10px;
    padding:22px 24px;
    position:relative;
    overflow:hidden;
    margin-bottom:22px;
  }
  .board-head::before{
    content:"";
    position:absolute; left:0; top:0; bottom:0; width:6px;
    background:var(--accent);
  }
  .logo-badge{
    position:absolute;
    top:20px;
    right:22px;
    background:#fff;
    border-radius:8px;
    padding:8px 12px;
    display:flex;
    align-items:center;
    box-shadow:0 2px 10px rgba(0,0,0,0.25);
  }
  .logo-badge img{
    height:32px;
    width:auto;
    display:block;
  }
  @media (max-width:560px){
    .logo-badge{ position:static; margin-bottom:14px; display:inline-flex; }
  }
  .eyebrow{
    font-family:var(--mono);
    font-size:11px;
    letter-spacing:.14em;
    color:var(--accent);
    text-transform:uppercase;
    margin:0 0 6px;
    display:flex; align-items:center; gap:8px;
  }
  .eyebrow .dot{width:6px;height:6px;border-radius:50%;background:var(--accent);display:inline-block;}
  h1{
    font-family:var(--display);
    font-weight:800;
    text-transform:uppercase;
    letter-spacing:.02em;
    font-size:clamp(28px,5vw,38px);
    margin:0 0 4px;
    line-height:1;
  }
  .sub{
    color:var(--muted);
    font-size:14px;
    margin:0;
  }
  .board-meta{
    display:flex;
    gap:22px;
    margin-top:18px;
    flex-wrap:wrap;
  }
  .meta-item{
    font-family:var(--mono);
  }
  .meta-item .num{
    font-size:22px;
    font-weight:600;
    color:var(--text);
    display:block;
    line-height:1.1;
  }
  .meta-item .lbl{
    font-size:10px;
    letter-spacing:.1em;
    text-transform:uppercase;
    color:var(--muted);
  }
  .num.accent-num{color:var(--accent);}
  .num.good-num{color:var(--good);}

  /* ---- tabs ---- */
  .tabs{
    display:flex;
    gap:2px;
    margin-bottom:0;
    border-radius:10px 10px 0 0;
    overflow:hidden;
    border:1px solid var(--border);
    border-bottom:none;
  }
  .tab-btn{
    flex:1;
    padding:13px 16px;
    background:var(--panel-2);
    color:var(--muted);
    border:none;
    font-family:var(--display);
    font-weight:700;
    letter-spacing:.03em;
    text-transform:uppercase;
    font-size:14px;
    cursor:pointer;
    transition:background .15s, color .15s;
  }
  .tab-btn:hover{color:var(--text);}
  .tab-btn.active{
    background:var(--accent);
    color:#1a1500;
  }

  .panel{
    border:1px solid var(--border);
    border-radius:0 0 10px 10px;
    background:var(--panel);
    padding:22px;
    margin-bottom:26px;
  }

  form{display:grid; gap:14px;}
  .row2{display:grid; grid-template-columns:1fr 1fr; gap:14px;}
  @media (max-width:560px){ .row2{grid-template-columns:1fr;} }

  label{
    font-size:11px;
    text-transform:uppercase;
    letter-spacing:.08em;
    color:var(--muted);
    display:block;
    margin-bottom:6px;
  }
  input, textarea, select{
    width:100%;
    background:var(--panel-2);
    border:1px solid var(--border);
    color:var(--text);
    border-radius:6px;
    padding:10px 12px;
    font-family:var(--body);
    font-size:14px;
  }
  input:focus, textarea:focus, select:focus{
    outline:2px solid var(--accent);
    outline-offset:1px;
    border-color:var(--accent);
  }
  textarea{resize:vertical; min-height:56px;}
  .field-mono input{font-family:var(--mono);}

  .submit-btn{
    justify-self:start;
    background:var(--accent);
    color:#1a1500;
    border:none;
    font-family:var(--display);
    font-weight:800;
    text-transform:uppercase;
    letter-spacing:.04em;
    font-size:15px;
    padding:12px 26px;
    border-radius:6px;
    cursor:pointer;
    transition:background .15s, transform .1s;
  }
  .submit-btn:hover{background:var(--accent-2);}
  .submit-btn:active{transform:scale(.98);}
  .submit-btn:disabled{opacity:.4; cursor:not-allowed;}

  /* ---- fuel gauge ---- */
  .fuel-gauge{
    display:flex;
    border:1px solid var(--border);
    border-radius:8px;
    overflow:hidden;
  }
  .fuel-seg{
    flex:1;
    background:var(--panel-2);
    border:none;
    border-right:1px solid var(--border);
    color:var(--muted);
    font-family:var(--mono);
    font-size:13px;
    font-weight:600;
    padding:11px 4px;
    cursor:pointer;
    transition:background .15s, color .15s;
  }
  .fuel-seg:last-child{border-right:none;}
  .fuel-seg:hover{color:var(--text); background:#2b303a;}
  .fuel-seg.active{
    background:var(--accent);
    color:#1a1500;
  }
  .fuel-seg[data-value="E"].active{background:var(--warn); color:#fff;}
  .fuel-seg[data-value="1/4"].active{background:#e08a3d; color:#1a1500;}
  .fuel-seg[data-value="1/2"].active{background:var(--accent); color:#1a1500;}
  .fuel-seg[data-value="3/4"].active{background:#8fc98a; color:#132312;}
  .fuel-seg[data-value="F"].active{background:var(--good); color:#0e2018;}
  .fuel-readout{
    font-family:var(--mono);
    font-size:11px;
    color:var(--muted);
    margin-top:6px;
  }

  /* ---- photo capture ---- */
  .photo-field{
    border:1px dashed var(--border);
    border-radius:8px;
    padding:14px;
    background:var(--panel-2);
  }
  .photo-actions{
    display:flex;
    align-items:center;
    gap:12px;
    flex-wrap:wrap;
  }
  .photo-btn{
    background:none;
    border:1px solid var(--accent);
    color:var(--accent);
    border-radius:6px;
    padding:9px 16px;
    font-family:var(--body);
    font-weight:600;
    font-size:13px;
    cursor:pointer;
    display:inline-flex;
    align-items:center;
    gap:7px;
  }
  .photo-btn:hover{background:rgba(242,163,61,0.12);}
  .photo-hint{font-size:12px; color:var(--muted);}
  .photo-preview-wrap{
    margin-top:12px;
    display:none;
    align-items:center;
    gap:12px;
  }
  .photo-preview-wrap.show{display:flex;}
  .photo-preview-wrap img{
    width:88px;
    height:88px;
    object-fit:cover;
    border-radius:6px;
    border:1px solid var(--border);
    cursor:pointer;
  }
  .photo-remove{
    background:none;
    border:none;
    color:var(--warn);
    font-size:12px;
    text-decoration:underline;
    cursor:pointer;
  }

  .ticket-photo{
    width:100%;
    max-height:130px;
    object-fit:cover;
    border-radius:6px;
    margin-top:10px;
    border:1px solid var(--border);
    cursor:pointer;
    display:block;
  }

  .lightbox{
    position:fixed;
    inset:0;
    background:rgba(10,11,13,0.9);
    display:none;
    align-items:center;
    justify-content:center;
    z-index:100;
    padding:24px;
    cursor:zoom-out;
  }
  .lightbox.open{display:flex;}
  .lightbox img{
    max-width:100%;
    max-height:100%;
    border-radius:8px;
    box-shadow:0 10px 40px rgba(0,0,0,0.5);
  }

  .empty-note{
    color:var(--muted);
    font-size:14px;
    font-style:italic;
  }

  .veiculos-list{
    margin-top:20px;
    padding-top:18px;
    border-top:1px solid var(--border);
    display:grid;
    gap:10px;
  }
  .veiculo-card{
    display:flex;
    justify-content:space-between;
    align-items:center;
    gap:12px;
    background:var(--panel-2);
    border:1px solid var(--border);
    border-radius:8px;
    padding:12px 14px;
  }
  .veiculo-info .placa{
    font-family:var(--mono);
    font-size:15px;
    color:var(--accent-2);
    font-weight:600;
    letter-spacing:.03em;
  }
  .veiculo-info .modelo{
    font-size:13px;
    color:var(--text);
    margin-top:2px;
  }
  .veiculo-info .detalhe{
    font-size:11.5px;
    color:var(--muted);
    margin-top:2px;
  }
  .veiculo-remove{
    background:none;
    border:1px solid var(--border);
    color:var(--muted);
    border-radius:6px;
    padding:6px 12px;
    font-size:12px;
    cursor:pointer;
    flex-shrink:0;
  }
  .veiculo-remove:hover{border-color:var(--warn); color:var(--warn);}

  .agendamentos-list{
    margin-top:20px;
    padding-top:18px;
    border-top:1px solid var(--border);
    display:grid;
    gap:10px;
  }
  .agendamento-card{
    display:flex;
    justify-content:space-between;
    align-items:flex-start;
    gap:12px;
    background:var(--panel-2);
    border:1px solid var(--border);
    border-radius:8px;
    padding:12px 14px;
  }
  .agendamento-card.past{opacity:.45;}
  .agendamento-info .veiculo{
    font-family:var(--mono);
    font-size:14px;
    color:var(--accent-2);
    font-weight:600;
    letter-spacing:.02em;
  }
  .agendamento-info .periodo{
    font-family:var(--mono);
    font-size:12.5px;
    color:var(--text);
    margin-top:4px;
  }
  .agendamento-info .funcionario{
    font-size:13px;
    color:var(--muted);
    margin-top:3px;
  }
  .agendamento-info .motivo{
    font-size:12.5px;
    color:var(--muted);
    margin-top:6px;
    line-height:1.4;
    background:var(--panel);
    border-radius:6px;
    padding:6px 9px;
  }
  .agendamento-remove{
    background:none;
    border:1px solid var(--border);
    color:var(--muted);
    border-radius:6px;
    padding:6px 12px;
    font-size:12px;
    cursor:pointer;
    flex-shrink:0;
    white-space:nowrap;
  }
  .agendamento-remove:hover{border-color:var(--warn); color:var(--warn);}

  .manutencoes-list{
    margin-top:20px;
    padding-top:18px;
    border-top:1px solid var(--border);
    display:grid;
    gap:10px;
  }
  .manutencao-card{
    display:flex;
    justify-content:space-between;
    align-items:flex-start;
    gap:12px;
    background:var(--panel-2);
    border:1px solid var(--border);
    border-radius:8px;
    padding:12px 14px;
  }
  .manutencao-info .topo{
    display:flex;
    align-items:center;
    gap:8px;
    flex-wrap:wrap;
  }
  .manutencao-info .veiculo{
    font-family:var(--mono);
    font-size:14px;
    color:var(--accent-2);
    font-weight:600;
    letter-spacing:.02em;
  }
  .manutencao-tipo{
    font-size:11px;
    font-weight:600;
    text-transform:uppercase;
    letter-spacing:.04em;
    color:var(--accent-2);
    background:var(--panel);
    border:1px solid var(--border);
    border-radius:99px;
    padding:2px 9px;
  }
  .manutencao-info .data{
    font-family:var(--mono);
    font-size:12.5px;
    color:var(--text);
    margin-top:4px;
  }
  .manutencao-info .meta{
    font-size:12.5px;
    color:var(--muted);
    margin-top:3px;
  }
  .manutencao-info .descricao{
    font-size:12.5px;
    color:var(--muted);
    margin-top:6px;
    line-height:1.4;
    background:var(--panel);
    border-radius:6px;
    padding:6px 9px;
  }
  .manutencao-remove{
    background:none;
    border:1px solid var(--border);
    color:var(--muted);
    border-radius:6px;
    padding:6px 12px;
    font-size:12px;
    cursor:pointer;
    flex-shrink:0;
    white-space:nowrap;
  }
  .manutencao-remove:hover{border-color:var(--warn); color:var(--warn);}
  .msg{
    font-size:13px;
    padding:9px 12px;
    border-radius:6px;
    margin-top:2px;
  }
  .msg.err{background:rgba(226,87,76,0.12); color:#ff9188; border:1px solid rgba(226,87,76,0.3);}
  .msg.ok{background:var(--good-bg); color:var(--good); border:1px solid rgba(79,174,126,0.3);}

  /* ---- ledger section ---- */
  .ledger-head{
    display:flex;
    justify-content:space-between;
    align-items:baseline;
    margin-bottom:14px;
    flex-wrap:wrap;
    gap:8px;
  }
  .ledger-head h2{
    font-family:var(--display);
    text-transform:uppercase;
    font-weight:700;
    letter-spacing:.03em;
    font-size:20px;
    margin:0;
  }
  .reset-link{
    background:none;
    border:none;
    color:var(--muted);
    font-size:12px;
    text-decoration:underline;
    cursor:pointer;
    font-family:var(--body);
  }
  .reset-link:hover{color:var(--warn);}

  .sync-status{
    display:inline-flex;
    align-items:center;
    gap:6px;
    font-family:var(--mono);
    font-size:11px;
    color:var(--muted);
  }
  .sync-dot{
    width:8px;
    height:8px;
    border-radius:50%;
    background:var(--good);
    display:inline-block;
    animation:pulse-dot 2s infinite;
  }
  @keyframes pulse-dot{
    0%{ box-shadow:0 0 0 0 rgba(79,174,126,0.5); }
    70%{ box-shadow:0 0 0 6px rgba(79,174,126,0); }
    100%{ box-shadow:0 0 0 0 rgba(79,174,126,0); }
  }

  /* ticket stub */
  .ticket{
    position:relative;
    display:grid;
    grid-template-columns:1fr 1fr;
    border:1px solid var(--border);
    border-radius:10px;
    background:var(--panel);
    margin-bottom:16px;
    overflow:hidden;
  }
  @media (max-width:620px){ .ticket{grid-template-columns:1fr;} }

  .ticket-half{padding:16px 18px;}
  .ticket-half.saida{border-right:1px dashed var(--border);}
  @media (max-width:620px){ .ticket-half.saida{border-right:none; border-bottom:1px dashed var(--border);} }

  /* perforation notches */
  .ticket-half.saida::after{
    content:"";
    position:absolute;
    top:50%; right:-9px;
    transform:translateY(-50%);
    width:18px; height:18px;
    border-radius:50%;
    background:var(--bg);
    border:1px solid var(--border);
  }
  @media (max-width:620px){
    .ticket-half.saida::after{
      right:50%; top:auto; bottom:-9px; transform:translateX(50%);
    }
  }

  .ticket-top{
    display:flex;
    justify-content:space-between;
    align-items:center;
    margin-bottom:10px;
  }
  .badge{
    font-family:var(--mono);
    font-size:10px;
    letter-spacing:.1em;
    text-transform:uppercase;
    padding:4px 9px;
    border-radius:20px;
  }
  .badge.open{background:rgba(242,163,61,0.15); color:var(--accent);}
  .badge.closed{background:var(--good-bg); color:var(--good);}
  .ticket-id{font-family:var(--mono); font-size:11px; color:var(--muted);}

  .ticket-name{
    font-family:var(--display);
    font-weight:700;
    font-size:18px;
    text-transform:uppercase;
    margin:0 0 2px;
  }
  .ticket-vehicle{
    font-family:var(--mono);
    color:var(--accent-2);
    font-size:13px;
    margin:0 0 12px;
  }
  .dl{display:grid; gap:7px;}
  .dl-row{display:flex; justify-content:space-between; gap:10px; font-size:13px;}
  .dl-row .k{color:var(--muted);}
  .dl-row .v{font-family:var(--mono); text-align:right;}
  .route-note{
    margin-top:10px;
    font-size:13px;
    color:var(--text);
    background:var(--panel-2);
    border-radius:6px;
    padding:8px 10px;
    line-height:1.4;
  }
  .route-note .k{
    display:block;
    color:var(--muted);
    font-size:10px;
    text-transform:uppercase;
    letter-spacing:.08em;
    margin-bottom:3px;
    font-family:var(--mono);
  }
  .waiting{
    color:var(--muted);
    font-style:italic;
    font-size:13px;
  }
  .km-total{
    margin-top:12px;
    padding-top:10px;
    border-top:1px solid var(--border);
    display:flex;
    justify-content:space-between;
    font-family:var(--mono);
    font-size:13px;
  }
  .km-total .v{color:var(--accent-2); font-weight:600;}

  /* ---- qr access ---- */
  .qr-card{
    margin-top:18px;
    padding-top:16px;
    border-top:1px dashed var(--border);
    display:flex;
    gap:16px;
    align-items:center;
    flex-wrap:wrap;
  }
  .qr-box{
    background:#fff;
    padding:8px;
    border-radius:8px;
    width:96px;
    height:96px;
    display:flex;
    align-items:center;
    justify-content:center;
    flex-shrink:0;
  }
  .qr-box img, .qr-box canvas{width:80px; height:80px;}
  .qr-info{flex:1; min-width:200px;}
  .qr-info .lbl{
    font-family:var(--mono);
    font-size:10px;
    letter-spacing:.1em;
    text-transform:uppercase;
    color:var(--accent);
    margin:0 0 4px;
  }
  .qr-info p{margin:0 0 8px; font-size:12.5px; color:var(--muted); line-height:1.5;}
  .qr-url-row{display:flex; gap:8px; flex-wrap:wrap;}
  .qr-url{
    font-family:var(--mono);
    font-size:11px;
    color:var(--text);
    background:var(--panel-2);
    border:1px solid var(--border);
    border-radius:5px;
    padding:6px 9px;
    word-break:break-all;
    flex:1;
    min-width:160px;
  }
  .copy-btn{
    background:none;
    border:1px solid var(--border);
    color:var(--muted);
    border-radius:5px;
    padding:6px 11px;
    font-size:11px;
    font-family:var(--body);
    cursor:pointer;
  }
  .copy-btn:hover{border-color:var(--accent); color:var(--accent);}

  .loading{color:var(--muted); font-size:13px; padding:20px 0;}
  .footnote{
    color:var(--muted);
    font-size:11px;
    margin-top:28px;
    text-align:center;
  }
</style>
</head>
<body>
<div class="wrap">

  <div class="config-banner" id="configBanner" style="display:none;">
    <strong>⚠ Supabase não configurado.</strong> Abra o arquivo HTML em um editor de texto, localize <code>SUPABASE_URL</code> e <code>SUPABASE_ANON_KEY</code> no início do <code>&lt;script&gt;</code> e cole os dados do seu projeto Supabase. Até lá, o app não vai conseguir salvar nem sincronizar registros.
  </div>

  <div class="board-head">
    <div class="logo-badge"><img src="data:image/png;base64,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" alt="Logo IBGP"></div>
    <p class="eyebrow"><span class="dot"></span>Controle de frota</p>
    <h1>Frota IBGP</h1>
    <p class="sub">Registro de saída e retorno de veículos da empresa</p>
    <div class="board-meta">
      <div class="meta-item"><span class="num accent-num" id="statOpen">–</span><span class="lbl">Em rota</span></div>
      <div class="meta-item"><span class="num good-num" id="statClosed">–</span><span class="lbl">Retornados</span></div>
      <div class="meta-item"><span class="num" id="statTotal">–</span><span class="lbl">Total de registros</span></div>
      <div class="meta-item"><span class="num" id="statVeiculos">–</span><span class="lbl">Veículos cadastrados</span></div>
      <div class="meta-item"><span class="num accent-num" id="statAgendamentos">–</span><span class="lbl">Agendamentos futuros</span></div>
    </div>
    <div class="qr-card">
      <div class="qr-box" id="qrBox"></div>
      <div class="qr-info">
        <p class="lbl">Acesso rápido</p>
        <p>Aponte a câmera do celular para abrir este app neste endereço. Se este link não puder ser aberto em outro dispositivo, publique o arquivo em algum lugar acessível (link compartilhado, intranet, Google Drive) — o QR sempre aponta para o endereço atual da página.</p>
        <div class="qr-url-row">
          <span class="qr-url" id="qrUrl">—</span>
          <button class="copy-btn" id="copyUrlBtn">Copiar link</button>
        </div>
      </div>
    </div>
  </div>

  <div class="tabs">
    <button class="tab-btn active" id="tabAgendamentoBtn">Agendamento</button>
    <button class="tab-btn" id="tabSaidaBtn">Nova saída</button>
    <button class="tab-btn" id="tabRetornoBtn">Registrar retorno</button>
    <button class="tab-btn" id="tabVeiculosBtn">Veículos</button>
    <button class="tab-btn" id="tabManutencaoBtn">Manutenções</button>
  </div>

  <div class="panel" id="panelAgendamento">
    <form id="formAgendamento">
      <div class="row2">
        <div>
          <label for="fAgFuncionario">Funcionário</label>
          <input type="text" id="fAgFuncionario" placeholder="Nome do funcionário" required>
        </div>
        <div>
          <label for="fAgVeiculo">Veículo</label>
          <select id="fAgVeiculo" required></select>
        </div>
      </div>
      <div id="semVeiculoHintAg" class="empty-note" style="display:none; margin:-6px 0 4px;">
        Nenhum veículo cadastrado ainda. Cadastre um na aba <strong>Veículos</strong> antes de agendar.
      </div>
      <div class="row2">
        <div>
          <label for="fAgInicio">Data e hora prevista de retirada</label>
          <input type="datetime-local" id="fAgInicio" required>
        </div>
        <div>
          <label for="fAgFim">Data e hora prevista de devolução</label>
          <input type="datetime-local" id="fAgFim" required>
        </div>
      </div>
      <div>
        <label for="fAgMotivo">Motivo do agendamento</label>
        <textarea id="fAgMotivo" placeholder="Ex: Visita técnica na obra Zona Norte"></textarea>
      </div>
      <div id="msgAgendamento"></div>
      <button type="submit" class="submit-btn">Agendar retirada</button>
    </form>

    <div class="agendamentos-list" id="agendamentosList"></div>
  </div>

  <div class="panel" id="panelSaida" style="display:none;">
    <form id="formSaida">
      <div class="row2">
        <div>
          <label for="fFuncionario">Funcionário</label>
          <input type="text" id="fFuncionario" placeholder="Nome do funcionário" required>
        </div>
        <div>
          <label for="fVeiculo">Veículo</label>
          <select id="fVeiculo" required></select>
        </div>
      </div>
      <div id="semVeiculoHint" class="empty-note" style="display:none; margin:-6px 0 4px;">
        Nenhum veículo cadastrado ainda. Cadastre um na aba <strong>Veículos</strong> antes de registrar a saída.
      </div>
      <div class="row2">
        <div>
          <label for="fSaidaData">Data e hora de saída</label>
          <input type="datetime-local" id="fSaidaData" required>
        </div>
        <div class="field-mono">
          <label for="fSaidaKm">Quilometragem de saída (km)</label>
          <input type="number" id="fSaidaKm" min="0" step="1" placeholder="0" required>
        </div>
      </div>
      <div>
        <label>Nível de combustível (saída)</label>
        <div class="fuel-gauge" id="gaugeSaida">
          <button type="button" class="fuel-seg" data-value="E">E</button>
          <button type="button" class="fuel-seg" data-value="1/4">¼</button>
          <button type="button" class="fuel-seg" data-value="1/2">½</button>
          <button type="button" class="fuel-seg" data-value="3/4">¾</button>
          <button type="button" class="fuel-seg" data-value="F">F</button>
        </div>
        <input type="hidden" id="fCombustivelSaida">
      </div>
      <div>
        <label for="fMotivo">Motivo da retirada</label>
        <textarea id="fMotivo" placeholder="Ex: Entrega de materiais na obra Zona Sul" required></textarea>
      </div>
      <div>
        <label for="fRotaIda">Rota de ida</label>
        <textarea id="fRotaIda" placeholder="Ex: Matriz → Depósito Central → Cliente"></textarea>
      </div>
      <div>
        <label>Foto do veículo (saída)</label>
        <div class="photo-field">
          <div class="photo-actions">
            <button type="button" class="photo-btn" id="btnFotoSaida">📷 Tirar / anexar foto</button>
            <span class="photo-hint">Registra a condição do veículo antes de sair</span>
          </div>
          <input type="file" accept="image/*" capture="environment" id="fFotoSaida" style="display:none;">
          <div class="photo-preview-wrap" id="previewSaidaWrap">
            <img id="previewSaidaImg" alt="Foto do veículo na saída">
            <button type="button" class="photo-remove" id="removeFotoSaida">Remover foto</button>
          </div>
        </div>
      </div>
      <div id="msgSaida"></div>
      <button type="submit" class="submit-btn">Registrar saída</button>
    </form>
  </div>

  <div class="panel" id="panelRetorno" style="display:none;">
    <form id="formRetorno">
      <div>
        <label for="fSelecionar">Veículo em rota</label>
        <select id="fSelecionar" required></select>
      </div>
      <div class="row2">
        <div>
          <label for="fRetornoData">Data e hora de chegada</label>
          <input type="datetime-local" id="fRetornoData" required>
        </div>
        <div class="field-mono">
          <label for="fRetornoKm">Quilometragem de chegada (km)</label>
          <input type="number" id="fRetornoKm" min="0" step="1" placeholder="0" required>
        </div>
      </div>
      <div>
        <label>Nível de combustível (retorno)</label>
        <div class="fuel-gauge" id="gaugeRetorno">
          <button type="button" class="fuel-seg" data-value="E">E</button>
          <button type="button" class="fuel-seg" data-value="1/4">¼</button>
          <button type="button" class="fuel-seg" data-value="1/2">½</button>
          <button type="button" class="fuel-seg" data-value="3/4">¾</button>
          <button type="button" class="fuel-seg" data-value="F">F</button>
        </div>
        <input type="hidden" id="fCombustivelRetorno">
      </div>
      <div>
        <label for="fRotaVolta">Rota de volta</label>
        <textarea id="fRotaVolta" placeholder="Ex: Cliente → Posto de combustível → Matriz"></textarea>
      </div>
      <div>
        <label>Foto do veículo (retorno)</label>
        <div class="photo-field">
          <div class="photo-actions">
            <button type="button" class="photo-btn" id="btnFotoRetorno">📷 Tirar / anexar foto</button>
            <span class="photo-hint">Registra a condição do veículo na chegada</span>
          </div>
          <input type="file" accept="image/*" capture="environment" id="fFotoRetorno" style="display:none;">
          <div class="photo-preview-wrap" id="previewRetornoWrap">
            <img id="previewRetornoImg" alt="Foto do veículo no retorno">
            <button type="button" class="photo-remove" id="removeFotoRetorno">Remover foto</button>
          </div>
        </div>
      </div>
      <div id="msgRetorno"></div>
      <button type="submit" class="submit-btn" id="btnRetorno">Registrar retorno</button>
    </form>
  </div>

  <div class="panel" id="panelVeiculos" style="display:none;">
    <form id="formVeiculo">
      <div class="row2">
        <div>
          <label for="fPlaca">Placa</label>
          <input type="text" id="fPlaca" placeholder="Ex: ABC-1D23" required>
        </div>
        <div>
          <label for="fModelo">Modelo</label>
          <input type="text" id="fModelo" placeholder="Ex: Fiat Strada" required>
        </div>
      </div>
      <div class="row2">
        <div>
          <label for="fMarca">Marca</label>
          <input type="text" id="fMarca" placeholder="Ex: Fiat">
        </div>
        <div>
          <label for="fAnoCor">Ano / cor</label>
          <input type="text" id="fAnoCor" placeholder="Ex: 2023 · Branco">
        </div>
      </div>
      <div id="msgVeiculo"></div>
      <button type="submit" class="submit-btn">Cadastrar veículo</button>
    </form>

    <div class="veiculos-list" id="veiculosList"></div>
  </div>

  <div class="panel" id="panelManutencao" style="display:none;">
    <form id="formManutencao">
      <div class="row2">
        <div>
          <label for="fManVeiculo">Veículo</label>
          <select id="fManVeiculo" required></select>
        </div>
        <div>
          <label for="fManTipo">Tipo de manutenção</label>
          <select id="fManTipo" required>
            <option value="Preventiva">Preventiva</option>
            <option value="Corretiva">Corretiva</option>
            <option value="Revisão">Revisão</option>
            <option value="Troca de óleo">Troca de óleo</option>
            <option value="Pneus">Pneus</option>
            <option value="Outro">Outro</option>
          </select>
        </div>
      </div>
      <div id="semVeiculoHintMan" class="empty-note" style="display:none; margin:-6px 0 4px;">
        Nenhum veículo cadastrado ainda. Cadastre um na aba <strong>Veículos</strong> antes de registrar uma manutenção.
      </div>
      <div class="row2">
        <div>
          <label for="fManData">Data da manutenção</label>
          <input type="date" id="fManData" required>
        </div>
        <div>
          <label for="fManKm">Quilometragem</label>
          <input type="number" id="fManKm" placeholder="Ex: 48200" min="0">
        </div>
      </div>
      <div class="row2">
        <div>
          <label for="fManCusto">Custo (R$)</label>
          <input type="number" id="fManCusto" placeholder="Ex: 350,00" min="0" step="0.01">
        </div>
        <div>
          <label for="fManOficina">Oficina / responsável</label>
          <input type="text" id="fManOficina" placeholder="Ex: Oficina Central">
        </div>
      </div>
      <div>
        <label for="fManDescricao">Descrição do serviço</label>
        <textarea id="fManDescricao" placeholder="Ex: Troca de pastilhas de freio dianteiras e alinhamento"></textarea>
      </div>
      <div>
        <label for="fManProxima">Próxima manutenção prevista (opcional)</label>
        <input type="date" id="fManProxima">
      </div>
      <div id="msgManutencao"></div>
      <button type="submit" class="submit-btn">Registrar manutenção</button>
    </form>

    <div class="manutencoes-list" id="manutencoesList"></div>
  </div>

  <div class="ledger-head">
    <div style="display:flex; align-items:center; gap:12px; flex-wrap:wrap;">
      <h2>Registros</h2>
      <span class="sync-status"><span class="sync-dot" id="syncDot"></span><span id="syncLabel">Sincronizando…</span></span>
    </div>
    <button class="reset-link" id="resetBtn">Limpar todos os registros</button>
  </div>
  <div id="ledger"><p class="loading">Carregando registros…</p></div>

  <p class="footnote">Os registros são compartilhados entre todas as pessoas que acessam este app e são atualizados automaticamente.</p>
</div>
<div class="lightbox" id="lightbox"><img id="lightboxImg" alt="Foto ampliada do veículo"></div>

<script>
// =======================================================
// CONFIGURAÇÃO DO SUPABASE — preencha com os dados do seu projeto
// (Project Settings → API, no painel do Supabase)
// =======================================================
const SUPABASE_URL = 'https://fqspjptscgbmuiqgzgrx.supabase.co';
const SUPABASE_ANON_KEY = 'sb_publishable_JnlpJGN8Qp8c8Wd1GunwWw_O93j4FNh';
// =======================================================

const supaConfigured = SUPABASE_URL.startsWith('http') && SUPABASE_ANON_KEY.length > 10;
const supabaseClient = supaConfigured ? window.supabase.createClient(SUPABASE_URL, SUPABASE_ANON_KEY) : null;

let registros = [];
let veiculos = [];
let agendamentos = [];
let manutencoes = [];

function fmtCombustivel(v){
  const map = {'E':'Reserva (E)', '1/4':'¼ do tanque', '1/2':'½ do tanque', '3/4':'¾ do tanque', 'F':'Cheio (F)'};
  return map[v] || v;
}

function describeError(e){
  if(!e) return 'erro desconhecido';
  const parts = [];
  if(e.message) parts.push(e.message);
  if(e.details) parts.push('Detalhes: ' + e.details);
  if(e.hint) parts.push('Dica: ' + e.hint);
  if(e.code) parts.push('Código: ' + e.code);
  return parts.length ? parts.join(' — ') : JSON.stringify(e);
}

function fmtDateTime(iso){
  if(!iso) return '—';
  const d = new Date(iso);
  return d.toLocaleString('pt-BR', {day:'2-digit', month:'2-digit', year:'numeric', hour:'2-digit', minute:'2-digit'});
}
function nowLocalInputValue(){
  const d = new Date();
  d.setMinutes(d.getMinutes() - d.getTimezoneOffset());
  return d.toISOString().slice(0,16);
}
document.getElementById('fSaidaData').value = nowLocalInputValue();
document.getElementById('fRetornoData').value = nowLocalInputValue();

// ---- mapeamento snake_case (banco) <-> camelCase (app) ----
function rowToRegistro(row){
  return {
    id: row.id,
    funcionario: row.funcionario,
    veiculo: row.veiculo,
    saidaDataHora: row.saida_data_hora,
    saidaKm: row.saida_km,
    combustivelSaida: row.combustivel_saida,
    motivo: row.motivo,
    rotaIda: row.rota_ida,
    fotoSaida: row.foto_saida,
    retornoDataHora: row.retorno_data_hora,
    retornoKm: row.retorno_km,
    combustivelRetorno: row.combustivel_retorno,
    rotaVolta: row.rota_volta,
    fotoRetorno: row.foto_retorno,
    status: row.status
  };
}
function rowToVeiculo(row){
  return { id: row.id, placa: row.placa, modelo: row.modelo, marca: row.marca, anoCor: row.ano_cor };
}
function rowToAgendamento(row){
  return { id: row.id, funcionario: row.funcionario, veiculo: row.veiculo, inicio: row.inicio, fim: row.fim, motivo: row.motivo };
}
function rowToManutencao(row){
  return {
    id: row.id,
    veiculo: row.veiculo,
    tipo: row.tipo,
    data: row.data,
    km: row.km,
    custo: row.custo,
    oficina: row.oficina,
    descricao: row.descricao,
    proximaData: row.proxima_data
  };
}

if(!supaConfigured){
  document.getElementById('configBanner').style.display = 'block';
}

// ---- carregar dados ----
async function loadRegistros(){
  if(!supaConfigured) return;
  try{
    const { data, error } = await supabaseClient
      .from('registros_frota')
      .select('*')
      .order('saida_data_hora', { ascending: false });
    if(error) throw error;
    registros = data.map(rowToRegistro);
    renderAll();
    updateSyncIndicator(true);
  }catch(e){
    console.error('Erro ao carregar registros:', describeError(e));
    updateSyncIndicator(false, true, describeError(e));
  }
}

async function loadVeiculos(){
  if(!supaConfigured) return;
  try{
    const { data, error } = await supabaseClient
      .from('veiculos_frota')
      .select('*')
      .order('created_at', { ascending: false });
    if(error) throw error;
    veiculos = data.map(rowToVeiculo);
    renderVeiculosList();
    populateVeiculoSelect();
    renderStats();
  }catch(e){
    console.error('Erro ao carregar veículos:', describeError(e));
    updateSyncIndicator(false, true, describeError(e));
  }
}

async function loadAgendamentos(){
  if(!supaConfigured) return;
  try{
    const { data, error } = await supabaseClient
      .from('agendamentos_frota')
      .select('*')
      .order('inicio', { ascending: true });
    if(error) throw error;
    agendamentos = data.map(rowToAgendamento);
    renderAgendamentosList();
    renderStats();
  }catch(e){
    console.error('Erro ao carregar agendamentos:', describeError(e));
    updateSyncIndicator(false, true, describeError(e));
  }
}

async function loadManutencoes(){
  if(!supaConfigured) return;
  try{
    const { data, error } = await supabaseClient
      .from('manutencoes_frota')
      .select('*')
      .order('data', { ascending: false });
    if(error) throw error;
    manutencoes = data.map(rowToManutencao);
    renderManutencoesList();
  }catch(e){
    console.error('Erro ao carregar manutenções:', describeError(e));
    updateSyncIndicator(false, true, describeError(e));
  }
}

function updateSyncIndicator(success, error, errorMsg){
  const dot = document.getElementById('syncDot');
  const label = document.getElementById('syncLabel');
  if(!dot || !label) return;
  if(!supaConfigured){
    dot.style.background = 'var(--warn)';
    label.textContent = 'Supabase não configurado';
    return;
  }
  if(error){
    dot.style.background = 'var(--warn)';
    label.textContent = errorMsg ? `Erro de sincronização: ${errorMsg}` : 'Falha ao sincronizar — tentando novamente…';
    return;
  }
  dot.style.background = 'var(--good)';
  const agora = new Date().toLocaleTimeString('pt-BR', {hour:'2-digit', minute:'2-digit', second:'2-digit'});
  label.textContent = `Sincronizado em tempo real · ${agora}`;
}

// ---- tempo real via Supabase Realtime (WebSocket, sem polling) ----
if(supaConfigured){
  supabaseClient
    .channel('registros_frota_changes')
    .on('postgres_changes', { event: '*', schema: 'public', table: 'registros_frota' }, ()=>{
      const selecionadoAntes = document.getElementById('fSelecionar') ? document.getElementById('fSelecionar').value : '';
      loadRegistros().then(()=>{
        const selEl = document.getElementById('fSelecionar');
        if(selecionadoAntes && selEl && [...selEl.options].some(o=>o.value===selecionadoAntes)){
          selEl.value = selecionadoAntes;
        }
      });
    })
    .subscribe();

  supabaseClient
    .channel('veiculos_frota_changes')
    .on('postgres_changes', { event: '*', schema: 'public', table: 'veiculos_frota' }, ()=>{
      const veiculoAntes = document.getElementById('fVeiculo') ? document.getElementById('fVeiculo').value : '';
      loadVeiculos().then(()=>{
        const vEl = document.getElementById('fVeiculo');
        if(veiculoAntes && vEl && [...vEl.options].some(o=>o.value===veiculoAntes)){
          vEl.value = veiculoAntes;
        }
      });
    })
    .subscribe();

  supabaseClient
    .channel('agendamentos_frota_changes')
    .on('postgres_changes', { event: '*', schema: 'public', table: 'agendamentos_frota' }, ()=>{
      loadAgendamentos();
    })
    .subscribe();

  supabaseClient
    .channel('manutencoes_frota_changes')
    .on('postgres_changes', { event: '*', schema: 'public', table: 'manutencoes_frota' }, ()=>{
      loadManutencoes();
    })
    .subscribe();
}


function showMsg(elId, text, ok){
  const el = document.getElementById(elId);
  el.innerHTML = `<div class="msg ${ok?'ok':'err'}">${text}</div>`;
  if(ok) setTimeout(()=>{ el.innerHTML=''; }, 3500);
}

// ---- tabs ----
const tabAgendamentoBtn = document.getElementById('tabAgendamentoBtn');
const tabSaidaBtn = document.getElementById('tabSaidaBtn');
const tabRetornoBtn = document.getElementById('tabRetornoBtn');
const tabVeiculosBtn = document.getElementById('tabVeiculosBtn');
const tabManutencaoBtn = document.getElementById('tabManutencaoBtn');
const panelAgendamento = document.getElementById('panelAgendamento');
const panelSaida = document.getElementById('panelSaida');
const panelRetorno = document.getElementById('panelRetorno');
const panelVeiculos = document.getElementById('panelVeiculos');
const panelManutencao = document.getElementById('panelManutencao');

function activateTab(tab){
  const tabs = {
    agendamento: [tabAgendamentoBtn, panelAgendamento],
    saida: [tabSaidaBtn, panelSaida],
    retorno: [tabRetornoBtn, panelRetorno],
    veiculos: [tabVeiculosBtn, panelVeiculos],
    manutencao: [tabManutencaoBtn, panelManutencao]
  };
  Object.entries(tabs).forEach(([key, [btn, panel]])=>{
    const active = key === tab;
    btn.classList.toggle('active', active);
    panel.style.display = active ? 'block' : 'none';
  });
  if(tab === 'retorno') populateSelect();
  if(tab === 'saida') populateVeiculoSelect();
  if(tab === 'agendamento'){ populateVeiculoSelect(); renderAgendamentosList(); }
  if(tab === 'manutencao'){ populateVeiculoSelect(); renderManutencoesList(); }
}

tabAgendamentoBtn.addEventListener('click', ()=> activateTab('agendamento'));
tabSaidaBtn.addEventListener('click', ()=> activateTab('saida'));
tabRetornoBtn.addEventListener('click', ()=> activateTab('retorno'));
tabVeiculosBtn.addEventListener('click', ()=> activateTab('veiculos'));
tabManutencaoBtn.addEventListener('click', ()=> activateTab('manutencao'));

// ---- photo capture ----
let fotoSaidaData = null;
let fotoRetornoData = null;

function compressImage(file, maxDim=900, quality=0.65){
  return new Promise((resolve, reject)=>{
    const reader = new FileReader();
    reader.onload = (ev)=>{
      const img = new Image();
      img.onload = ()=>{
        let w = img.width, h = img.height;
        if(w > h && w > maxDim){ h = Math.round(h * maxDim / w); w = maxDim; }
        else if(h > maxDim){ w = Math.round(w * maxDim / h); h = maxDim; }
        const canvas = document.createElement('canvas');
        canvas.width = w; canvas.height = h;
        canvas.getContext('2d').drawImage(img, 0, 0, w, h);
        resolve(canvas.toDataURL('image/jpeg', quality));
      };
      img.onerror = reject;
      img.src = ev.target.result;
    };
    reader.onerror = reject;
    reader.readAsDataURL(file);
  });
}

function setupPhotoField(btnId, inputId, wrapId, imgId, removeId, onChange){
  const btn = document.getElementById(btnId);
  const input = document.getElementById(inputId);
  const wrap = document.getElementById(wrapId);
  const img = document.getElementById(imgId);
  const removeBtn = document.getElementById(removeId);

  btn.addEventListener('click', ()=> input.click());

  input.addEventListener('change', async ()=>{
    const file = input.files[0];
    if(!file) return;
    btn.disabled = true;
    btn.textContent = 'Processando foto…';
    try{
      const dataUrl = await compressImage(file);
      onChange(dataUrl);
      img.src = dataUrl;
      wrap.classList.add('show');
    }catch(e){
      alert('Não foi possível processar a foto. Tente novamente.');
    }
    btn.disabled = false;
    btn.textContent = '📷 Tirar / anexar foto';
  });

  removeBtn.addEventListener('click', ()=>{
    onChange(null);
    input.value = '';
    wrap.classList.remove('show');
  });
}

setupPhotoField('btnFotoSaida','fFotoSaida','previewSaidaWrap','previewSaidaImg','removeFotoSaida', (v)=>{ fotoSaidaData = v; });
setupPhotoField('btnFotoRetorno','fFotoRetorno','previewRetornoWrap','previewRetornoImg','removeFotoRetorno', (v)=>{ fotoRetornoData = v; });

// ---- fuel gauge ----
function setupFuelGauge(gaugeId, hiddenInputId){
  const gauge = document.getElementById(gaugeId);
  const hidden = document.getElementById(hiddenInputId);
  gauge.querySelectorAll('.fuel-seg').forEach(seg=>{
    seg.addEventListener('click', ()=>{
      gauge.querySelectorAll('.fuel-seg').forEach(s=>s.classList.remove('active'));
      seg.classList.add('active');
      hidden.value = seg.getAttribute('data-value');
    });
  });
}
function resetFuelGauge(gaugeId, hiddenInputId){
  document.getElementById(gaugeId).querySelectorAll('.fuel-seg').forEach(s=>s.classList.remove('active'));
  document.getElementById(hiddenInputId).value = '';
}
setupFuelGauge('gaugeSaida','fCombustivelSaida');
setupFuelGauge('gaugeRetorno','fCombustivelRetorno');

function resetPhotoField(wrapId, imgId){
  document.getElementById(wrapId).classList.remove('show');
  document.getElementById(imgId).src = '';
}

const lightbox = document.getElementById('lightbox');
const lightboxImg = document.getElementById('lightboxImg');
function openLightbox(src){
  if(!src) return;
  lightboxImg.src = src;
  lightbox.classList.add('open');
}
lightbox.addEventListener('click', ()=> lightbox.classList.remove('open'));

// ---- form: saida ----
document.getElementById('formSaida').addEventListener('submit', async (e)=>{
  e.preventDefault();
  if(!supaConfigured){
    showMsg('msgSaida', 'Configure o Supabase no início do arquivo antes de usar o app.', false);
    return;
  }
  const veiculoSelecionado = document.getElementById('fVeiculo').value.trim();
  if(!veiculoSelecionado){
    showMsg('msgSaida', 'Cadastre um veículo na aba "Veículos" antes de registrar a saída.', false);
    return;
  }
  const combustivelSaida = document.getElementById('fCombustivelSaida').value;
  if(!combustivelSaida){
    showMsg('msgSaida', 'Selecione o nível de combustível na saída.', false);
    return;
  }
  const btn = e.target.querySelector('.submit-btn');
  const row = {
    id: 'r' + Date.now(),
    funcionario: document.getElementById('fFuncionario').value.trim(),
    veiculo: veiculoSelecionado,
    saida_data_hora: document.getElementById('fSaidaData').value,
    saida_km: Number(document.getElementById('fSaidaKm').value),
    combustivel_saida: combustivelSaida,
    motivo: document.getElementById('fMotivo').value.trim(),
    rota_ida: document.getElementById('fRotaIda').value.trim(),
    foto_saida: fotoSaidaData,
    status: 'aberto'
  };
  btn.disabled = true;
  const { error } = await supabaseClient.from('registros_frota').insert([row]);
  btn.disabled = false;
  if(!error){
    showMsg('msgSaida', 'Saída registrada com sucesso.', true);
    e.target.reset();
    document.getElementById('fSaidaData').value = nowLocalInputValue();
    fotoSaidaData = null;
    resetPhotoField('previewSaidaWrap','previewSaidaImg');
    resetFuelGauge('gaugeSaida','fCombustivelSaida');
    await loadRegistros();
  }else{
    console.error('Erro ao salvar saída:', describeError(error));
    showMsg('msgSaida', 'Não foi possível salvar: ' + describeError(error), false);
  }
});

// ---- form: retorno ----
function populateSelect(){
  const sel = document.getElementById('fSelecionar');
  const abertos = registros.filter(r=>r.status==='aberto');
  sel.innerHTML = '';
  if(abertos.length === 0){
    sel.innerHTML = '<option value="">Nenhum veículo em rota</option>';
    document.getElementById('btnRetorno').disabled = true;
    return;
  }
  document.getElementById('btnRetorno').disabled = false;
  abertos.forEach(r=>{
    const opt = document.createElement('option');
    opt.value = r.id;
    opt.textContent = `${r.veiculo} — ${r.funcionario} (saiu ${fmtDateTime(r.saidaDataHora)})`;
    sel.appendChild(opt);
  });
}

document.getElementById('formRetorno').addEventListener('submit', async (e)=>{
  e.preventDefault();
  if(!supaConfigured){
    showMsg('msgRetorno', 'Configure o Supabase no início do arquivo antes de usar o app.', false);
    return;
  }
  const id = document.getElementById('fSelecionar').value;
  if(!id) return;
  const registroAtual = registros.find(r=>r.id===id);
  if(!registroAtual) return;

  const retornoKm = Number(document.getElementById('fRetornoKm').value);
  if(retornoKm < registroAtual.saidaKm){
    showMsg('msgRetorno', 'A quilometragem de chegada não pode ser menor que a de saída.', false);
    return;
  }
  const combustivelRetorno = document.getElementById('fCombustivelRetorno').value;
  if(!combustivelRetorno){
    showMsg('msgRetorno', 'Selecione o nível de combustível na chegada.', false);
    return;
  }

  const btn = document.getElementById('btnRetorno');
  const updates = {
    retorno_data_hora: document.getElementById('fRetornoData').value,
    retorno_km: retornoKm,
    combustivel_retorno: combustivelRetorno,
    rota_volta: document.getElementById('fRotaVolta').value.trim(),
    foto_retorno: fotoRetornoData,
    status: 'fechado'
  };

  btn.disabled = true;
  const { error } = await supabaseClient.from('registros_frota').update(updates).eq('id', id);
  btn.disabled = false;
  if(!error){
    showMsg('msgRetorno', 'Retorno registrado com sucesso.', true);
    e.target.reset();
    document.getElementById('fRetornoData').value = nowLocalInputValue();
    fotoRetornoData = null;
    resetPhotoField('previewRetornoWrap','previewRetornoImg');
    resetFuelGauge('gaugeRetorno','fCombustivelRetorno');
    await loadRegistros();
    populateSelect();
  }else{
    console.error('Erro ao salvar retorno:', describeError(error));
    showMsg('msgRetorno', 'Não foi possível salvar: ' + describeError(error), false);
  }
});

// ---- veículos: cadastro ----
function populateVeiculoSelect(){
  populateVeiculoSelectFor('fVeiculo', 'semVeiculoHint');
  populateVeiculoSelectFor('fAgVeiculo', 'semVeiculoHintAg');
  populateVeiculoSelectFor('fManVeiculo', 'semVeiculoHintMan');
}

function populateVeiculoSelectFor(selId, hintId){
  const sel = document.getElementById(selId);
  const hint = document.getElementById(hintId);
  const valorAtual = sel.value;
  sel.innerHTML = '';
  if(veiculos.length === 0){
    sel.innerHTML = '<option value="">Nenhum veículo cadastrado</option>';
    sel.disabled = true;
    hint.style.display = 'block';
    return;
  }
  sel.disabled = false;
  hint.style.display = 'none';
  veiculos.forEach(v=>{
    const opt = document.createElement('option');
    opt.value = `${v.placa} — ${v.modelo}`;
    opt.textContent = `${v.placa} — ${v.modelo}`;
    sel.appendChild(opt);
  });
  if(valorAtual && [...sel.options].some(o=>o.value===valorAtual)){
    sel.value = valorAtual;
  }
}

function renderVeiculosList(){
  const list = document.getElementById('veiculosList');
  if(veiculos.length === 0){
    list.innerHTML = '<p class="empty-note">Nenhum veículo cadastrado ainda.</p>';
    return;
  }
  list.innerHTML = veiculos.map(v=>`
    <div class="veiculo-card">
      <div class="veiculo-info">
        <div class="placa">${escapeHtml(v.placa)}</div>
        <div class="modelo">${escapeHtml(v.modelo)}</div>
        ${(v.marca || v.anoCor) ? `<div class="detalhe">${escapeHtml(v.marca || '')}${v.marca && v.anoCor ? ' · ' : ''}${escapeHtml(v.anoCor || '')}</div>` : ''}
      </div>
      <button type="button" class="veiculo-remove" data-id="${v.id}">Excluir</button>
    </div>
  `).join('');

  list.querySelectorAll('.veiculo-remove').forEach(btn=>{
    btn.addEventListener('click', async ()=>{
      const id = btn.getAttribute('data-id');
      if(!confirm('Excluir este veículo do cadastro? Registros de saída/retorno já feitos não são afetados.')) return;
      btn.disabled = true;
      const { error } = await supabaseClient.from('veiculos_frota').delete().eq('id', id);
      btn.disabled = false;
      if(!error){
        await loadVeiculos();
      }else{
        console.error('Erro ao excluir veículo:', describeError(error));
        alert('Não foi possível excluir: ' + describeError(error));
      }
    });
  });
}

document.getElementById('formVeiculo').addEventListener('submit', async (e)=>{
  e.preventDefault();
  if(!supaConfigured){
    showMsg('msgVeiculo', 'Configure o Supabase no início do arquivo antes de usar o app.', false);
    return;
  }
  const btn = e.target.querySelector('.submit-btn');
  const row = {
    id: 'v' + Date.now(),
    placa: document.getElementById('fPlaca').value.trim().toUpperCase(),
    modelo: document.getElementById('fModelo').value.trim(),
    marca: document.getElementById('fMarca').value.trim(),
    ano_cor: document.getElementById('fAnoCor').value.trim()
  };
  btn.disabled = true;
  const { error } = await supabaseClient.from('veiculos_frota').insert([row]);
  btn.disabled = false;
  if(!error){
    showMsg('msgVeiculo', 'Veículo cadastrado com sucesso.', true);
    e.target.reset();
    await loadVeiculos();
  }else{
    console.error('Erro ao cadastrar veículo:', describeError(error));
    showMsg('msgVeiculo', 'Não foi possível cadastrar: ' + describeError(error), false);
  }
});

// ---- agendamento: verificação de conflito e cadastro ----
document.getElementById('fAgInicio').value = nowLocalInputValue();

async function verificarConflitoAgendamento(veiculo, inicio, fim, ignorarId){
  let query = supabaseClient
    .from('agendamentos_frota')
    .select('*')
    .eq('veiculo', veiculo)
    .lt('inicio', fim)
    .gt('fim', inicio);
  if(ignorarId) query = query.neq('id', ignorarId);
  const { data, error } = await query;
  if(error) throw error;
  return data;
}

document.getElementById('formAgendamento').addEventListener('submit', async (e)=>{
  e.preventDefault();
  if(!supaConfigured){
    showMsg('msgAgendamento', 'Configure o Supabase no início do arquivo antes de usar o app.', false);
    return;
  }
  const veiculoSelecionado = document.getElementById('fAgVeiculo').value.trim();
  if(!veiculoSelecionado){
    showMsg('msgAgendamento', 'Cadastre um veículo na aba "Veículos" antes de agendar.', false);
    return;
  }
  const inicioLocal = document.getElementById('fAgInicio').value;
  const fimLocal = document.getElementById('fAgFim').value;
  if(!inicioLocal || !fimLocal){
    showMsg('msgAgendamento', 'Informe a data/hora de retirada e de devolução prevista.', false);
    return;
  }
  if(new Date(fimLocal) <= new Date(inicioLocal)){
    showMsg('msgAgendamento', 'A devolução prevista deve ser depois da retirada.', false);
    return;
  }
  // O campo datetime-local não tem fuso horário embutido; convertemos para
  // ISO com o fuso correto antes de salvar, para que o horário exibido depois
  // seja exatamente o horário escolhido, e não deslocado pelo fuso do banco.
  const inicio = new Date(inicioLocal).toISOString();
  const fim = new Date(fimLocal).toISOString();

  const btn = e.target.querySelector('.submit-btn');
  btn.disabled = true;

  try{
    const conflitos = await verificarConflitoAgendamento(veiculoSelecionado, inicio, fim);
    if(conflitos.length > 0){
      const c = conflitos[0];
      showMsg('msgAgendamento', `Este veículo já está agendado por ${escapeHtml(c.funcionario)} de ${fmtDateTime(c.inicio)} até ${fmtDateTime(c.fim)}. Escolha outro horário ou veículo.`, false);
      btn.disabled = false;
      return;
    }

    const row = {
      id: 'a' + Date.now(),
      funcionario: document.getElementById('fAgFuncionario').value.trim(),
      veiculo: veiculoSelecionado,
      inicio: inicio,
      fim: fim,
      motivo: document.getElementById('fAgMotivo').value.trim()
    };
    const { error } = await supabaseClient.from('agendamentos_frota').insert([row]);
    if(error){
      if(error.code === '23P01'){
        showMsg('msgAgendamento', 'Este horário acabou de ser reservado por outra pessoa para este veículo. Escolha outro horário.', false);
      }else{
        console.error('Erro ao agendar:', describeError(error));
        showMsg('msgAgendamento', 'Não foi possível agendar: ' + describeError(error), false);
      }
      btn.disabled = false;
      return;
    }
    showMsg('msgAgendamento', 'Retirada agendada com sucesso.', true);
    e.target.reset();
    document.getElementById('fAgInicio').value = nowLocalInputValue();
    await loadAgendamentos();
  }catch(err){
    console.error('Erro ao verificar disponibilidade:', describeError(err));
    showMsg('msgAgendamento', 'Não foi possível verificar a disponibilidade: ' + describeError(err), false);
  }
  btn.disabled = false;
});

function renderAgendamentosList(){
  const list = document.getElementById('agendamentosList');
  if(agendamentos.length === 0){
    list.innerHTML = '<p class="empty-note">Nenhum agendamento ainda.</p>';
    return;
  }
  const agora = new Date();
  list.innerHTML = agendamentos.map(a=>{
    const passado = new Date(a.fim) < agora;
    return `
    <div class="agendamento-card ${passado?'past':''}">
      <div class="agendamento-info">
        <div class="veiculo">${escapeHtml(a.veiculo)}</div>
        <div class="periodo">${fmtDateTime(a.inicio)} → ${fmtDateTime(a.fim)}</div>
        <div class="funcionario">${escapeHtml(a.funcionario)}</div>
        ${a.motivo ? `<div class="motivo">${escapeHtml(a.motivo)}</div>` : ''}
      </div>
      <button type="button" class="agendamento-remove" data-id="${a.id}">Cancelar</button>
    </div>`;
  }).join('');

  list.querySelectorAll('.agendamento-remove').forEach(btn=>{
    btn.addEventListener('click', async ()=>{
      const id = btn.getAttribute('data-id');
      if(!confirm('Cancelar este agendamento?')) return;
      btn.disabled = true;
      const { error } = await supabaseClient.from('agendamentos_frota').delete().eq('id', id);
      btn.disabled = false;
      if(!error){
        await loadAgendamentos();
      }else{
        console.error('Erro ao cancelar agendamento:', describeError(error));
        alert('Não foi possível cancelar: ' + describeError(error));
      }
    });
  });
}

// ---- manutenções ----
document.getElementById('fManData').value = new Date().toISOString().slice(0,10);

document.getElementById('formManutencao').addEventListener('submit', async (e)=>{
  e.preventDefault();
  if(!supaConfigured){
    showMsg('msgManutencao', 'Configure o Supabase no início do arquivo antes de usar o app.', false);
    return;
  }
  const veiculoSelecionado = document.getElementById('fManVeiculo').value.trim();
  if(!veiculoSelecionado){
    showMsg('msgManutencao', 'Cadastre um veículo na aba "Veículos" antes de registrar uma manutenção.', false);
    return;
  }
  const data = document.getElementById('fManData').value;
  if(!data){
    showMsg('msgManutencao', 'Informe a data da manutenção.', false);
    return;
  }

  const btn = e.target.querySelector('.submit-btn');
  btn.disabled = true;

  const kmVal = document.getElementById('fManKm').value;
  const custoVal = document.getElementById('fManCusto').value;
  const proximaVal = document.getElementById('fManProxima').value;

  const row = {
    id: 'm' + Date.now(),
    veiculo: veiculoSelecionado,
    tipo: document.getElementById('fManTipo').value,
    data: data,
    km: kmVal ? Number(kmVal) : null,
    custo: custoVal ? Number(custoVal) : null,
    oficina: document.getElementById('fManOficina').value.trim(),
    descricao: document.getElementById('fManDescricao').value.trim(),
    proxima_data: proximaVal || null
  };

  const { error } = await supabaseClient.from('manutencoes_frota').insert([row]);
  btn.disabled = false;
  if(!error){
    showMsg('msgManutencao', 'Manutenção registrada com sucesso.', true);
    e.target.reset();
    document.getElementById('fManData').value = new Date().toISOString().slice(0,10);
    await loadManutencoes();
  }else{
    console.error('Erro ao registrar manutenção:', describeError(error));
    showMsg('msgManutencao', 'Não foi possível registrar: ' + describeError(error), false);
  }
});

function fmtDateBR(d){
  if(!d) return '—';
  const [y,m,day] = d.split('-');
  return `${day}/${m}/${y}`;
}
function fmtMoeda(v){
  if(v === null || v === undefined || v === '') return null;
  return Number(v).toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });
}

function renderManutencoesList(){
  const list = document.getElementById('manutencoesList');
  if(manutencoes.length === 0){
    list.innerHTML = '<p class="empty-note">Nenhuma manutenção registrada ainda.</p>';
    return;
  }
  list.innerHTML = manutencoes.map(m=>{
    const detalhes = [];
    if(m.km !== null && m.km !== undefined) detalhes.push(`${Number(m.km).toLocaleString('pt-BR')} km`);
    if(m.custo !== null && m.custo !== undefined) detalhes.push(fmtMoeda(m.custo));
    if(m.oficina) detalhes.push(escapeHtml(m.oficina));
    if(m.proximaData) detalhes.push(`Próxima: ${fmtDateBR(m.proximaData)}`);
    return `
    <div class="manutencao-card">
      <div class="manutencao-info">
        <div class="topo">
          <span class="veiculo">${escapeHtml(m.veiculo)}</span>
          <span class="manutencao-tipo">${escapeHtml(m.tipo)}</span>
        </div>
        <div class="data">${fmtDateBR(m.data)}</div>
        ${detalhes.length ? `<div class="meta">${detalhes.join(' · ')}</div>` : ''}
        ${m.descricao ? `<div class="descricao">${escapeHtml(m.descricao)}</div>` : ''}
      </div>
      <button type="button" class="manutencao-remove" data-id="${m.id}">Excluir</button>
    </div>`;
  }).join('');

  list.querySelectorAll('.manutencao-remove').forEach(btn=>{
    btn.addEventListener('click', async ()=>{
      const id = btn.getAttribute('data-id');
      if(!confirm('Excluir este registro de manutenção?')) return;
      btn.disabled = true;
      const { error } = await supabaseClient.from('manutencoes_frota').delete().eq('id', id);
      btn.disabled = false;
      if(!error){
        await loadManutencoes();
      }else{
        console.error('Erro ao excluir manutenção:', describeError(error));
        alert('Não foi possível excluir: ' + describeError(error));
      }
    });
  });
}

// ---- reset ----
document.getElementById('resetBtn').addEventListener('click', async ()=>{
  if(!confirm('Isso vai apagar todos os registros para todas as pessoas que usam este app. Confirmar?')) return;
  const { error } = await supabaseClient.from('registros_frota').delete().like('id', 'r%');
  if(!error){
    await loadRegistros();
    populateSelect();
  }else{
    alert('Não foi possível limpar os registros.');
  }
});

// ---- render ----
function renderAll(){
  renderStats();
  renderLedger();
  populateSelect();
}

function renderStats(){
  const open = registros.filter(r=>r.status==='aberto').length;
  const closed = registros.filter(r=>r.status==='fechado').length;
  const agora = new Date();
  const agendamentosFuturos = agendamentos.filter(a=> new Date(a.fim) >= agora).length;
  document.getElementById('statOpen').textContent = open;
  document.getElementById('statClosed').textContent = closed;
  document.getElementById('statTotal').textContent = registros.length;
  document.getElementById('statVeiculos').textContent = veiculos.length;
  document.getElementById('statAgendamentos').textContent = agendamentosFuturos;
}

function renderLedger(){
  const ledger = document.getElementById('ledger');
  if(registros.length === 0){
    ledger.innerHTML = '<p class="empty-note">Nenhum registro ainda. Registre a primeira saída acima.</p>';
    return;
  }
  ledger.innerHTML = registros.map(r=>{
    const isOpen = r.status === 'aberto';
    const kmTotal = isOpen ? null : (r.retornoKm - r.saidaKm);
    return `
    <div class="ticket">
      <div class="ticket-half saida">
        <div class="ticket-top">
          <span class="badge ${isOpen?'open':'closed'}">${isOpen?'Em rota':'Retornado'}</span>
          <span class="ticket-id">#${r.id.slice(-5)}</span>
        </div>
        <p class="ticket-name">${escapeHtml(r.funcionario)}</p>
        <p class="ticket-vehicle">${escapeHtml(r.veiculo)}</p>
        <div class="dl">
          <div class="dl-row"><span class="k">Saída</span><span class="v">${fmtDateTime(r.saidaDataHora)}</span></div>
          <div class="dl-row"><span class="k">KM saída</span><span class="v">${r.saidaKm.toLocaleString('pt-BR')} km</span></div>
          ${r.combustivelSaida ? `<div class="dl-row"><span class="k">Combustível</span><span class="v">${fmtCombustivel(r.combustivelSaida)}</span></div>` : ''}
        </div>
        <div class="route-note">
          <span class="k">Motivo</span>${escapeHtml(r.motivo) || '—'}
        </div>
        ${r.rotaIda ? `<div class="route-note"><span class="k">Rota de ida</span>${escapeHtml(r.rotaIda)}</div>` : ''}
        ${r.fotoSaida ? `<img class="ticket-photo" src="${r.fotoSaida}" alt="Foto do veículo na saída" onclick="openLightbox('${r.fotoSaida}')">` : ''}
      </div>
      <div class="ticket-half retorno">
        ${isOpen ? `
          <p class="waiting">Aguardando retorno do veículo…</p>
        ` : `
          <div class="dl">
            <div class="dl-row"><span class="k">Chegada</span><span class="v">${fmtDateTime(r.retornoDataHora)}</span></div>
            <div class="dl-row"><span class="k">KM chegada</span><span class="v">${r.retornoKm.toLocaleString('pt-BR')} km</span></div>
            ${r.combustivelRetorno ? `<div class="dl-row"><span class="k">Combustível</span><span class="v">${fmtCombustivel(r.combustivelRetorno)}</span></div>` : ''}
          </div>
          ${r.rotaVolta ? `<div class="route-note"><span class="k">Rota de volta</span>${escapeHtml(r.rotaVolta)}</div>` : ''}
          ${r.fotoRetorno ? `<img class="ticket-photo" src="${r.fotoRetorno}" alt="Foto do veículo no retorno" onclick="openLightbox('${r.fotoRetorno}')">` : ''}
          <div class="km-total"><span class="k">Total percorrido</span><span class="v">${kmTotal.toLocaleString('pt-BR')} km</span></div>
        `}
      </div>
    </div>`;
  }).join('');
}

function escapeHtml(str){
  if(!str) return '';
  const d = document.createElement('div');
  d.textContent = str;
  return d.innerHTML;
}

function setupQuickAccess(){
  const url = window.location.href;
  document.getElementById('qrUrl').textContent = url;
  try{
    new QRCode(document.getElementById('qrBox'), {
      text: url,
      width: 80,
      height: 80,
      colorDark: '#14161a',
      colorLight: '#ffffff',
      correctLevel: QRCode.CorrectLevel.M
    });
  }catch(e){
    document.getElementById('qrBox').textContent = 'QR indisponível';
  }

  document.getElementById('copyUrlBtn').addEventListener('click', async ()=>{
    const btn = document.getElementById('copyUrlBtn');
    try{
      await navigator.clipboard.writeText(url);
      btn.textContent = 'Copiado!';
    }catch(e){
      btn.textContent = 'Não foi possível copiar';
    }
    setTimeout(()=>{ btn.textContent = 'Copiar link'; }, 2000);
  });
}

setupQuickAccess();
if(supaConfigured){
  loadRegistros();
  loadVeiculos();
  loadAgendamentos();
  loadManutencoes();
}else{
  renderAll();
  updateSyncIndicator();
}
</script>
</body>
</html>
