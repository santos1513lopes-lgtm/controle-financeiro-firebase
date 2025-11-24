# 🚀 Do Zero ao App: Criando um Sistema Financeiro Serverless

**Versão do Projeto:** v19 (Mobile/Firebase)
**Tecnologias:** HTML5, TailwindCSS, JavaScript (ES6), Google Firebase.

---

## 📑 Sumário

1. [Introdução](#1-introdução)
2. [Preparando o Ambiente](#2-preparando-o-ambiente)
3. [Configurando o Backend (Firebase)](#3-configurando-o-backend-firebase)
4. [Estrutura do Front-end (Mobile First)](#4-estrutura-do-front-end-mobile-first)
5. [A Lógica do Sistema (JavaScript)](#5-a-lógica-do-sistema-javascript)
6. [Publicação e Uso](#6-publicação-e-uso)

---

## 1. Introdução

Neste guia, documentamos a criação de um **Sistema de Controle Financeiro Empresarial**. O objetivo foi criar uma aplicação que funcionasse tanto no PC quanto no celular (com experiência de App nativo), sem custos fixos de servidor e com dados sincronizados em tempo real na nuvem.

### A Arquitetura "Serverless"
Em vez de alugar um servidor (VPS) e configurar bancos de dados complexos, utilizamos uma arquitetura moderna e leve:
* **Front-end (O Site):** Arquivos estáticos (HTML/JS) hospedados gratuitamente no GitHub Pages.
* **Back-end (O Banco):** Google Firebase (Firestore e Authentication), que gerencia os dados, o login e a segurança.

---

## 2. Preparando o Ambiente

Para este projeto, a simplicidade foi a chave. Não utilizamos instaladores complexos (como Node.js, React ou Webpack). Optamos pelo **JavaScript Puro (Vanilla JS)** com Módulos, o que garante leveza e facilidade de manutenção.

### Estrutura de Arquivos
O projeto consiste em apenas dois arquivos principais na raiz do repositório:
* `index.html`: O coração do sistema. Contém toda a estrutura visual, o estilo (CSS) e a lógica (Scripts).
* `README.md`: Documentação básica para apresentação do projeto.

---

## 3. Configurando o Backend (Firebase)

O "motor" do sistema é o Firebase. Abaixo, o passo a passo exato da configuração realizada:

### Passo A: Criação do Projeto
1.  Acesso ao [console.firebase.google.com](https://console.firebase.google.com).
2.  Criação de um novo projeto chamado "ControleFinanceiro".

### Passo B: Banco de Dados (Firestore)
1.  No menu **Criação**, selecionamos **Firestore Database**.
2.  Criamos o banco em modo de teste inicial.
3.  **Regras de Segurança (Crítico):** Para garantir que o sistema funcione para sempre e com segurança total, alteramos as regras na aba "Regras" para:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      // Permite leitura/gravação APENAS se o usuário estiver logado
      allow read, write: if request.auth != null;
    }
  }
}

---

## 4. Estrutura do Front-end (Mobile First)

Utilizamos o framework **TailwindCSS** via CDN para estilizar todo o projeto. O foco principal do desenvolvimento foi a usabilidade em dispositivos móveis (celulares).

### Principais Técnicas de Layout:
* **Grid Responsivo:** O sistema utiliza `grid-cols-1` para celulares (empilhando os cards verticalmente) e `md:grid-cols-3` para desktops (colocando-os lado a lado).
* **Visual Clean:** Uso de fundo claro (`bg-gray-50`), sombras suaves (`shadow-sm`) e cantos arredondados para uma aparência moderna e agradável.
* **Feedback Visual por Cores:**
    * Receitas e valores positivos são exibidos automaticamente em **Verde** (`text-green-600`).
    * Despesas e valores negativos são exibidos em **Vermelho** (`text-red-600`).

### Navegação SPA (Single Page Application)
Criamos um sistema onde o site nunca precisa recarregar a página. Todo o conteúdo é carregado inicialmente, e usamos JavaScript para alternar a visibilidade das seções:
* O script manipula a classe `.hidden` do CSS.
* Ao clicar em uma aba (ex: "Transações"), o sistema esconde as outras seções e mostra apenas a desejada instantaneamente.

---

## 5. A Lógica do Sistema (JavaScript)

O código utiliza **ES Modules** modernos para importar as funções do Firebase diretamente dos servidores do Google, eliminando a necessidade de ferramentas de build complexas.

### Fluxo de Dados e Segurança:
1.  **Inicialização:** O app conecta ao Firebase usando as chaves de API configuradas.
2.  **Auth Observer:** O sistema monitora constantemente se há um usuário logado (`onAuthStateChanged`).
    * *Se Sim:* Exibe o conteúdo do App e busca os dados no banco.
    * *Se Não:* Exibe apenas a tela de Login/Cadastro.
3.  **Busca de Transações:**
    O sistema faz uma consulta segura ao Firestore:
    ```javascript
    // Busca apenas os dados onde o 'user_id' é igual ao ID do usuário logado
    const q = query(collection(db, "transactions"), where("user_id", "==", currentUser.uid));
    ```
4.  **Cálculos do Dashboard:**
    Utilizamos métodos nativos como `.filter()` e `.reduce()` para somar receitas e despesas diretamente no navegador do cliente, garantindo velocidade instantânea.
5.  **Lógica de "Receitas Previstas":**
    O sistema filtra especificamente as transações marcadas como "entrada" (Receita) que ainda possuem o status "pendente".

---

## 6. Publicação e Uso

### Hospedagem Gratuita
O código final (`index.html`) foi enviado para um repositório no **GitHub**. Ativamos a funcionalidade **GitHub Pages** nas configurações do repositório, tornando o site acessível via URL pública e segura (HTTPS).

### Instalação no Celular (Experiência PWA)
O sistema foi desenhado para ser usado como um aplicativo nativo:
1.  O usuário abre o site no navegador do celular (Chrome no Android ou Safari no iOS).
2.  Acessa o menu de opções do navegador.
3.  Seleciona **"Adicionar à Tela Inicial"**.
4.  O sistema passa a rodar em tela cheia, removendo a barra de endereços e comportando-se como um App instalado.
