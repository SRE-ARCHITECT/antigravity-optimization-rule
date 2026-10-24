# 📌 Status do Projeto & Direção (antigravity-optimization-rule)

## Últimas Alterações Realizadas

### Sessão 2026-10-18 - Atualização para v3.0 (Copywriting de IA, Sanitização Universal de Links e Sincronização Mandatória)
- [x] Inclusão da **Seção 12: Copywriting de IA, Automação de Mídias Sociais e Higienização de Links (LinkedIn, X e Buffer)**:
  - **Higienização Defensiva de Links:** Sanitizador determinístico via Regex (`sanitize_social_text`) desmontando links Markdown (`[texto](url)`, `[[url]]`), removendo colchetes/parênteses em URLs e deduplicando URLs idênticas, eliminando 100% de links quebrados nas timelines.
  - **Padrão de Copywriting Executivo para LinkedIn:** Linha 1 obrigatória como HEADLINE EM CAIXA ALTA (8 a 12 palavras, sem arrogância e sem menosprezar desenvolvedores ou tecnologias legadas) e teto estrito de 80 a 100 palavras no corpo para alta escaneabilidade móvel.
  - **Bases de Conhecimento Estruturadas em JSON:** Registro formal de arquivos de referência (`book_reference.json`) para sorteio dinâmico de capítulos e princípios duradouros, prevenindo alucinações da IA e garantindo rotação perene de conteúdo.
- [x] Atualização da **Seção 10: Aprendizado Contínuo**:
  - Diretriz mandatória exigindo a sincronização simultânea de todas as instâncias centrais (`C:\Users\TI\.gemini\config`, `F:\Documents\GitClone\GEMINI MD` e o repositório GitHub `antigravity-optimization-rule`).
- [x] Inclusão consolidada da **Seção 11: Otimização Nativa Mobile e Gestão de Memória em Android (Kotlin / Jetpack Compose)**.


### Sessão 2026-10-13 — Atualização para v2.8 (Aprendizado Contínuo, Favicon Físico Canônico e Governança Estendida)
- [x] Atualização da regra global `GEMINI.md` para v2.8 com os **10 pilares consolidados**:
  - **10. Aprendizado Contínuo, Síntese de Soluções e Sugestão Ativa de Melhorias**: Mecanismo ativo de reflexão pós-implementação, identificação proativa de padrões reutilizáveis (SEO/AEO, resiliência de IA, pooling serverless), documentação imediata e proposição explícita de atualização das diretrizes globais e do repositório público.
  - Reforço do **Favicon Físico Canônico** (`public/favicon.ico` multi-camadas 16px a 256px) para zerar erros 404 em crawlers de busca e garantir indexação nas SERPs mobile e desktop.
  - Reforço da **Auditoria Ativa de Modelos de IA** contra o catálogo oficial da conta (`client.models.list()`) e disparo de prompt de teste real contra erros 404/429.
  - Reforço do **Singleton Pattern e Pooling** para conexões Serverless com bancos relacionais (Neon/Supabase) via `globalThis`.
  - Reforço de **Timeouts Defensivos Serverless de 8s** (`AbortSignal.timeout(8000)`) em APIs externas governamentais e integrações críticas.
- [x] Atualização do `README.md` para a versão v2.8 e sincronização com `C:\Users\TI\.gemini\config\GEMINI.md` e `rules\GEMINI.md`.
  - Reforço de **Merchant Listings Zero Avisos**: Inclusão de `brand`, `sku`, `shippingDestination` (região/país) e `handlingTime` (0 dias para SaaS) nas diretrizes da Seção 8.3.
- [x] Sincronização com o repositório remoto `SRE-ARCHITECT/antigravity-optimization-rule` no GitHub.

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


### Sessão 2026-10-23 — Atualização para v3.1 (Otimização Mobile, PDF Nativo, Gestão de Modais e AdMob)
- [x] Atualização da seção 11 da regra global `GEMINI.md` com os pilares de desenvolvimento mobile e Capacitor/WebView:
  - **Exportação e Compartilhamento de PDF em Apps Híbridos (Capacitor/WebView):** Proibição de dependência de `<a download>` ou `doc.save()` em WebViews; obrigatoriedade de conversão para Base64, persistência via `@capacitor/filesystem` ou `@JavascriptInterface` e acionamento da Share Sheet nativa (`@capacitor/share` / `FileProvider`).
  - **Desbloqueio de Rolagem e Higiene de Modais:** Modais inativos devem usar `display: none !important;` para eliminar camadas aceleradas por GPU (`backdrop-filter`) que interceptam gestos de toque no Android WebView. Contenção estrita de formulários com flexbox e `flex-shrink: 0` nos rodapés.
  - **Coexistência de Google AdMob Banner com Modais:** Controle dinâmico de visibilidade com `hideBanner()` e `resumeBanner()` em modais para evitar sobreposição de botões de rodapé e prevenir cliques acidentais. Proibição estrita de identificadores de teste como fallback em releases de produção.
- [x] Sincronização simultânea entre todas as 5 instâncias centrais (`.gemini/config`, `antigravity-optimization-rule` e `GEMINI MD`).

- **Blindagem de Mídias Multi-Canal (HTTP 200), Compatibilidade WebP/PNG e Linguagem Humana no GEMINI.md**:
  - Incorporada diretriz de proibição de URLs raw do GitHub em repositórios privados para publicação social, evitando retornos 404 e descartes de imagem no Buffer.
  - Fixada a matriz de formatos: WebP ultraleve para LinkedIn e X, com PNG/JPEG obrigatório para Google Business Profile.
  - Implementada regra de separação estanque entre engenharia interna e proposta de valor ao usuário final nos prompts de copywriting.
  - Sincronização obrigatória de todas as instâncias centrais e tag remota no repositório de governança `rollback-prod-20261024-0234`.

## Estado Atual
- Repositório remoto `SRE-ARCHITECT/antigravity-optimization-rule` atualizado e sincronizado na v3.0.
- Regra global ativa no ambiente do usuário e em projetos conectados.

## Próximos Passos
- [ ] Divulgar a v2.7 para a comunidade dev/vibecoders.
- [ ] Adicionar exemplos práticos de `PROGRESS.md` para diferentes stacks (Android, React/Next.js, Python).
