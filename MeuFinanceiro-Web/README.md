# Meu Financeiro Web — guia do aplicativo

Esta pasta contém o aplicativo atual em HTML, CSS e JavaScript.

- [Abrir aplicativo](https://santos1513lopes-lgtm.github.io/controle-financeiro-firebase/MeuFinanceiro-Web/)
- [Abrir demonstração com dados fictícios](https://santos1513lopes-lgtm.github.io/controle-financeiro-firebase/MeuFinanceiro-Web/?demo=1)
- [Documentação principal e origem do projeto](../README.md)
- [Configurar Firebase Authentication e Cloud Firestore](../README.md#configurar-o-firebase)
- [Publicar no GitHub Pages](../README.md#publicar-no-github-pages)
- [Limpar arquivos antigos do GitHub](../README.md#limpar-os-arquivos-antigos-do-github)
- [Limpar dados antigos do Firestore](../README.md#limpar-os-dados-antigos-no-firestore)
- [Regras a publicar no console do Firestore](firestore.rules)
- [Configuração pública do Firebase](js/firebase-config.js)
- [Desenvolvimento e testes](../README.md#desenvolvimento-e-testes)

As instruções completas ficam no README da raiz para manter uma única referência atualizada.

**Importante:** enviar o arquivo de regras ao GitHub não as publica no Firebase. Os registros novos usam o caminho `finance_v2/{uid}/...`. Apagar arquivos do repositório não exclui registros do banco.

O modo de demonstração foi validado pelo proprietário. O motor financeiro passou em 39 casos automatizados; login, persistência e regras devem ser validados separadamente no projeto Firebase.
