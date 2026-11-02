# Meu Financeiro Web — versão 2

Aplicativo em HTML, CSS e JavaScript para GitHub Pages, com Firebase Authentication e Cloud Firestore. Sem etapa de build nem dependências de produção instaladas por npm. O Node.js é necessário somente para os testes ou um servidor local de desenvolvimento.

## Estado desta entrega

- Implementado: painel, contas, categorias, receitas/despesas, transferências, rendimentos, parcelas e recorrências, baixas e cancelamentos, cartões/faturas/pagamentos parciais, orçamento, metas, investimentos, calculadoras, CSV, backup JSON e impressão/PDF.
- 39 casos do motor financeiro passaram em testes automatizados.
- A sintaxe dos módulos JavaScript foi verificada.
- A abertura da prévia por navegador não foi autorizada neste ambiente. Layout, navegação e formulários ainda precisam de teste no navegador.
- Login e persistência reais, além das regras Firestore, ainda precisam ser validados no seu projeto. As regras foram preparadas, mas não foram publicadas nem testadas no emulador Firebase nesta sessão.
- Nenhum dado antigo foi apagado e nenhuma alteração foi enviada ao GitHub. Esta é uma versão para revisão, não uma implantação já concluída.

## Primeiro: publicar uma revisão separada

Para experimentar sem substituir a página atual, envie a pasta inteira MeuFinanceiro-Web para a raiz do repositório. A revisão ficará em https://santos1513lopes-lgtm.github.io/controle-financeiro-firebase/MeuFinanceiro-Web/?demo=1 . Esse endereço só funcionará depois que os arquivos forem enviados e o Pages terminar de publicar. O parâmetro demo=1 abre os dados fictícios sem login e sem gravar no Firestore. Não é necessário alterar as regras do Firebase para essa primeira revisão.

Depois de aprovar e testar, siga os passos abaixo para colocar a versão na página principal.

## Publicar no seu GitHub Pages

1. Faça uma cópia dos arquivos atuais do repositório.
2. Extraia o pacote ZIP. No GitHub, abra o repositório controle-financeiro-firebase e use Add file > Upload files.
3. Envie o CONTEÚDO da pasta MeuFinanceiro-Web: index.html, styles.css, icon.svg, a pasta js e os demais arquivos. Não envie apenas o ZIP nem crie uma pasta adicional envolvendo o aplicativo. O index.html precisa ficar na raiz usada pelo Pages.
4. Revise o commit antes de confirmar. Se o Pages estiver configurado para publicar main / (root), esse commit atualizará o site automaticamente após a publicação do GitHub.
5. A configuração pública do seu Firebase foi reaproveitada em js/firebase-config.js. Esse arquivo não contém senha de usuário nem credencial de administrador. Não coloque credenciais de conta de serviço no repositório.
6. No Firebase Console > Authentication > Settings > Authorized domains, confira se santos1513lopes-lgtm.github.io está autorizado. Mantenha o provedor Email/Password habilitado.
7. No Firebase Console > Firestore Database > Rules, revise e publique o conteúdo de firestore.rules. Nenhuma regra é enviada ao Firebase automaticamente por colocar esse arquivo no GitHub.
8. Abra o site, faça primeiro os testes da demonstração e, depois, o login com sua conta. A seção nova fica vazia até cadastrar contas, categorias e lançamentos.

Use uma cópia de revisão do site se quiser testar antes de trocar a página principal. A conta Firebase pode ser a mesma, porque os dados novos usam outro caminho.

## Dados e regras

Os registros novos usam finance_v2/{uid}/accounts, cards, categories, transactions, budgets e goals. O usuário só pode ler/escrever suas próprias subcoleções. As regras também validam campos, tipos, valores e referências básicas.

A coleção transactions antiga não é usada pelo aplicativo novo. O arquivo de regras mantém sua leitura restrita ao proprietário e bloqueia novas gravações nessa coleção. Revise esse impacto antes de publicar as regras: o aplicativo antigo perderá permissão para gravar nela. Nenhum documento é apagado.

As permissões são aplicadas pelo Firestore; não dependem somente do login ou do filtro na tela. A configuração pública do Firebase não substitui as regras.

## Como testar sem acessar sua conta

A tela inicial tem Experimentar demonstração. Os dados são fictícios e ficam em uma chave própria do localStorage deste navegador. Nenhum lançamento da demonstração é enviado ao Firestore. Entre na demonstração, registre algo, navegue e recarregue para conferir persistência local. A demonstração não prova a persistência real no Firebase.

Se usar um servidor local de desenvolvimento, sirva esta pasta por HTTP em localhost. Abrir index.html diretamente como file:// não é uma maneira suportada de executar os módulos. O site publicado pelo Pages usa HTTPS.

## Primeiro uso real

