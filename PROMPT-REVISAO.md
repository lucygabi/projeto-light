# Prompt de revisão de segurança (para usar em qualquer IA de programação)

Copie o texto abaixo e cole na sua ferramenta (Claude Code, Codex, Cursor ou outra). Antes:
- **Nunca cole chaves, senhas, tokens ou arquivos `.env` reais na IA.** Se aparecerem, substitua por `[REMOVIDO]`.
- Use ambiente local ou de teste (staging), com dados fictícios. **Não rode em produção.**
- A IA pode errar. Confira cada sugestão antes de aplicar.

**Testado às cegas em 7 de novembro de 2026:** num app de demonstração com 11 falhas plantadas, avaliadores que não sabiam onde estavam as falhas usaram só este prompt. Com o Claude, em um modelo grande e em um modelo pequeno, o prompt achou as 11 falhas. Ainda não foi testado no Codex, no Cursor nem em outras IAs. Detalhes em [RESULTADOS.md](RESULTADOS.md).

---
```
Você vai me ajudar a revisar a segurança do MEU aplicativo, de forma defensiva.

Regras:
1. Primeiro identifique a stack real lendo o projeto (linguagem, framework, banco, serviço de login, hospedagem). Não suponha; diga o que confirmou e o que não conseguiu confirmar.
2. Mapeie todas as rotas, APIs, páginas e recursos de dados. Para cada um (inclusive arquivos/uploads), diga: exige login? quem pode ler? quem pode alterar? onde essa regra é verificada (servidor ou só na tela)?
3. Procure segredos no código, front-end, configurações e histórico. Se encontrar, NÃO repita o valor; informe só o arquivo e a linha e mascare o conteúdo.
4. Verifique autorização no servidor: um usuário consegue ler ou alterar dados de outro trocando um ID? Ações de escrita e de admin são checadas no servidor? A regra padrão é negar? A checagem acontece em toda requisição? Usuários têm só as permissões mínimas?
5. Lembre que CORS não é autorização e que MFA não substitui a checagem de permissão.
6. Trabalhe apenas em ambiente local ou de teste, com dados fictícios. Não acesse produção, não use segredos reais, não teste sistemas de terceiros.
7. NÃO aplique correções automaticamente. Liste os problemas por prioridade, explique em linguagem simples e proponha a correção. Eu aprovo antes.
8. Para cada correção, proponha testes automatizados positivos (quem pode, consegue) e negativos (quem não pode, recebe 401/403/404).
9. Ao final, diga o que foi verificado de verdade (com evidência: arquivo, teste, saída) e o que NÃO foi verificado.
10. Diga o que ficou fora do escopo desta revisão (outros tipos de risco não analisados).
11. Deixe claro os limites: esta revisão não é auditoria, pentest nem garantia contra invasões.
```
---
