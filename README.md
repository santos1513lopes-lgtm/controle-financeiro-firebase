# Meu Financeiro Web

Controle financeiro pessoal em **HTML, CSS e JavaScript**, publicado no **GitHub Pages**, com **Firebase Authentication** para login e **Cloud Firestore** para armazenar os registros de cada usuário.

- [Abrir o aplicativo](https://santos1513lopes-lgtm.github.io/controle-financeiro-firebase/MeuFinanceiro-Web/)
- [Experimentar com dados fictícios](https://santos1513lopes-lgtm.github.io/controle-financeiro-firebase/MeuFinanceiro-Web/?demo=1)

O modo de demonstração usa apenas o armazenamento local do navegador. Para guardar registros na nuvem, abra o aplicativo sem `?demo=1` e faça login após configurar o Firebase.

## Como o projeto foi criado

A versão atual nasceu da combinação de dois trabalhos do proprietário do repositório:

1. Uma planilha de finanças pessoais para Excel/Office 2021, com VBA, formulários, parcelas, faturas, orçamento, metas e projeções.
2. O aplicativo web anterior deste repositório, que já usava Firebase e GitHub Pages.

Com auxílio do Codex, as regras financeiras foram reescritas em JavaScript e organizadas em módulos. A interface foi refeita para computador e celular. Foram acrescentados contas individuais, investimentos e simuladores.

O VBA não roda no navegador: a versão web implementa suas próprias regras. O banco novo usa `finance_v2` e não depende da coleção antiga `transactions`.

### Situação da versão

- A demonstração publicada foi testada pelo proprietário, que confirmou seu funcionamento.
- 39 casos automatizados do motor financeiro passaram durante o desenvolvimento.
- A sintaxe JavaScript e as referências aos arquivos locais foram verificadas.
- A autenticação e a gravação reais devem ser verificadas após configurar o Firebase.
- O arquivo de regras está preparado; sua presença no GitHub não comprova que foi publicado no console nem que foi testado no emulador.

## Funcionalidades

| Área | Recursos |
| --- | --- |
| Painel | Saldo disponível, entradas e saídas pagas, patrimônio investido e projeção de 12 meses. |
| Contas | Conta corrente, carteira, poupança/reserva e investimento, com abertura e saldo próprios. |
| Lançamentos | Receitas, despesas, rendimentos, transferências, previsões, baixa, edição e cancelamento. |
| Recorrências | Até 120 ocorrências mensais, incluindo a primeira; cada ocorrência repete o valor informado. |
| Cartões | Fechamento, vencimento, parcelas, faturas e pagamentos parciais. |
| Orçamento | Limites por categoria e competência, com comprometido e disponível. |
| Metas | Valor objetivo, acumulado manual, prazo e progresso. |
| Investimentos | Aportes/resgates por transferência e registro de rendimentos efetivos. |
| Calculadoras | Aritmética, juros compostos com aportes e comparação de compra à vista/financiada. |
| Relatórios | Filtros, CSV, impressão/PDF pelo navegador e backup manual JSON. |

Simulações são estimativas separadas dos registros reais e não geram lançamentos automaticamente.

## Organização do código

O aplicativo está em `MeuFinanceiro-Web/`:

```text
MeuFinanceiro-Web/
├── index.html                 # Estrutura da interface
├── styles.css                 # Aparência e adaptação às telas
├── icon.svg
├── js/
│   ├── app.js                 # Telas, formulários e ações
│   ├── finance.js             # Regras e cálculos financeiros
│   ├── storage.js             # Firebase e demonstração local
│   └── firebase-config.js     # Configuração pública do projeto
├── firestore.rules            # Regras a publicar no Firestore
├── firebase.json              # Referência local ao arquivo de regras
├── package.json               # Comando dos testes
├── tests/finance.test.js      # Testes do motor financeiro
└── README.md                  # Acesso a este guia principal
```

Não há build obrigatório, framework de interface, servidor próprio ou hospedagem no Firebase. O SDK Firebase é importado pelo navegador; as fontes usam serviço externo com fallback local.

## Publicar no GitHub Pages

A estrutura atual mantém o aplicativo na subpasta `MeuFinanceiro-Web`. Não é necessário mover os arquivos para a raiz.

1. Abra **Settings / Configurações → Pages** no repositório.
2. Para publicação a partir de branch, use **Deploy from a branch**, branch `main` e pasta `/(root)`.
3. Mantenha a pasta `MeuFinanceiro-Web` dentro dessa raiz.
4. Confirme que a publicação terminou sem erro, pelo painel Pages ou pela execução correspondente em Actions.
5. Abra o endereço do aplicativo indicado no início deste documento.

No fluxo de publicação por branch, as pastas selecionáveis são a raiz ou `docs/`; não selecione `MeuFinanceiro-Web/` como fonte nessa modalidade. Ela será publicada como subpasta da raiz.

Se o repositório já utiliza um workflow de Pages, preserve essa configuração e confirme qual diretório ele publica antes de trocar a fonte.

[Documentação do GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site).

## Configurar o Firebase

Neste projeto, o banco é **Cloud Firestore**, não **Realtime Database**. “FireDB” aqui se refere ao Firestore.

### 1. Selecionar o projeto e conferir o aplicativo Web

1. Acesse o [Console do Firebase](https://console.firebase.google.com/).
2. Selecione o projeto que você já usa. Não é preciso criar outro para esta versão.
3. Em **Configurações do projeto → Geral → Seus aplicativos**, confira o aplicativo Web.
4. Compare a configuração com [firebase-config.js](MeuFinanceiro-Web/js/firebase-config.js). Todos os campos devem pertencer ao mesmo projeto.
5. Se estiver configurando outro projeto, registre um aplicativo Web e substitua o objeto exportado no arquivo.

A configuração Web é pública. Não coloque senhas pessoais, arquivos de conta de serviço, chaves privadas ou credenciais administrativas nesse arquivo.

[Configuração oficial do Firebase para Web](https://firebase.google.com/docs/web/setup).

### 2. Ativar autenticação

1. Abra **Authentication → Sign-in method / Método de login**.
2. Ative **Email/Password / E-mail e senha**.
3. Em **Authentication → Settings → Authorized domains**, confira o domínio `santos1513lopes-lgtm.github.io`. Use somente o domínio, sem `https://` e sem o caminho do repositório.
4. Abra o aplicativo e use **Criar conta**, ou entre com um cadastro existente.
5. A recuperação de senha é feita pelo botão **Esqueci a senha**.

O cadastro de acesso fica no Firebase Authentication. As contas financeiras cadastradas dentro do aplicativo são outra coisa e ficam no Firestore.

[Autenticação por e-mail e senha](https://firebase.google.com/docs/auth/web/password-auth).

### 3. Criar ou conferir o Cloud Firestore

1. Abra **Firestore Database**.
2. Se o banco já existe, mantenha-o.
3. Se ainda não existir, crie um banco **Cloud Firestore Standard**, identificado como `(default)`, com acesso nativo para SDKs Web/Firebase.
4. Escolha a localização apropriada ao criar o banco. Essa escolha exige atenção porque não pode ser simplesmente alterada depois.
5. Comece com acesso restrito/produção e publique as regras do projeto na etapa seguinte. Não é necessário deixar o banco em modo de teste aberto.

O código usa o banco `(default)`. Não use outro identificador sem também adaptar o código.

[Início do Cloud Firestore](https://firebase.google.com/docs/firestore/quickstart).

### 4. Publicar as regras

1. Abra [MeuFinanceiro-Web/firestore.rules](MeuFinanceiro-Web/firestore.rules).
2. Copie **o conteúdo inteiro**, sem as linhas de formatação do GitHub.
3. No console, abra **Firestore Database → Rules / Regras**.
4. Guarde uma cópia das regras atuais e revise o impacto descrito abaixo.
5. Substitua pelo conteúdo do arquivo e clique em **Publicar**.
6. Se o console indicar erro, confira se o arquivo inteiro foi copiado antes de testar os cadastros.

**Atenção:** o arquivo permite acesso aos novos registros somente ao dono do UID autenticado. Ele também mantém leitura da coleção antiga `transactions` restrita ao proprietário e **bloqueia gravações do aplicativo antigo** nessa coleção. Nenhum registro é apagado pela publicação dessas regras.

Não substitua as regras por `allow read, write: if request.auth != null;`. Só exigir login não separa os registros de usuários diferentes.

Subir `firestore.rules` ou `firebase.json` para o GitHub **não aplica regras ao Firebase**. A publicação deve ocorrer no console, ou por um fluxo administrativo próprio configurado para isso.

[Regras de acesso do Firestore](https://firebase.google.com/docs/firestore/security/rules-conditions).

### 5. Entender onde os registros ficam

```text
finance_v2/
└── {uid-do-usuario}/
    ├── accounts/       # Contas financeiras e abertura
    ├── cards/          # Cartões
    ├── categories/     # Categorias
    ├── transactions/   # Movimentações
    ├── budgets/        # Limites mensais
    └── goals/          # Metas
```

`uid-do-usuario` é o identificador do Firebase Authentication, não o e-mail.

As coleções e os documentos surgem com a primeira gravação. Não é necessário criar tabelas ou preencher uma estrutura manualmente. O documento pai do UID pode aparecer no console como contêiner sem campos próprios; os registros estão nas subcoleções.

### 6. Testar a leitura e a gravação reais

1. Abra o aplicativo **sem** `?demo=1` e entre na sua conta.
2. Cadastre uma conta financeira de teste com abertura de **R$ 1.000,00**.
3. Cadastre uma categoria de teste.
4. Registre uma receita recebida de **R$ 500,00** e uma despesa paga de **R$ 200,00**, com datas compatíveis com a abertura.
5. O saldo esperado é **R$ 1.300,00**.
6. Clique em **Atualizar**, saia e entre novamente. Os registros devem permanecer.
7. Confira os documentos no caminho `finance_v2/{uid}/...` no console.
8. Para verificar o isolamento, entre com outro usuário de teste: ele não deve ver os registros do primeiro.

A demonstração funcionando não comprova autenticação ou persistência reais. Teste esses fluxos separadamente.

### 7. Uso gratuito e acompanhamento

O plano Spark oferece cotas gratuitas, com limites por serviço. O aplicativo não exige Firebase Hosting nem Cloud Functions. Acompanhe o uso do Firestore no console, porque o histórico é lido ao entrar ou atualizar.

Não há garantia de uso ilimitado sem custo. Confira o [plano e os preços vigentes](https://firebase.google.com/pricing) antes de ativar faturamento ou outros serviços.

## Primeiro uso e regras financeiras

- **Abertura:** saldo anterior aos movimentos da data escolhida. Abertura não é receita.
- **Disponível:** inclui conta corrente, carteira e poupança/reserva. Investimentos aparecem separados.
- **Previsto:** entra na projeção e não altera o saldo atual. A baixa informa a data efetiva e retira o compromisso da previsão.
- **Transferência:** movimenta contas sem virar receita ou despesa. Aportes/resgates usam esse tipo.
- **Rendimento:** registre o valor efetivamente reconhecido; ele não é importado do simulador.
- **Parcelas no cartão:** o valor total é dividido em centavos. O ajuste fica na última parcela. O mês da primeira fatura pode ser informado para coincidir com o banco.
- **Pagamento de fatura:** reduz o saldo da conta e a dívida do cartão. Não crie outra despesa comum para representar o mesmo pagamento.
- **Orçamento:** inclui despesas previstas e compras no cartão na competência; exclui cancelados, transferências e pagamentos de fatura.
- **Cancelamento/edição:** afeta uma ocorrência, preservando o restante da série.
- **Metas:** o acumulado é manual e não movimenta contas.
- **Projeção:** parte do saldo disponível atual e considera apenas compromissos registrados. Vencidos entram no primeiro mês. Aportes futuros reduzem a liquidez; resgates previstos aumentam.

## Limpar os arquivos antigos do GitHub

**Apagar arquivos no GitHub não apaga dados no Firestore nem usuários no Authentication.**

Na raiz, estes arquivos pertencem à versão anterior e podem ser removidos quando você não precisar mais deles:

- `BACKUP_PROJETO_V24.md`.
- `Ebook.md`.
- O conteúdo antigo de `index.html`, desde que você decida como ficará a entrada principal do site.

Mantenha a pasta `MeuFinanceiro-Web/` inteira e este `README.md`.

### Estrutura recomendada após a limpeza

```text
/
├── README.md
├── index.html             # Somente redirecionamento para a pasta nova
└── MeuFinanceiro-Web/
    └── ...                # Aplicativo completo
```

O `index.html` da raiz pode ser substituído por:

```html
<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <meta http-equiv="refresh" content="0; url=./MeuFinanceiro-Web/">
  <title>Meu Financeiro</title>
</head>
<body>
  <p><a href="./MeuFinanceiro-Web/">Abrir Meu Financeiro</a></p>
</body>
</html>
```

Esse é um exemplo para uma alteração posterior; a atualização da documentação, por si só, não substitui o `index.html` antigo.

Se quiser deixar **somente a pasta nova**, o aplicativo continua acessível pelo endereço com `/MeuFinanceiro-Web/`, mas o endereço da raiz não terá mais a entrada antiga do aplicativo. Manter o README e o redirecionamento costuma facilitar o uso e a manutenção.

### Excluir um arquivo pelo site do GitHub

1. Abra o repositório e entre no arquivo exato que quer remover.
2. Use o menu **… → Delete file / Excluir arquivo**.
3. Confira o caminho antes de confirmar.
4. Escreva uma mensagem como `Remove documentação da versão antiga` e confirme o commit.
5. Repita para o outro arquivo antigo.

Não exclua a pasta nova nem seus arquivos `js`, `index.html`, `styles.css`, `firestore.rules` ou `firebase-config.js`. As exclusões normais ficam registradas no histórico Git.

[Documentação sobre exclusão de arquivos](https://docs.github.com/en/repositories/working-with-files/managing-files/deleting-files-in-a-repository).

## Limpar os dados antigos no Firestore

Essa é uma operação **separada e destrutiva**. Só é necessária se você realmente quiser remover os registros antigos; a versão nova já usa um caminho diferente.

1. Confirme primeiro a gravação real da versão nova.
2. No console, confira o projeto correto e abra **Firestore Database → Data / Dados**.
3. Localize a coleção antiga **`transactions` na raiz**.
4. Confirme que são os registros descartáveis do aplicativo anterior.
5. Pelo menu da coleção, use a opção de excluir e revise o caminho mostrado antes de confirmar.

Não confunda:

- `/transactions`: coleção antiga.
- `/finance_v2/{uid}/transactions`: lançamentos da versão nova.

Não exclua `finance_v2`, o banco inteiro, o projeto ou os usuários do Authentication para fazer essa limpeza. A remoção pelo console pode abranger dados aninhados no alvo escolhido.

[Exclusão de dados no Firestore](https://firebase.google.com/docs/firestore/manage-data/delete-data).

## Backup, relatórios e limitações

- **Backup:** exportação manual JSON dos dados carregados. Não é backup automático do Firebase e não há restauração/importação automática nesta versão.
- **CSV:** exporta o filtro dos relatórios.
- **PDF:** usa a impressão do navegador; escolha “Salvar como PDF”.
- **Metas e limite de cartão:** acompanhamento manual/informativo; não há controle automático de limite disponível.
- **Cartões:** não há cálculo de rotativo nem compensação automática de crédito entre faturas. Alterar fechamento/vencimento não muda parcelas já existentes.
- **Baixa parcial:** suportada para faturas. Em receita/despesa comum, divida o registro em parte liquidada e pendente.
- **Séries:** até 120 ocorrências por operação, gravadas em lote atômico. Não há geração automática com o site fechado.
- **Atualização:** dados lidos ao entrar/atualizar; sem sincronização em tempo real, fila offline, paginação de históricos grandes ou resolução de conflitos de edição simultânea.
- **Integrações:** sem OFX/CSV de entrada, conexão bancária, anexos ou notificações.
- **Simulações:** taxas hipotéticas constantes, sem impostos, inflação, tarifas ou CET. Investimentos usam taxa anual efetiva convertida para mensal ou taxa mensal informada, com aportes no início/fim. Financiamento usa Price.
- **Calculadora comum:** aritmética sem `eval`. Porcentagem significa dividir por 100: `200 * 10% = 20`.
- **Segurança:** as regras preparadas não substituem testes reais entre usuários nem revisão das permissões publicadas.

## Desenvolvimento e testes

Não é necessário instalar pacotes npm para usar o site ou executar os testes de cálculo.

Com Node.js instalado:

```bash
cd MeuFinanceiro-Web
npm test
```

Ou:

```bash
node MeuFinanceiro-Web/tests/finance.test.js
```

Os testes cobrem datas de fim de mês, centavos e parcelas, saldos, transferências, faturas parciais, orçamento, projeção e calculadoras. Eles não substituem testes da interface, do login, do Firestore ou de suas regras.

Para desenvolvimento local, use um servidor HTTP (por exemplo, a extensão Live Server do editor). Abrir `index.html` diretamente como `file://` não é o modo suportado para os módulos JavaScript.

## Fontes técnicas

- [GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)
- [Firebase para Web](https://firebase.google.com/docs/web/setup)
- [Authentication por senha](https://firebase.google.com/docs/auth/web/password-auth)
- [Cloud Firestore](https://firebase.google.com/docs/firestore/quickstart)
- [Regras de segurança](https://firebase.google.com/docs/firestore/security/rules-conditions)
- [Gravações em lote](https://firebase.google.com/docs/firestore/manage-data/transactions)
- [Exclusão de dados](https://firebase.google.com/docs/firestore/manage-data/delete-data)