1. Cadastre contas e seus saldos de abertura. O saldo deve ser o valor antes dos movimentos da data inicial. Contas com tipo Investimento ficam separadas do saldo disponível; Poupança/reserva integra o disponível.
2. Cadastre categorias e cartões.
3. Registre receitas/despesas e suas previsões. Para séries mensais, informe quantas ocorrências; o valor integral é repetido em cada uma.
4. Para uma compra no cartão, informe o total e as parcelas. O total é dividido em centavos e o ajuste fica na última parcela. Datas de fim de mês são ajustadas sem pular fevereiro. O mês da primeira fatura pode ser informado manualmente para respeitar o extrato do banco.
5. Em Cartões e faturas, use Pagar para pagar a fatura inteira ou informe um valor menor para pagamento parcial. O pagamento altera o saldo da conta e reduz a dívida, sem duplicar despesas por categoria.
6. Para baixar um previsto, use Baixar e informe a data efetiva. Para baixa parcial de receita/despesa comum, divida o registro em parte liquidada e pendente. Não há baixa parcial automática para esse tipo.
7. Transferências entre contas não são receitas ou despesas. Aportes/resgates são transferências entre a conta bancária e a conta de investimento. Rendimentos são registrados quando efetivamente reconhecidos, não pela simulação.
8. O botão Backup exporta um JSON completo dos registros carregados; salve o arquivo em lugar seguro. Não é backup automático do Firebase e esta versão não possui restauração/importação automática.
9. CSV exporta o filtro de relatórios. Imprimir/PDF abre a impressão do navegador; escolha Salvar como PDF.

## Regras de cálculo e limites conhecidos

- Valores de movimentos são guardados como centavos inteiros; saldos de abertura podem ser negativos.
- Comprometido no orçamento inclui despesas previstas e compras no cartão na competência escolhida, excluindo cancelados. Transferências e pagamentos de fatura não entram de novo.
- A projeção parte do saldo disponível de hoje, inclui vencidos no primeiro mês e considera previsões e faturas em aberto. Transferências previstas para investimentos reduzem o disponível; resgates previstos aumentam. Não presume salários ou gastos que você ainda não registrou.
- Metas têm acumulado manual e não movimentam contas.
- Limite do cartão é informativo. Não há controle automático de limite disponível, juros de rotativo ou compensação automática de crédito entre faturas. O formulário impede pagamentos acima da dívida; créditos podem aparecer ao cancelar/editar uma compra já paga e exigem conferência manual.
- Alterar fechamento/vencimento afeta novas compras; não recalcula datas das parcelas existentes. No dia de fechamento, a compra passa ao ciclo seguinte; confira a regra da administradora.
- A edição e o cancelamento afetam uma única ocorrência. Não há edição/exclusão em lote de toda uma série.
- Operações de séries usam writeBatch e são atômicas. Cada série é limitada a 120 ocorrências. Não há geração automática de novas recorrências com o site fechado.
- As leituras carregam o histórico do usuário ao entrar/atualizar. Não há sincronização em tempo real, trabalho offline com fila, paginação de históricos grandes ou resolução de conflitos de edição simultânea. Para uso pessoal inicial; acompanhe as cotas do Firebase.
- Sem importação OFX/CSV, integração bancária, anexos, notificações ou exclusão definitiva pelo aplicativo.
- As calculadoras usam taxas hipotéticas constantes. Investimentos usam taxa anual efetiva convertida para mensal ou taxa mensal informada, com aportes no início/fim. Não incluem impostos, inflação nem tarifas. Financiamento usa tabela Price e não calcula CET. Resultados nunca geram lançamentos automaticamente.
- Calculadora comum usa uma gramática aritmética, sem eval. Porcentagem é número dividido por 100: 200 * 10% = 20.

## Verificação no navegador antes de uso real

- Entrar/sair, cadastrar conta e recuperar senha com os fluxos do Firebase.
- Conta inicial R$ 1.000; receita paga R$ 500; despesa paga R$ 200: saldo R$ 1.300.
- Compra no cartão R$ 100 em 3 vezes: R$ 33,33 / R$ 33,33 / R$ 33,34. Conferir meses, faturas e orçamento.
- Pagar R$ 10 da primeira fatura: saldo bancário R$ 1.290 e primeira fatura aberta R$ 23,33.
- Aporte R$ 100 para conta Investimento: saldo bancário R$ 1.190, investimento +R$ 100, patrimônio total preservado.
- Previsto não altera saldo atual; baixa altera saldo e retira o mesmo valor da previsão.
- Cancelar exclui o efeito e preserva o histórico. Conferir quando a compra já tiver pagamento associado.
- Abrir no celular e testar menus, formulários, filtros, relatórios e calculadoras.
- Sair, reabrir e confirmar os dados reais no Firestore e o isolamento entre dois usuários de teste.

## Testes de cálculo

Com Node.js instalado, execute npm test ou node tests/finance.test.js. Nenhuma instalação npm é necessária.

## Fontes técnicas consultadas

- https://firebase.google.com/docs/firestore/manage-data/transactions
- https://firebase.google.com/docs/firestore/security/rules-conditions
- https://firebase.google.com/docs/firestore/security/rules-query
