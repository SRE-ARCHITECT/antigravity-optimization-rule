# 📌 Status do Projeto & Direção (antigravity-optimization-rule)

## Últimas Alterações Realizadas

### Sessão 2026-10-12 — Atualização para v2.6 (SEO, GEO, AEO e Google Search Console)
- [x] Atualização da regra global `GEMINI.md` para v2.6 com os **8 pilares consolidados**:
  - **1 a 7**: Otimização & Tokens, Controle Estrito, Windows & PowerShell, Engenharia (SRP 300L), Continuidade de Sessões, Protocolo Pré-Push (Rollback + Backup FIFO) e Segurança de Dados.
  - **8. Protocolo e Checklist de Otimização de Buscas (SEO, GEO, AEO, SERP e Google Search Console)**:
    - Consulta prévia obrigatória à documentação oficial (Google Search Central & Schema.org).
    - Sincronia estrita entre marcação Schema.org em JSON-LD e o conteúdo legível no DOM (`<body>`).
    - Conformidade rigorosa de Merchant Listings e `Product` (`image` obrigatório, `validFrom`, `shippingDetails` com taxa zero mesmo para digitais, e enumeração válida `ReturnByMail` em devoluções).
    - Formatação estrita de vídeo (`VideoObject` com `uploadDate` em ISO 8601 com timezone).
    - Prontidão para motores de IA e Answer Engines (`llms.txt` e liberação de robôs de IA no `robots.txt`).
    - Identidade visual nas SERPs com Favicon oficial da marca (quadrado múltiplo de 48px e multi-resolução `.ico` e `.png`).
    - Integridade de rastreamento: exclusão de rotas privadas do `sitemap.xml` e `metadataBase` explícito.
- [x] Sincronização direta com `C:\Users\TI\.gemini\config\GEMINI.md` e `C:\Users\TI\.gemini\config\rules\GEMINI.md`.
- [x] Atualização da regra global `GEMINI.md` para v2.5 com as 7 seções consolidadas:
  - **1. Otimização & Tokens**: Comunicação exclusiva PT-BR, zero preâmbulos, edições cirúrgicas por blocos/diffs e bloqueio de daemons/compilação em background (Android/Gradle/npm/pip/cargo).
  - **2. Controle Estrito de Alterações**: Regra inegociável de autorização prévia do usuário para código, commits, pushs e deploys.
  - **3. Otimizações Windows & PowerShell**: I/O restrito à pasta do projeto, PowerShell sem scripts inline complexos (evitando travas e falhas de encoding `cp1252`), encerramento de processos em background ao fim de sessões e higiene de pastas temporárias.
  - **4. Engenharia de Software & Arquitetura**: Modularização com limite de 300 linhas por arquivo (SRP), integridade de lockfiles, proibição de engolir erros (`except: pass` / `catch` vazio) e padronização temporal Brasil (UTC no banco, America/Sao_Paulo UTC-3 em exibições/crons).
  - **5. Continuidade de Sessões & Documentação**: Manutenção obrigatória de `BLUEPRINT.md`, `README.md` (com créditos webappdesigner.com.br e LinkedIn), `PRD.md`, `AGENTS.md` e `PROGRESS.md`.
  - **6. Protocolo de Segurança Pré-Push**: Rollback/tag remota no GitHub (`rollback-prod-YYYYMMDD-HHmm`), snapshot local com cópia de `.env` em `backup-producao/` (blindado no `.gitignore`) e política de retenção FIFO dos 5 snapshots mais recentes.
  - **7. Segurança de Dados & Integridade**: Proibição de `.env` no Git, blindagem de webhooks financeiros com validação de assinatura e idempotência, integridade de BD via padrão *Expand and Contract*, validação estática de tipos e proporção 9:16 estrita para Shorts/Reels sem thumbnails horizontais via API.
- [x] Criação de `.gitignore` para proteção de snapshots e variáveis locais.
- [x] Sincronização direta com a pasta de configuração global do IDE (`C:\Users\TI\.gemini\config\GEMINI.md`).

## Estado Atual
- Repositório remoto `SRE-ARCHITECT/antigravity-optimization-rule` atualizado e sincronizado na v2.6.
- Regra global ativa no ambiente do usuário e em projetos conectados.

## Próximos Passos
- [ ] Divulgar a v2.5 para a comunidade dev/vibecoders.
- [ ] Adicionar exemplos práticos de `PROGRESS.md` para diferentes stacks (Android, React/Next.js, Python).
