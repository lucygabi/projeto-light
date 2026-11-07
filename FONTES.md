# Fontes oficiais
Todos os links abaixo foram abertos e responderam normalmente em 7 de novembro de 2026 e o conteúdo das três folhas OWASP e do comunicado da GitBook foi lido para conferir o kit (conteúdo pode mudar).

- OWASP Authorization Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html
- OWASP Authentication Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html
- OWASP Secrets Management Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html
- OWASP Top 10:2025, A01 Broken Access Control (edição atual): https://top10.owasp.org/2025/A01_2025-Broken_Access_Control/
- OWASP Top 10, A01 Broken Access Control (edição anterior, 2021): https://owasp.org/Top10/A01_2021-Broken_Access_Control/
- GitHub Docs, secret scanning: https://docs.github.com/en/code-security/secret-scanning/about-secret-scanning
  - Conferido em 7 nov 2026: gratuito e automático em repositórios públicos; varre todo o histórico. Em repositórios privados depende de plano pago (GitHub Secret Protection).
- detect-secrets (scanner gratuito, código aberto, citado pela OWASP): https://github.com/Yelp/detect-secrets
- Supabase Docs, chaves de API: https://supabase.com/docs/guides/api/api-keys
  - Conferido em 7 nov 2026: a chave publishable pode ficar no app, mas só alcança o que as regras de Row Level Security permitem; a chave secret nunca vai para navegador, app publicado ou repositório.
- Firebase Docs, chaves de API: https://firebase.google.com/docs/projects/api-keys
  - Conferido em 7 nov 2026: a apiKey da configuração web não é segredo quando restrita aos serviços do Firebase; a proteção vem das Security Rules e do App Check.
- GitBook, comunicado oficial do incidente (post mortem de 4 nov 2026): https://www.gitbook.com/status/security-incident-2026-11-03
  - Conferido: incidente em 2 nov 2026 (post mortem publicado em 4 nov 2026), falha no login SSO + GitHub App mal configurado deram acesso a repositórios internos, a uma chave da Cloudflare e a segredos de produção. A GitBook trocou segredos e revogou tokens. Ações para clientes variam: API → criar novo token e atualizar apps; ferramentas de terceiros → trocar tokens/segredos relevantes; Git Sync (GitHub app) → sem ação necessária, já trocado pela GitBook. O comunicado não atribui o incidente a IA.

## O que conferimos nas folhas OWASP
- Autorização: menor privilégio, negar por padrão, validar permissão em toda requisição, nunca depender de checagem no cliente, proteger recursos estáticos, testes unitários/integração de autorização, prevenção de IDOR.
- Autenticação: MFA, armazenamento seguro de senhas, gestão de sessão, mensagens de erro genéricas, reautenticação para ações sensíveis.
- Segredos: rotação e revogação, menor privilégio, detecção de segredos, não vazar em pipelines/logs.
- Observação: "CORS não é autorização" é uma orientação geral deste kit, não uma citação dessas folhas.
- OWASP Top 10 A01 e GitHub Docs: na versão 1, só o acesso tinha sido conferido. Na versão 2, o conteúdo foi lido em 7 nov 2026 (edição 2025 do Top 10: falha de controle de acesso continua em 1º lugar).
- Autenticação (OWASP): a mensagem genérica vale também para cadastro e recuperação de senha, e o bloqueio por tentativas deve contar por conta, não por IP.
- Segredos (OWASP): em caso de vazamento, a ordem é revogar primeiro, depois trocar, depois apagar.
