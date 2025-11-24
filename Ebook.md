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
