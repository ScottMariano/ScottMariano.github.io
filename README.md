<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CondoTech Soluções — Acesso via Lello</title>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif; background: #f0f2f5; color: #333; min-height: 100vh; }

        /* ── HEADER ── */
        .header { background: #1a1a2e; color: #fff; padding: 1rem 2rem; display: flex; align-items: center; justify-content: space-between; box-shadow: 0 2px 8px rgba(0,0,0,.35); }
        .header-left { display: flex; align-items: center; gap: .9rem; }
        .partner-logo { width: 42px; height: 42px; border-radius: 10px; background: linear-gradient(135deg, #667eea, #764ba2); display: flex; align-items: center; justify-content: center; font-weight: 800; font-size: 1.05rem; color: #fff; flex-shrink: 0; }
        .partner-name { font-size: 1.15rem; font-weight: 700; letter-spacing: -.01em; }
        .partner-sub  { font-size: .72rem; color: #8899bb; margin-top: .1rem; }
        .header-right { display: flex; align-items: center; gap: .7rem; }
        .lello-badge  { background: rgba(168,230,207,.12); border: 1px solid rgba(168,230,207,.25); color: #a8e6cf; font-size: .7rem; padding: .28rem .7rem; border-radius: 20px; font-weight: 600; letter-spacing: .04em; }

        /* ── STATUS BAR ── */
        .status-bar { background: #141424; color: #667; font-size: .72rem; font-family: 'Consolas', monospace; padding: .45rem 2rem; display: flex; gap: 2rem; flex-wrap: wrap; border-bottom: 1px solid #222; }
        .status-bar .sb { transition: color .3s; }
        .status-bar .sb.ok      { color: #a8e6cf; }
        .status-bar .sb.pending { color: #ffd700; }
        .status-bar .sb.error   { color: #ff7675; }

        /* ── DEMO BANNER ── */
        .demo-banner { background: #fff3cd; border-bottom: 1px solid #ffc107; color: #856404; font-size: .78rem; padding: .45rem 2rem; display: none; }
        .demo-banner.visible { display: flex; align-items: center; gap: .5rem; }

        /* ── LAYOUT ── */
        .container { max-width: 820px; margin: 1.8rem auto; padding: 0 1.5rem 3rem; }
        .timeline { display: flex; flex-direction: column; gap: 1.1rem; }

        /* ── STEP CARD ── */
        .step { background: #fff; border: 1px solid #e2e2e2; border-radius: 10px; overflow: hidden; transition: box-shadow .2s, opacity .3s; }
        .step.dimmed { opacity: .45; pointer-events: none; }
        .step.dimmed .step-body { display: none; }
        .step-header { display: flex; align-items: center; gap: .75rem; padding: .85rem 1.2rem; border-bottom: 1px solid #f2f2f2; background: #fafafa; }
        .step-num { width: 28px; height: 28px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: .82rem; font-weight: 700; flex-shrink: 0; }
        .step-num.recv    { background: #e8f5e9; color: #2e7d32; }
        .step-num.proc    { background: #fffde7; color: #f57f17; }
        .step-num.done    { background: #e3f2fd; color: #1565c0; }
        .step-num.success { background: #e8f5e9; color: #2e7d32; }
        .step-num.fail    { background: #ffebee; color: #b71c1c; }
        .step-title { font-weight: 700; font-size: .9rem; color: #1a1a2e; }
        .step-desc  { font-size: .74rem; color: #999; margin-top: .1rem; }
        .step-body  { padding: 1.2rem; }

        /* ── TOKEN BOX ── */
        .token-label { font-size: .7rem; font-weight: 700; text-transform: uppercase; color: #999; letter-spacing: .05em; margin-bottom: .45rem; }
        .token-wrap  { position: relative; }
        .token-box   { background: #0d1117; color: #58a6ff; border: 1px solid #30363d; border-radius: 6px; padding: .8rem 1rem; font-family: 'Consolas', 'Courier New', monospace; font-size: .73rem; word-break: break-all; line-height: 1.55; max-height: 76px; overflow: hidden; transition: max-height .3s; }
        .token-box.expanded { max-height: 300px; }
        .token-fade  { position: absolute; bottom: 0; left: 0; right: 0; height: 28px; background: linear-gradient(transparent, #0d1117); border-radius: 0 0 6px 6px; display: flex; align-items: flex-end; justify-content: center; padding-bottom: 3px; cursor: pointer; }
        .token-fade span { font-size: .68rem; color: #58a6ff; font-family: 'Consolas', monospace; }
        .token-box.expanded + .token-fade { display: none; }

        /* ── SECRET ROW ── */
        .secret-row   { display: flex; align-items: center; gap: .55rem; margin-top: .85rem; flex-wrap: wrap; }
        .secret-label { font-size: .7rem; font-weight: 700; text-transform: uppercase; color: #999; letter-spacing: .04em; min-width: 110px; }
        .secret-val   { flex: 1; min-width: 160px; background: #f8f9fa; border: 1px solid #ddd; border-radius: 5px; padding: .35rem .7rem; font-family: 'Consolas', monospace; font-size: .82rem; color: #c0392b; letter-spacing: .06em; }
        .btn-sm       { border: 1px solid #ddd; background: #fff; color: #555; font-size: .72rem; padding: .3rem .7rem; border-radius: 5px; cursor: pointer; font-weight: 600; white-space: nowrap; transition: background .15s; }
        .btn-sm:hover { background: #f0f0f0; }

        /* ── ALGO NOTE ── */
        .algo-note { margin-top: .85rem; font-size: .72rem; color: #aaa; line-height: 1.6; }
        .algo-note b { color: #666; }

        /* ── LOADING ── */
        .loading-section { text-align: center; padding: 1.4rem 1rem; }
        .spinner { width: 40px; height: 40px; border: 3px solid #eee; border-top-color: #27ae60; border-radius: 50%; animation: spin .75s linear infinite; margin: 0 auto .8rem; }
        @keyframes spin { to { transform: rotate(360deg); } }
        .loading-title { font-weight: 700; color: #1a1a2e; font-size: .95rem; }
        .loading-sub   { font-size: .78rem; color: #aaa; margin-top: .3rem; }
        .progress-wrap { background: #eee; border-radius: 20px; height: 4px; margin: 1rem auto 0; max-width: 220px; overflow: hidden; }
        .progress-fill { height: 100%; background: linear-gradient(90deg, #27ae60, #a8e6cf); border-radius: 20px; width: 0; }
        .progress-fill.running { animation: progressAnim 1.45s cubic-bezier(.4,0,.2,1) forwards; }
        @keyframes progressAnim { to { width: 100%; } }

        /* ── STEP 2 SUCCESS STATE ── */
        .step2-ok { text-align: center; padding: 1rem; }
        .step2-ok .check { font-size: 2rem; margin-bottom: .4rem; }
        .step2-ok .ok-text { font-weight: 700; color: #27ae60; font-size: .9rem; }
        .step2-ok .ok-sub  { font-size: .75rem; color: #aaa; margin-top: .25rem; }

        /* ── RESULT CARD ── */
        .result-card { opacity: 0; transform: translateY(6px); transition: opacity .4s ease, transform .4s ease; }
        .result-card.visible { opacity: 1; transform: translateY(0); }

        .user-hero { display: flex; align-items: center; gap: 1rem; padding-bottom: 1rem; border-bottom: 1px solid #f0f0f0; margin-bottom: 1rem; }
        .user-avatar { width: 52px; height: 52px; border-radius: 50%; background: linear-gradient(135deg, #1a1a2e, #27ae60); display: flex; align-items: center; justify-content: center; font-size: 1.25rem; font-weight: 700; color: #fff; flex-shrink: 0; }
        .user-name { font-size: 1.1rem; font-weight: 700; color: #1a1a2e; }
        .user-tipo { display: inline-block; margin-top: .25rem; background: #d4edda; color: #155724; font-size: .67rem; font-weight: 700; padding: .15rem .55rem; border-radius: 10px; text-transform: uppercase; letter-spacing: .05em; }

        .section-label { font-size: .68rem; font-weight: 700; text-transform: uppercase; color: #bbb; letter-spacing: .06em; margin: .9rem 0 .55rem; }
        .fields-grid { display: grid; grid-template-columns: 1fr 1fr; gap: .55rem .9rem; }
        .field-item { display: flex; flex-direction: column; gap: .15rem; }
        .field-item .fl { font-size: .67rem; font-weight: 700; text-transform: uppercase; color: #bbb; letter-spacing: .04em; }
        .field-item .fv { font-size: .84rem; color: #222; font-family: 'Consolas', monospace; }
        .field-item .fv.text { font-family: inherit; font-weight: 600; }

        /* ── ERROR ── */
        .error-box { background: #fff3f3; border: 1px solid #f5c6cb; border-radius: 6px; padding: 1rem; color: #721c24; font-size: .83rem; display: none; white-space: pre-wrap; line-height: 1.5; }
        .error-box.visible { display: block; }

        @media (max-width: 520px) {
            .fields-grid { grid-template-columns: 1fr; }
            .header { flex-direction: column; align-items: flex-start; gap: .6rem; }
            .status-bar { gap: .9rem; }
        }
    </style>
</head>
<body>

<!-- ══ HEADER ══ -->
<div class="header">
    <div class="header-left">
        <div class="partner-logo" id="partnerLogoEl">CT</div>
        <div>
            <div class="partner-name" id="partnerNameEl">CondoTech Soluções</div>
            <div class="partner-sub">Portal de Acesso do Parceiro</div>
        </div>
    </div>
    <div class="header-right">
        <span class="lello-badge">🔐 autenticado via Lello</span>
    </div>
</div>

<!-- ══ STATUS BAR ══ -->
<div class="status-bar">
    <span class="sb" id="sbPacket">📦 Aguardando pacote...</span>
    <span class="sb" id="sbDecrypt">🔑 Descriptografia: pendente</span>
    <span class="sb" id="sbUser">👤 Usuário: não identificado</span>
</div>

<!-- ══ DEMO BANNER ══ -->
<div class="demo-banner" id="demoBanner">
    <span>⚠</span>
    <span>Modo demonstração — nenhum <code>?payload=</code> encontrado na URL. Usando payload de exemplo gerado localmente.</span>
</div>

<!-- ══ MAIN ══ -->
<div class="container">
    <div class="timeline">

        <!-- STEP 1 — Pacote Recebido -->
        <div class="step" id="stepPacket">
            <div class="step-header">
                <div class="step-num recv">1</div>
                <div>
                    <div class="step-title">Pacote Recebido</div>
                    <div class="step-desc">Payload criptografado enviado pela Lello via <code>?payload=</code></div>
                </div>
            </div>
            <div class="step-body">
                <div class="token-label">Payload Criptografado (AES-GCM · Base64)</div>
                <div class="token-wrap">
                    <div class="token-box" id="tokenBox">
                        <span id="tokenText">Preparando pacote...</span>
                    </div>
                    <div class="token-fade" onclick="expandToken()"><span>▼ expandir</span></div>
                </div>

                <div class="secret-row">
                    <span class="secret-label">Package Secret</span>
                    <div class="secret-val" id="secretDisplay">••••••••••••••••••••••••</div>
                    <button class="btn-sm" onclick="toggleSecret()" id="btnReveal">Revelar</button>
                    <button class="btn-sm" onclick="copySecret()" id="btnCopy">Copiar</button>
                </div>

                <div class="algo-note">
                    <b>Algoritmo:</b> AES/GCM/NoPadding &nbsp;·&nbsp;
                    <b>Derivação de chave:</b> SHA-256(UTF-8(packageSecret)) &nbsp;·&nbsp;
                    <b>IV:</b> 12 bytes (embutido no início do pacote) &nbsp;·&nbsp;
                    <b>Tag:</b> 128 bits (GCM)
                </div>
            </div>
        </div>

        <!-- STEP 2 — Processando -->
        <div class="step dimmed" id="stepProcess">
            <div class="step-header">
                <div class="step-num proc" id="stepProcNum">2</div>
                <div>
                    <div class="step-title" id="stepProcTitle">Identificando Usuário</div>
                    <div class="step-desc">Descriptografando pacote com o Package Secret do parceiro</div>
                </div>
            </div>
            <div class="step-body" id="stepProcBody">
                <div class="loading-section" id="loadingSection">
                    <div class="spinner"></div>
                    <div class="loading-title">Identificando usuário...</div>
                    <div class="loading-sub">Descriptografando pacote com Package Secret</div>
                    <div class="progress-wrap">
                        <div class="progress-fill" id="progressFill"></div>
                    </div>
                </div>
            </div>
        </div>

        <!-- STEP 3 — Resultado -->
        <div class="step dimmed" id="stepResult">
            <div class="step-header">
                <div class="step-num done" id="stepResultNum">3</div>
                <div>
                    <div class="step-title" id="stepResultTitle">Usuário Identificado</div>
                    <div class="step-desc">Dados extraídos do pacote — acesso autorizado</div>
                </div>
            </div>
            <div class="step-body">
                <div class="error-box" id="errorBox"></div>
                <div class="result-card" id="resultCard">
                    <div class="user-hero">
                        <div class="user-avatar" id="userAvatar">?</div>
                        <div>
                            <div class="user-name" id="userName">—</div>
                            <span class="user-tipo" id="userTipo">—</span>
                        </div>
                    </div>
                    <div class="section-label">Dados de Localização</div>
                    <div class="fields-grid" id="gridLocalizacao"></div>
                    <div class="section-label">Dados de Contato</div>
                    <div class="fields-grid" id="gridContato"></div>
                    <div class="section-label">Claims do Pacote</div>
                    <div class="fields-grid" id="gridClaims"></div>
                </div>
            </div>
        </div>

    </div><!-- /timeline -->
</div><!-- /container -->

<script>
// ════════════════════════════════════════════════════════════
// CONFIGURAÇÃO ESTÁTICA DO PARCEIRO
// Altere estas constantes para cada parceiro/página
// ════════════════════════════════════════════════════════════
const PARTNER_NAME   = 'CondoTech Soluções';
const PARTNER_INITIALS = 'CT';
const PACKAGE_SECRET = 'lello-demo-secret-2024';

// ════════════════════════════════════════════════════════════
// PAYLOAD DE DEMONSTRAÇÃO
// Usado quando não há ?payload= na URL
// ════════════════════════════════════════════════════════════
function buildDemoPayload() {
    const now = Math.floor(Date.now() / 1000);
    return {
        iss: 'lello',
        aud: 'partner:condotech-demo',
        partnerId: 'condotech-demo',
        morador: {
            idMorador:    10432,
            idUnidade:    205,
            idCondominio: 1001,
            idBloco:      3,
            nome:         'Carlos Eduardo Mendes',
            cpf:          '123.456.789-00',
            email:        'carlos.mendes@email.com',
            telefone:     '(11) 98765-4321',
            tipoAcesso:   'MORADOR',
            nmUnidade:    'Apto 91',
            nmBloco:      'Bloco C',
            condominio:   'Residencial Jardim das Flores'
        },
        iat: now,
        exp: now + 60,
        jti: crypto.randomUUID()
    };
}

// ════════════════════════════════════════════════════════════
// CRYPTO — espelha exatamente:
//   AesEncryptionUtil.java  (AES/GCM/NoPadding, IV 12 bytes)
//   CredentialService.java  (chave = SHA-256(UTF-8(packageSecret)))
//
// Formato do pacote: Base64_padrão( IV[12] || ciphertext+GCM_TAG )
// ════════════════════════════════════════════════════════════
function uint8ToBase64(bytes) {
    let binary = '';
    for (let i = 0; i < bytes.length; i++) binary += String.fromCharCode(bytes[i]);
    return btoa(binary);
}

function base64ToUint8(b64) {
    // aceita Base64 padrão (com +/) e Base64Url (com -_)
    const normalized = b64.replace(/-/g, '+').replace(/_/g, '/');
    const binary = atob(normalized);
    const bytes = new Uint8Array(binary.length);
    for (let i = 0; i < binary.length; i++) bytes[i] = binary.charCodeAt(i);
    return bytes;
}

async function deriveKey(secret, usage) {
    const keyMaterial = await crypto.subtle.digest(
        'SHA-256',
        new TextEncoder().encode(secret)
    );
    return crypto.subtle.importKey('raw', keyMaterial, { name: 'AES-GCM' }, false, [usage]);
}

async function encryptPayload(plainText, secret) {
    const key = await deriveKey(secret, 'encrypt');
    const iv  = crypto.getRandomValues(new Uint8Array(12));
    const cipherBuf = await crypto.subtle.encrypt(
        { name: 'AES-GCM', iv, tagLength: 128 },
        key,
        new TextEncoder().encode(plainText)
    );
    const combined = new Uint8Array(12 + cipherBuf.byteLength);
    combined.set(iv, 0);
    combined.set(new Uint8Array(cipherBuf), 12);
    return uint8ToBase64(combined);
}

async function decryptPayload(base64, secret) {
    const key   = await deriveKey(secret, 'decrypt');
    const bytes = base64ToUint8(base64);
    const iv          = bytes.slice(0, 12);
    const ciphertext  = bytes.slice(12);
    const decBuf = await crypto.subtle.decrypt(
        { name: 'AES-GCM', iv, tagLength: 128 },
        key,
        ciphertext
    );
    return new TextDecoder().decode(decBuf);
}

// ════════════════════════════════════════════════════════════
// HELPERS
// ════════════════════════════════════════════════════════════
const maskCpf = cpf =>
    cpf ? cpf.replace(/(\d{3})\.\d{3}\.\d{3}-(\d{2})/, '$1.***.***-$2') : '—';
const maskTel = tel =>
    tel ? tel.replace(/(\(\d{2}\))\s?\d{4,5}-(\d{4})/, '$1 *****-$2') : '—';
const fmtTs  = ts =>
    ts ? new Date(ts * 1000).toLocaleString('pt-BR') : '—';
const initials = nome =>
    nome ? nome.split(' ').slice(0, 2).map(w => w[0]).join('').toUpperCase() : '?';

function makeField(label, value, textClass) {
    const div = document.createElement('div');
    div.className = 'field-item';
    div.innerHTML = `<span class="fl">${label}</span><span class="fv${textClass ? ' ' + textClass : ''}">${value ?? '—'}</span>`;
    return div;
}

function appendFields(gridId, pairs) {
    const grid = document.getElementById(gridId);
    pairs.forEach(([label, value, cls]) => grid.appendChild(makeField(label, value, cls)));
}

// ════════════════════════════════════════════════════════════
// STATUS BAR
// ════════════════════════════════════════════════════════════
function setSb(id, text, state) {
    const el = document.getElementById(id);
    el.textContent = text;
    el.className = 'sb' + (state ? ' ' + state : '');
}

// ════════════════════════════════════════════════════════════
// SECRET TOGGLE / COPY
// ════════════════════════════════════════════════════════════
let secretRevealed = false;

function toggleSecret() {
    secretRevealed = !secretRevealed;
    document.getElementById('secretDisplay').textContent =
        secretRevealed ? PACKAGE_SECRET : '••••••••••••••••••••••••';
    document.getElementById('btnReveal').textContent =
        secretRevealed ? 'Ocultar' : 'Revelar';
}

function copySecret() {
    navigator.clipboard.writeText(PACKAGE_SECRET).then(() => {
        const btn = document.getElementById('btnCopy');
        btn.textContent = '✓ Copiado';
        setTimeout(() => btn.textContent = 'Copiar', 1600);
    });
}

function expandToken() {
    document.getElementById('tokenBox').classList.add('expanded');
}

// ════════════════════════════════════════════════════════════
// RENDER RESULT
// ════════════════════════════════════════════════════════════
function showResult(morador, payload) {
    document.getElementById('userAvatar').textContent = initials(morador.nome);
    document.getElementById('userName').textContent   = morador.nome   || '—';
    document.getElementById('userTipo').textContent   = morador.tipoAcesso || 'MORADOR';

    appendFields('gridLocalizacao', [
        ['Condomínio',  morador.condominio || (morador.idCondominio ? `#${morador.idCondominio}` : null), 'text'],
        ['ID Condomínio', morador.idCondominio],
        ['Bloco',       morador.nmBloco   || (morador.idBloco ? `#${morador.idBloco}` : null), 'text'],
        ['Unidade',     morador.nmUnidade || (morador.idUnidade ? `#${morador.idUnidade}` : null), 'text'],
        ['ID Morador',  morador.idMorador],
        ['ID Unidade',  morador.idUnidade],
    ]);

    appendFields('gridContato', [
        ['Nome',      morador.nome,          'text'],
        ['CPF',       maskCpf(morador.cpf)],
        ['E-mail',    morador.email,          'text'],
        ['Telefone',  maskTel(morador.telefone)],
    ]);

    appendFields('gridClaims', [
        ['Emitido em',  fmtTs(payload.iat), 'text'],
        ['Expira em',   fmtTs(payload.exp), 'text'],
        ['Audiência',   payload.aud],
        ['JTI',         payload.jti ? payload.jti.substring(0, 18) + '…' : null],
    ]);

    document.getElementById('stepResult').classList.remove('dimmed');
    setSb('sbDecrypt', '🔑 Descriptografia: ✓ OK', 'ok');
    setSb('sbUser',    `👤 ${morador.nome || 'Usuário identificado'}`, 'ok');

    setTimeout(() => document.getElementById('resultCard').classList.add('visible'), 60);
}

function showError(msg) {
    const box = document.getElementById('errorBox');
    box.textContent = '⚠  ' + msg;
    box.classList.add('visible');

    const stepResult = document.getElementById('stepResult');
    stepResult.classList.remove('dimmed');

    const num = document.getElementById('stepResultNum');
    num.textContent = '✕';
    num.className = 'step-num fail';
    document.getElementById('stepResultTitle').textContent = 'Falha na Identificação';
    document.getElementById('stepResultDesc') && (document.getElementById('stepResultDesc').textContent = 'Não foi possível descriptografar o pacote.');

    setSb('sbDecrypt', '🔑 Descriptografia: ✗ ERRO', 'error');
    setSb('sbUser',    '👤 Usuário: não identificado', 'error');
}

// ════════════════════════════════════════════════════════════
// MAIN
// ════════════════════════════════════════════════════════════
async function main() {
    // — Aplicar nome do parceiro ao header —
    document.title = PARTNER_NAME + ' — Acesso via Lello';
    document.getElementById('partnerNameEl').textContent  = PARTNER_NAME;
    document.getElementById('partnerLogoEl').textContent  = PARTNER_INITIALS;

    // — 1. Obter payload da URL ou gerar demo —
    const params     = new URLSearchParams(window.location.search);
    const urlPayload = params.get('payload');
    let encryptedPayload;

    if (urlPayload) {
        encryptedPayload = urlPayload;
        setSb('sbPacket', '📦 Pacote recebido via URL ✓', 'ok');
    } else {
        document.getElementById('demoBanner').classList.add('visible');
        encryptedPayload = await encryptPayload(
            JSON.stringify(buildDemoPayload()),
            PACKAGE_SECRET
        );
        setSb('sbPacket', '📦 Pacote de demonstração gerado ✓', 'ok');
    }

    // — 2. Exibir token criptografado —
    document.getElementById('tokenText').textContent = encryptedPayload;

    // — 3. Ativar step de processamento —
    const stepProcess = document.getElementById('stepProcess');
    stepProcess.classList.remove('dimmed');
    setSb('sbDecrypt', '🔑 Descriptografia: em andamento...', 'pending');

    // iniciar progress bar
    const fill = document.getElementById('progressFill');
    void fill.offsetWidth;
    fill.classList.add('running');

    // — 4. Aguardar e descriptografar —
    await new Promise(r => setTimeout(r, 1500));

    try {
        const jsonText = await decryptPayload(encryptedPayload, PACKAGE_SECRET);
        const payload  = JSON.parse(jsonText);
        const morador  = payload.morador;

        if (!morador) throw new Error('Campo "morador" não encontrado no payload.');

        // Step 2 — mostrar estado de sucesso
        document.getElementById('loadingSection').innerHTML = `
            <div class="step2-ok">
                <div class="check">✅</div>
                <div class="ok-text">Pacote descriptografado com sucesso</div>
                <div class="ok-sub">Algoritmo AES-GCM · chave SHA-256(packageSecret)</div>
            </div>`;
        document.getElementById('stepProcNum').className   = 'step-num success';
        document.getElementById('stepProcNum').textContent = '✓';

        // Step 3 — exibir resultado
        showResult(morador, payload);

    } catch (err) {
        document.getElementById('loadingSection').innerHTML = `
            <div class="step2-ok">
                <div class="check">❌</div>
                <div class="ok-text" style="color:#c0392b">Falha na descriptografia</div>
                <div class="ok-sub">${err.message}</div>
            </div>`;
        document.getElementById('stepProcNum').className   = 'step-num fail';
        document.getElementById('stepProcNum').textContent = '✕';
        showError(`Não foi possível descriptografar o pacote.\n\nVerifique se o Package Secret está correto.\n\nDetalhe técnico: ${err.message}`);
    }
}

main();
</script>
</body>
</html>
