# Checklist antes de colocar o app no ar
Revisado em 7 de novembro de 2026 (versão 2, depois do teste em laboratório; veja [RESULTADOS.md](RESULTADOS.md)). Marque cada item só depois de conferir de verdade. Esta lista é um ponto de partida, **não cobre todos os riscos** e marcar tudo não significa que o app está seguro.

**App novo:** use desde o início, antes do primeiro deploy.
**App já no ar:** revise primeiro o que já existe (rotas, dados reais, chaves antigas e histórico do Git). Se uma chave já esteve exposta, trate como vazada: troque mesmo que tenha sido apagada do código. Teste em cópia/staging, não em produção.

## Chaves secretas
- [ ] Nenhuma chave, senha ou token está no código, no front-end (navegador/app) ou no histórico do Git.
  - Atenção: algumas chaves são públicas de propósito, como a chave publishable (antiga anon) do Supabase ou a apiKey da configuração web do Firebase. Essas podem ficar no front-end, mas aí a proteção depende das regras de acesso (RLS/rules) estarem ativas e testadas. Chaves secretas (secret ou service_role do Supabase, chaves de outras APIs, tokens) nunca vão para o front-end. Fontes em [FONTES.md](FONTES.md).
- [ ] Segredos ficam em variáveis de ambiente ou gerenciador de segredos do provedor.
- [ ] `.env` está no `.gitignore`; existe só um `.env.example` com valores **[FICTÍCIO]**.
- [ ] Varredura de segredos ativada (ex.: secret scanning do GitHub).
  - No GitHub ela é gratuita em repositório público. Em repositório privado de conta pessoal, pode não estar disponível. Alternativa gratuita: rodar um scanner no seu computador (ex.: detect-secrets) e varrer também o histórico do Git.
- [ ] Cada chave tem só as permissões necessárias (menor privilégio) e, se possível, uma chave por serviço.
- [ ] Logs e saídas de build/deploy não mostram segredos.
- [ ] Sei como trocar uma chave: **invalidar a antiga → criar nova → atualizar o app → confirmar que a antiga não funciona.**
  - Essa ordem é a certa quando a chave vazou, mesmo que o app pare por alguns minutos. Numa troca de rotina, se o provedor permitir duas chaves ativas, dá para criar a nova, atualizar o app e só então invalidar a antiga.

## Login (autenticação)
- [ ] Toda página/rota com dado privado exige login.
- [ ] Uso um serviço de login conhecido em vez de inventar o meu.
- [ ] Senhas nunca são guardadas em texto puro (hash adequado, feito pelo serviço de login ou biblioteca confiável).
- [ ] Mensagens de erro de login são genéricas (não revelam se o e-mail existe). Vale também para o cadastro e a recuperação de senha.
- [ ] Há limite de tentativas de login, contado por conta e não só por IP. Durante o bloqueio, até a senha certa é recusada.
- [ ] Ações sensíveis (trocar e-mail/senha) pedem nova confirmação.
- [ ] Sessões expiram e logout funciona de verdade.
- [ ] MFA disponível quando possível (lembrando: MFA não substitui a autorização).

## Autorização no servidor
- [ ] O **servidor** confere, em **toda** requisição, se o usuário pode acessar *aquele* registro (não só se está logado).
- [ ] Trocar um ID na URL ou na requisição não mostra dados de outra pessoa.
- [ ] Ações de escrita (criar, editar, apagar) são verificadas no servidor.
- [ ] Funções de administrador são bloqueadas para usuários comuns.
- [ ] Regra padrão: negar, a menos que seja permitido.
- [ ] Arquivos privados (uploads, PDFs) também exigem permissão.
- [ ] Usuários recebem só as permissões mínimas.
- [ ] Esconder botão na tela **não** é proteção; CORS **não** é autorização.
- [ ] Se uso banco tipo Supabase/Firebase, regras de acesso (RLS/rules) estão ativas e testadas.

## Antes de publicar
- [ ] Rodei os testes de [TESTES.md](TESTES.md) em ambiente de teste, com dados fictícios.
- [ ] Guardei evidências (o que testei, resultado, data).
- [ ] Revisei as mudanças sugeridas pela IA antes de aplicar.
