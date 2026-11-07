# Kit gratuito: seu aplicativo funciona. Mas outra pessoa consegue acessar o que não deveria?

Material educativo de Lucy (criadora independente) para quem está começando a criar aplicativos com Claude Code, Codex ou outras ferramentas de IA.

**Revisado em:** 7 de novembro de 2026 (versão 3, testada em laboratório) · **Custo:** gratuito. Este material não pede login, token, senha, pagamento nem dados pessoais.

## O que tem aqui
| Arquivo | Para quê |
|---|---|
| [CHECKLIST.md](CHECKLIST.md) | Lista curta do que revisar antes de colocar o app no ar |
| [PROMPT-REVISAO.md](PROMPT-REVISAO.md) | Prompt para pedir a qualquer IA de programação uma revisão segura |
| [TESTES.md](TESTES.md) | Testes defensivos simples, só no SEU app, em ambiente de teste |
| [FONTES.md](FONTES.md) | Links oficiais usados (OWASP, GitHub, GitBook) |
| [RESULTADOS.md](RESULTADOS.md) | O que aconteceu quando testamos o kit num app de demonstração |

## Os 3 cuidados do vídeo
1. **Chaves secretas:** fora do código e do navegador. Se vazou, apagar não basta: invalide a antiga, crie outra e atualize o app.
2. **Login (autenticação):** dados privados só para quem entrou na conta.
3. **Autorização no servidor:** o servidor confere, a cada pedido, se aquela pessoa pode ver ou mudar aquele dado.

## Testado na prática
Em 7 de novembro de 2026, o kit foi testado num app de demonstração, com dados fictícios, em duas versões: uma com falhas plantadas e outra corrigida.
- **Testes:** a versão com falhas reprovou em 9 dos 11 testes; a corrigida passou nos 11.
- **Prompt:** usado às cegas no Claude, achou as 11 falhas plantadas. Ainda não foi testado em outras IAs.
- **Ajustes:** o laboratório mostrou pontos em que um iniciante podia ter um "passou" falso. Esta versão 2 já traz esses ajustes.

Os detalhes e os limites estão em [RESULTADOS.md](RESULTADOS.md). Passar nos testes do kit não quer dizer que um app está seguro.

## Caso real (resumo)
O incidente **ocorreu em 2 de novembro de 2026**; o post mortem oficial da GitBook foi **publicado em 4 de novembro de 2026**. Segundo ele, uma falha no fluxo de login SSO e um GitHub App mal configurado levaram o invasor a repositórios internos, a uma chave da Cloudflare e a segredos de produção. A GitBook trocou segredos e revogou tokens. As ações indicadas aos clientes **variam conforme a integração**: quem usa a API deve criar um novo token e atualizar seus apps; quem conecta ferramentas de terceiros deve trocar os tokens/segredos relevantes; no Git Sync (GitHub app), a GitBook informou que já fez a troca e não é preciso ação. Detalhes em [FONTES.md](FONTES.md). O comunicado não diz que IA causou o incidente, e este kit também não diz isso.

## Limites importantes
- Isto **não garante** proteção contra invasões e **não é** auditoria, pentest nem conformidade (LGPD etc.).
- O objetivo é aprender e reduzir riscos. Para apps com dados sensíveis ou pagamentos, procure avaliação profissional.
- Teste apenas aplicativos que são seus. Nunca teste sistemas de terceiros.
- Este kit não cobre todos os riscos (ex.: injeção, upload de arquivos, dependências). Exemplos marcados como **[FICTÍCIO]** são inventados, só para ilustrar.

## Se não funcionar no seu caso
- Se o seu app não tem login nem dados privados, só a parte das chaves secretas se aplica.
- Se você não tem acesso ao código, o prompt rende menos. Os testes no navegador continuam valendo.
- O prompt foi testado no Claude. Em outras IAs, o resultado pode variar.
- Este kit é um guia educativo e não tem suporte individual.

## Precisa de uma análise maior?
Se o seu app guarda dados sensíveis (de saúde, financeiros ou dados pessoais de clientes) ou se você quer uma revisão mais completa do projeto, fale comigo:
- Formulário: https://forms.gle/in57A2X3riV1eTp2A
- Instagram: [@luciana.gabriella.9](https://www.instagram.com/luciana.gabriella.9/)

Posso fazer uma revisão orientada do seu projeto ou indicar um especialista para uma auditoria completa. Uma revisão não é auditoria nem garantia de segurança.
