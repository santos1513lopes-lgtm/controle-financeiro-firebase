========================================================================
PROJETO: CONTROLE FINANCEIRO (FIREBASE APP EDITION)
Data do Backup: 24/11/2025
Versão: v24 (Layout Mobile Nativo & Barra Inferior)
========================================================================

1. VISÃO GERAL
------------------------------------------------------------------------
Este projeto é um PWA (Progressive Web App) de controle financeiro. 
Diferente das versões anteriores, a v24 foi redesenhada para parecer e 
funcionar como um aplicativo nativo de celular (Android/iOS), mantendo 
a compatibilidade total com Desktop.

2. TECNOLOGIAS
------------------------------------------------------------------------
- Front-end: HTML5, JavaScript (ES Modules).
- Estilização: TailwindCSS (Responsividade) + FontAwesome (Ícones).
- Back-end: Google Firebase (Firestore & Authentication).
- Relatórios: jsPDF (PDF) e Blob (CSV).

3. NOVIDADES DA VERSÃO v24 (MOBILE APP)
------------------------------------------------------------------------
A) Navegação Mobile (Bottom Bar):
   - Substituição das abas do topo por uma barra fixa no rodapé.
   - Ícones intuitivos (Painel, Transações, Futuro, Extrato).
   - Facilita o uso com apenas uma mão (polegar).

B) Listas Otimizadas:
   - Tabelas redesenhadas para telas pequenas.
   - Ocultação inteligente de colunas menos importantes no mobile.
   - Botões de ação (Editar/Excluir) substituídos por ícones compactos.

C) Visual "Clean":
   - Inputs maiores para facilitar o toque.
   - Cores de alto contraste (Verde/Vermelho) para leitura rápida.
   - Botão Flutuante (+) reposicionado para não cobrir a navegação.

4. COMO CONFIGURAR O FIREBASE (RECUPERAÇÃO)
------------------------------------------------------------------------
Caso precise criar um novo banco de dados no futuro:

1. Acesse console.firebase.google.com
2. Crie um projeto novo.
3. Crie um "Firestore Database" (modo de teste).
   - Vá na aba "Regras" e cole: allow read, write: if request.auth != null;
4. Ative o "Authentication" (Provedor E-mail/Senha).
5. Vá em Configurações > Web App e copie a const firebaseConfig.

5. CÓDIGO FONTE COMPLETO (index.html)
------------------------------------------------------------------------
(Copie o código abaixo para o arquivo index.html)

