# 📌 Status do Projeto & Direção (antigravity-optimization-rule)

## Últimas Alterações Realizadas

### Sessão 2026-10-12 — Atualização para v2.8 (Governança de Modelos de IA, Resiliência e Serverless)
- [x] Atualização da regra global `GEMINI.md` para v2.7 com os **9 pilares consolidados**:
  - **1 a 3**: Otimização & Tokens, Controle Estrito de Alterações e Otimizações Windows/PowerShell.
  - **4. Engenharia de Software & Serverless**: Adição de timeouts defensivos em Serverless Functions (`AbortSignal.timeout(8000)`) contra erros 504 Gateway Timeout.
  - **5. Continuidade de Sessões & Documentação**: Manutenção de `BLUEPRINT.md`, `README.md` (com créditos webappdesigner.com.br e LinkedIn), `PRD.md`, `AGENTS.md` e `PROGRESS.md`.
  - **6. Protocolo de Segurança Pré-Push**: Sincronização obrigatória com bots/CI/CD (`git fetch` e `git rebase`), rollback/tag remota no GitHub (`rollback-prod-YYYYMMDD-HHmm`), snapshot local em `backup-producao/` e retenção FIFO dos 5 backups mais recentes.
  - **7. Segurança de Dados & Serverless**: Singleton e pooling em bancos relacionais serverless (Neon, Supabase) contra esgotamento de conexões, e higiene estrita de payloads de chat/áudios em base64 (LGPD).
  - **8. Otimização de Buscas (SEO, GEO, AEO, SERP & Search Console)**: Sincronia DOM x Schema, Merchant Listings com regras Google Search Central, VideoObject com ISO 8601 completo e prontidão para crawlers de IA (`llms.txt`).
  - **9. Governança, Validação Ativa e Resiliência de Modelos de IA**: Auditoria ativa de modelos (zero modelos descontinuados ou erro 404), teste real de catálogo e conectividade de API, fallback multi-provedor em cascata, controle de limites OTPM/TPM, testes de persona e observabilidade segura.
- [x] Atualização da documentação `README.md` com os novos pilares.
- [x] Sincronização simultânea entre `C:\Users\TI\.gemini\config\rules` e os diretórios do repositório `SRE-ARCHITECT/antigravity-optimization-rule`.

### Sessão 2026-10-12 — Atualização para v2.6 (SEO, GEO, AEO e Google Search Console)
- [x] Consolidação das diretrizes de dados estruturados Schema.org, Merchant Listings, Favicons múltiplos de 48px e manifestos `llms.txt`.

## Estado Atual
- Repositório remoto `SRE-ARCHITECT/antigravity-optimization-rule` atualizado e sincronizado na v2.7.
- Regra global ativa no ambiente do usuário e em projetos conectados.

## Próximos Passos
- [ ] Divulgar a v2.7 para a comunidade dev/vibecoders.
- [ ] Adicionar exemplos práticos de `PROGRESS.md` para diferentes stacks (Android, React/Next.js, Python).
