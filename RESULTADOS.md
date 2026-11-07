# Resultados do teste em laboratório

**Data:** 7 de novembro de 2026
**Onde:** computador isolado, só `localhost`, sem acesso a sistemas de terceiros.
**Dados:** todas as contas, senhas, pedidos, notas fiscais e chaves são **[FICTÍCIO]**. As chaves aparecem mascaradas (`fk_test_****`).

## O que foi testado
Montamos um app pequeno de pedidos (Node + Express) em duas versões, com as mesmas rotas e a mesma tela:
- **com falhas:** 11 falhas plantadas de propósito, do tipo que aparece em apps feitos às pressas;
- **corrigida:** o mesmo app com as proteções.

Usuários fictícios: Ana (dona do pedido-111), Bruno (dono do pedido-222) e um admin. Um provedor de frete falso, também local, servia para testar a troca de chave.

O app de demonstração não faz parte deste kit, porque serviu só para testar o material.

## Os testes do kit
Rodamos os testes do [TESTES.md](TESTES.md) automaticamente nas duas versões, sempre com o par positivo (quem pode, consegue) e o negativo (quem não pode, é barrado). Foram 5 rodadas, e o resultado de cada teste foi o mesmo em todas.

| # | Teste | Versão com falhas | Versão corrigida |
|---|---|---|---|
| 1 | Sem login, rota com dado privado | **Reprovou:** o pedido da Ana abriu sem login (200) | Passou: 401 |
| 2 | Ana abre o pedido dela | Passou (200) | Passou (200) |
| 3 | Ana troca o ID para o pedido do Bruno | **Reprovou:** viu os dados do Bruno (200) | Passou: 404 |
| 4 | Ana edita e apaga o pedido do Bruno | **Reprovou:** mudou o endereço e apagou o pedido (200) | Passou: 404, e o pedido do Bruno ficou intacto |
| 5 | Cliente chama a função de admin | **Reprovou:** recebeu a lista de usuários (200), mesmo com o botão escondido | Passou: 403; o admin recebe 200 |
| 6 | Reusar a sessão depois de sair | **Reprovou:** o cookie antigo continuou valendo (200) | Passou: 401 |
| 7 | Trocar a chave | Passou, **com ressalva**: só funcionou mexendo no código, e a chave nova voltou a aparecer no navegador | Passou: bastou trocar a variável de ambiente |
| 8 | Mensagem de login | **Reprovou:** "Senha incorreta." x "E-mail não cadastrado." | Passou: mesma mensagem e mesmo código |
| 9 | Muitas tentativas | **Reprovou:** 10 erros sem bloqueio, e a senha certa logo depois entrou | Passou: 429 a partir da 6ª tentativa, inclusive para a senha certa |
| 10 | Link direto do arquivo privado | **Reprovou:** a nota fiscal abriu sem login e para outra pessoa (200) | Passou: 401 sem login, 404 para outra pessoa |
| 11 | Procurar segredos | **Reprovou:** chave no servidor, chave no navegador, segredo da sessão e um `.env` apagado no histórico | Passou: nada encontrado, e o scanner achou um segredo plantado de propósito para provar que funcionava |

**Resumo:** a versão com falhas reprovou em 9 dos 11 testes. A corrigida passou nos 11. O teste 12 (senha guardada em hash) entrou nesta versão 2 depois do laboratório, porque nenhum dos 11 testes pegava a senha em texto puro.

## Apagar a chave do código não tira a chave do histórico
Na versão com falhas, alguém commitou um `.env` com uma chave e, depois, apagou o arquivo. Olhando a pasta, parece resolvido, mas o Git guarda a versão antiga:

```
$ git log --all --oneline -- .env
2a90516 ajustes de configuração
e66d1f5 estrutura inicial

$ git show e66d1f5:.env
PORT=3000
EMAIL_API_KEY=ek_t****
```

Duas lições:
1. Escanear só a saída do `git log -p` **não achou** esse `.env`, por um limite do scanner usado (detect-secrets). Extrair as versões antigas de cada arquivo e escanear achou.
2. O conserto de verdade é tratar a chave como vazada: invalidar no provedor e criar outra. Reescrever o histórico é um passo extra, e não ajuda se alguém já copiou o repositório.

## O prompt de revisão, testado às cegas
Avaliadores que não sabiam onde estavam as falhas receberam só o texto do [PROMPT-REVISAO.md](PROMPT-REVISAO.md) e o código do app.

| Quem revisou | App | Resultado |
|---|---|---|
| Claude, modelo grande | Versão com falhas | Achou as 11 falhas plantadas e confirmou rodando o app localmente |
| Claude, modelo pequeno | Versão com falhas | Achou as 11 falhas plantadas, só lendo o código e o histórico |
| Claude, modelo grande | Versão corrigida | Não apontou nenhuma das 11 falhas, o que está certo, porque elas não existem nesta versão. Confirmou a autorização com 51 verificações no app rodando. Apontou só pontos reais de gravidade média ou baixa, e as senhas de teste escritas no README, que existem nas duas versões de propósito para o teste |

As revisões também apontaram pontos reais que não estavam na lista, como a rota de frete aberta, o cookie sem `secure` e a falta de cabeçalhos de segurança. Nenhuma das revisões aplicou correção sem pedir, todas mascararam os segredos e todas explicaram os próprios limites, como o prompt manda.

**Ainda não testado:** Codex, Cursor e outras IAs. O resultado pode mudar de uma ferramenta para outra.

## O que mudou no kit por causa do laboratório
- **Teste 1:** conferir a rota de dados, não só a tela.
- **Teste 6:** reenviar o cookie antigo, porque só recarregar a página engana.
- **Teste 7:** a troca precisa ser feita sem mexer no código. Também ficou separada a troca de chave vazada da troca de rotina.
- **Teste 8:** comparar também o código HTTP.
- **Teste 9:** dizer quantas tentativas fazer e que até a senha certa precisa ser recusada no bloqueio.
- **Teste 10:** testar sem login e logado como outra pessoa.
- **Teste 11:** varrer também o histórico do Git, plantar um segredo fictício para provar que o scanner funciona, e saber que scanners não reconhecem nomes em português como `senha`.
- **Teste 12, novo:** a senha precisa estar guardada em hash.
- **Checklist:** chaves públicas por natureza (Supabase, Firebase), varredura de segredos em repositório privado, cadastro e recuperação de senha, e bloqueio por conta.

## Limites
- É um app de demonstração pequeno, com dados fictícios, rodando só em `localhost`.
- **Não é auditoria, pentest nem garantia de segurança.** Passar nos testes não quer dizer que um app está seguro.
- Não cobre, por exemplo: injeção de SQL, XSS, CSRF, upload de arquivos, dependências vulneráveis, HTTPS, configuração de nuvem e de banco.
- Onde o kit não definia um critério exato (testes 7 a 11), usamos os critérios descritos acima.
- Usamos um único scanner de segredos (detect-secrets 1.5.0). Outros podem achar mais ou menos coisas.
