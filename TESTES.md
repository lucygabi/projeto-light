# Testes defensivos (só no seu app)
Revisado em 7 de novembro de 2026 (versão 2, depois do teste em laboratório; veja [RESULTADOS.md](RESULTADOS.md)). Faça em ambiente local/de teste, com **contas e dados fictícios**. Nunca teste sistemas de outras pessoas.

## Preparação [FICTÍCIO]
- Usuária A: `ana-teste@exemplo.com` com o pedido `pedido-111`.
- Usuário B: `bruno-teste@exemplo.com` com o pedido `pedido-222`.

Faça sempre o par **positivo** (quem pode, consegue) e **negativo** (quem não pode, é barrado).

| # | Teste | Resultado esperado |
|---|---|---|
| 1 | Sem login, abrir uma página/rota com dado privado. Confira a rota de dados na aba Rede (Network) do navegador, não só a tela | Bloqueado (401 ou redireciona para login) |
| 2 | Logada como A, abrir `pedido-111` | Funciona (positivo) |
| 3 | Logada como A, trocar o ID para `pedido-222` | Bloqueado (403/404), sem mostrar dados de B |
| 4 | Logada como A, tentar editar/apagar `pedido-222` | Bloqueado no servidor; dado de B não muda |
| 5 | Usuário comum tentar função de admin | Bloqueado |
| 6 | Depois do logout, reusar a página/sessão antiga. Reenvie o cookie copiado antes de sair: só recarregar a página engana | Bloqueado |
| 7 | Trocar uma chave de teste: invalidar a antiga, criar nova, atualizar o app | App funciona com a nova; a antiga é recusada; a troca foi feita sem mexer no código e a chave nova não aparece no navegador |
| 8 | Login com senha errada para e-mail existente e inexistente | Mesma mensagem genérica e mesmo código HTTP nos dois |
| 9 | Muitas tentativas de login erradas (ex.: 10 seguidas) e, logo depois, a senha certa | Limite/atraso aplicado (ex.: 429); durante o bloqueio, até a senha certa é recusada; outra conta continua entrando |
| 10 | Sem permissão, abrir link direto de arquivo privado (teste sem login e logada como outra pessoa) | Bloqueado |
| 11 | Procurar segredos no código, no front-end e no histórico do Git (secret scanning) | Nenhum encontrado, e o scanner achou um segredo [FICTÍCIO] plantado de propósito (prova de que ele funciona) |
| 12 | Conferir como a senha fica guardada no banco de teste | Aparece só um hash (ex.: começa com `$2b$` ou `$argon2`), nunca a senha |

## Lembretes
- Esconder o botão não conta: o teste é se o **servidor** recusa.
- CORS não é autorização. MFA ajuda no login, mas não substitui os testes 3–5 e 10.
- Se possível, transforme os testes 2–5 em testes automatizados (a OWASP recomenda testes unitários e de integração para a lógica de autorização).
- Esta lista não cobre todos os riscos.
- Scanners de segredo não reconhecem tudo: o detect-secrets, por exemplo, não reconhece nomes em português como `senha`, `chave` ou `segredo`. Leia também o código ou use o [PROMPT-REVISAO.md](PROMPT-REVISAO.md).
- No histórico do Git, escanear só a saída do `git log -p` pode não achar um `.env` apagado. Procure as versões antigas dos arquivos, por exemplo com `git log --all -- .env` e `git show <commit>:.env`.
- Para os testes 4 e 6 você precisa reenviar um pedido: no navegador, aba Rede (Network), clique com o botão direito no pedido e use "Copiar como cURL"; cole no terminal e rode de novo.
- Anote data, teste, resultado e evidência (print ou saída do teste automatizado).

## Status deste kit
Na versão 1, estes testes ainda não tinham sido rodados em nenhum app. Na versão 2 (7 de novembro de 2026), eles foram rodados num app de demonstração com dados fictícios, em uma versão com falhas e outra corrigida: veja [RESULTADOS.md](RESULTADOS.md). Continuam sendo um roteiro para você aplicar no seu app, e o resultado lá depende de como ele foi feito.
