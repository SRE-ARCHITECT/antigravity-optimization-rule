# 📌 Status do Projeto & Direção (antigravity-optimization-rule)

## Últimas Alterações Realizadas
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
- Repositório remoto `SRE-ARCHITECT/antigravity-optimization-rule` pronto para atualização da v2.5.
- Regra global ativa no ambiente do usuário e em projetos conectados.

## Próximos Passos
- [ ] Divulgar a v2.5 para a comunidade dev/vibecoders.
- [ ] Adicionar exemplos práticos de `PROGRESS.md` para diferentes stacks (Android, React/Next.js, Python).