<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no" />
  <title>Controle Financeiro - App v24</title>
  
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/jspdf-autotable@3.5.25/dist/jspdf.plugin.autotable.min.js"></script>
  <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" />
  
  <style>
    body { font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, sans-serif; background:#f8fafc; padding-bottom: 80px; }
    .card { background:#fff;border-radius:12px;padding:16px;box-shadow:0 4px 12px rgba(0,0,0,0.05);margin-bottom:14px; }
    .fab { position:fixed; right:20px; bottom:80px; background:#f59e0b; color:#fff; width:56px; height:56px; border-radius:50%; display:flex; align-items:center; justify-content:center; font-size:24px; cursor:pointer; box-shadow:0 4px 15px rgba(245,158,11,0.4); z-index: 40; }
    .hidden { display:none; }
    table { width:100%; border-collapse:collapse; }
    th, td { padding:12px 8px; border-bottom:1px solid #f1f5f9; text-align:left; vertical-align: middle; font-size: 14px; }
    .btn { padding:8px 16px; border-radius:8px; background:#f59e0b; color:#fff; border:none; cursor:pointer; font-weight: 500; }
    .btn.secondary { background:#6b7280; }
    .text-green-600 { color: #16a34a !important; font-weight: 700; }
    .text-red-600 { color: #dc2626 !important; font-weight: 700; }
    .truncate-text { max-width: 140px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; display: block; }
    .mobile-nav { position: fixed; bottom: 0; left: 0; right: 0; background: white; border-top: 1px solid #e5e7eb; display: flex; justify-content: space-around; padding: 10px 0; z-index: 50; box-shadow: 0 -2px 10px rgba(0,0,0,0.05); }
    .nav-item { display: flex; flex-direction: column; align-items: center; color: #9ca3af; font-size: 10px; background: none; border: none; }
    .nav-item i { font-size: 20px; margin-bottom: 4px; }
    .nav-item.active { color: #f59e0b; font-weight: bold; }
    @media (max-width: 768px) { .desktop-nav { display: none; } .fab { bottom: 80px; } }
    @media (min-width: 769px) { .mobile-nav { display: none; } .desktop-nav { display: flex; } .fab { bottom: 30px; } }
  </style>
</head>
<body>
  <div class="max-w-6xl mx-auto p-4 md:p-6">
    <header class="flex flex-col md:flex-row items-center justify-between mb-6 gap-4">
      <div class="text-center md:text-left w-full">
        <h1 class="text-2xl font-bold text-gray-800">Finanças <span class="text-yellow-500">🔥</span></h1>
        <div id="user-email" class="text-xs text-gray-500 font-mono">Carregando...</div>
      </div>
      <div id="auth-area" class="w-full md:w-auto">
        <div id="login-form" class="card p-4">
            <div class="flex flex-col gap-2">
                <input id="email" placeholder="E-mail" class="p-2 border rounded w-full" />
                <input id="password" type="password" placeholder="Senha" class="p-2 border rounded w-full" />
                <div class="flex gap-2">
                    <button id="btn-login" class="btn w-full">Entrar</button>
                    <button id="btn-signup" class="btn secondary w-full">Criar</button>
                </div>
                <div id="auth-msg" class="text-xs text-center text-red-500"></div>
            </div>
        </div>
        <div id="user-actions" class="hidden flex gap-2 justify-center">
            <button id="btn-refresh-all" class="btn secondary text-xs"><i class="fas fa-sync"></i> Atualizar</button>
            <button id="btn-logout" class="btn secondary text-xs" style="background:#ef4444;">Sair</button>
        </div>
      </div>
    </header>

    <nav class="desktop-nav mb-4 gap-2">
        <button class="tab-btn btn active" data-tab="dashboard">Dashboard</button>
        <button class="tab-btn btn secondary" data-tab="transacoes">Transações</button>
        <button class="tab-btn btn secondary" data-tab="futuras">Contas Futuras</button>
        <button class="tab-btn btn secondary" data-tab="extrato">Extrato</button>
    </nav>

    <div id="app-content" class="hidden">
        <main>
            <section id="dashboard" class="card">
                <div class="flex items-center gap-3 mb-4">
                    <label class="text-sm text-gray-500">Mês:</label>
                    <input id="dash-month" type="month" class="border p-2 rounded bg-gray-50 font-bold text-gray-700" />
                </div>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-3 mb-3">
                    <div class="card bg-green-50 border border-green-100 mb-0"><div class="text-xs text-green-600 uppercase">Receitas</div><div id="total-receitas-mes" class="text-xl font-bold text-green-700">R$ 0,00</div></div>
                    <div class="card bg-red-50 border border-red-100 mb-0"><div class="text-xs text-red-600 uppercase">Despesas</div><div id="total-despesas-mes" class="text-xl font-bold text-red-700">R$ 0,00</div></div>
                    <div class="card bg-blue-50 border border-blue-100 mb-0"><div class="text-xs text-blue-600 uppercase">Saldo Líquido</div><div id="saldo-mes" class="text-xl font-bold text-blue-700">R$ 0,00</div></div>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-3">
                    <div class="card mb-0 border border-gray-100"><div class="text-xs text-gray-500 uppercase">A Receber (Previsto)</div><div id="receitas-previstas" class="text-lg font-bold text-green-600">R$ 0,00</div></div>
                    <div class="card mb-0 border border-gray-100"><div class="text-xs text-gray-500 uppercase">Contas a Pagar</div><div id="contas-a-pagar" class="text-lg font-bold text-red-600">R$ 0,00</div></div>
                    <div class="card mb-0 border border-gray-100"><div class="text-xs text-gray-500 uppercase">Previsão Futura</div><div id="previsao-futura" class="text-lg font-bold text-gray-600">R$ 0,
