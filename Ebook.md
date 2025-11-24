# 🚀 Do Zero ao App: Criando um Sistema Financeiro Serverless

**Autor:** [Seu Nome Aqui]  
**Versão do Projeto:** v19 (Mobile/Firebase)  
**Tecnologias:** HTML5, TailwindCSS, JavaScript (ES6), Google Firebase.

---

## 📑 Sumário

1.  [Introdução](#1-introdução)
2.  [Preparando o Ambiente](#2-preparando-o-ambiente)
3.  [Configurando o Backend (Firebase)](#3-configurando-o-backend-firebase)
4.  [Estrutura do Front-end (Mobile First)](#4-estrutura-do-front-end-mobile-first)
5.  [A Lógica do Sistema (JavaScript)](#5-a-lógica-do-sistema-javascript)
6.  [Publicação e Uso](#6-publicação-e-uso)

---

## 1. Introdução

Neste guia, documentamos a criação de um **Sistema de Controle Financeiro Empresarial**. O objetivo era criar uma aplicação que funcionasse tanto no PC quanto no celular (como um App nativo), sem custos de servidor e com dados sincronizados em tempo real.

### A Arquitetura "Serverless"
Em vez de alugar um servidor (VPS) e configurar banco de dados complexos, utilizamos uma arquitetura moderna:
* **O Site (Front-end):** Arquivos estáticos hospedados no GitHub Pages.
* **O Banco (Back-end):** Google Firebase (Firestore e Authentication), que gerencia os dados e a segurança na nuvem.

---

## 2. Preparando o Ambiente

Para este projeto, a simplicidade foi a chave. Não utilizamos instaladores complexos (como Node.js ou React). Usamos **JavaScript Puro (Vanilla JS)** com Módulos.

### Estrutura de Arquivos
O projeto consiste em apenas dois arquivos principais:
* `index.html`: Contém toda a estrutura, estilo e lógica.
* `README.md`: Documentação básica.

---

## 3. Configurando o Backend (Firebase)

O "motor" do sistema é o Firebase. Siga os passos abaixo para replicar a configuração:

### Passo A: Criar Projeto
1.  Acesse [console.firebase.google.com](https://console.firebase.google.com).
2.  Crie um novo projeto chamado "ControleFinanceiro".

### Passo B: Banco de Dados (Firestore)
1.  No menu **Criação**, selecione **Firestore Database**.
2.  Crie um banco no modo de teste.
3.  **Regras de Segurança (Crítico):** Para garantir que o sistema funcione para sempre e com segurança, altere as regras para:

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
