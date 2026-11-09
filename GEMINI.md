# Diretrizes Gerais, Protocolo Operacional e Governança de IA (GEMINI.md)

## 1. Otimização, Performance e Economia de Tokens
- Comunique-se SEMPRE e EXCLUSIVAMENTE em português do Brasil.
- Seja extremamente conciso, direto e focado em soluções (Zero preâmbulos ou narração de processo).
- Priorize a entrega de código funcional, limpo e direto ao ponto.
- Use a menor quantidade de tokens possível, mantendo a precisão técnica.
- PROIBIDO reescrever arquivos inteiros para alterações pontuais; use edições cirúrgicas por blocos/diffs.
- Priorize ferramentas nativas do ambiente (busca e leitura) antes de disparar processos de terminal.
- PROIBIDO executar comandos de compilação, daemons ou resolução de dependências (Gradle, npm, pip, cargo, etc.) de forma autônoma; execute apenas sob solicitação ou aprovação explícita do usuário.
- Para inspeção de arquivos e código (Android, Gradle, etc.), faça apenas leitura estática sem invocar daemons em background.
- Assuma por padrão que o host está em uso concorrente: mantenha concorrência baixa, sem builds pesados ou downloads em lote sem autorização.
- PROIBIÇÃO ABSOLUTA DE NAVEGADORES AUTOMATIZADOS: Expressamente PROIBIDO abrir ou instanciar navegadores automatizados (Playwright, Puppeteer, Selenium, browser_subagent ou Chromium headless). Testes visuais e navegação são realizados EXCLUSIVAMENTE pelo próprio usuário de forma manual.

## 2. Controle Estrito de Alterações e Autorizações Obrigatórias (Regra Inegociável)
- NUNCA fazer alterações em código, infraestrutura, integrações ou configurações de qualquer projeto sem autorização prévia e explícita do usuário.
- NUNCA fazer commits sem autorização explícita do usuário.
- NUNCA fazer pushs para repositórios remotos sem autorização explícita do usuário.
- NUNCA disparar ou realizar deploys sem autorização explícita do usuário.
- NUNCA sugerir avanço para protocolos de backup, commit, push, build ou deploy sem antes executar testes técnicos de validação comprovando que nada quebrou e que a entrega está 100% funcional.
- As diretrizes contidas neste documento (GEMINI.md) são prioritárias, mandatórias e devem ser rigorosamente respeitadas sem exceção.

## 3. Otimizações para Windows e Contexto Operacional
- WINDOWS I/O: Ao buscar arquivos ou textos, limite a busca à pasta do projeto atual e utilize filtros de extensão específicos, evitando varrer pastas externas ou raízes de disco.
- CANONICIDADE DE LETRA DE UNIDADE NO WINDOWS (DRIVE LETTER CASING): Comandos de terminal, scripts de build (Vite, Rollup, Webpack, Gradle) e referências de caminhos no Windows DEVEM utilizar obrigatoriamente letras de unidade em maiúsculo (ex: `C:\`, `F:\` e nunca `c:\` ou `f:\`), prevenindo falhas silenciosas de resolução como `[vite:html-inline-proxy]`.
- POWERSHELL LIMPO E ROBUSTO: Evite encadear comandos shell pesados ou deixar processos contínuos (watchers/daemons) rodando em background no Windows.
- EXECUÇÃO SEGURA NO TERMINAL: PROIBIDO disparar comandos com scripts inline multi-linha complexos via `python -c` ou `node -e`. Testes e validações devem ser gravados em arquivos temporários/scratch antes da execução para evitar travamentos e falhas de encoding (`cp1252`).
- RESPEITO AO CONTEXTO E LOGS: Ignore arquivos de build, caches, binários e mapas (.map). Para logs (.log), inspecione apenas os trechos estritamente necessários para o diagnóstico.
- LIMPEZA DE PROCESSOS E RECURSOS: Ao concluir testes ou ao finalizar a sessão, encerre servidores de dev/teste e processos em background (Vite, Next.js, Uvicorn, watchers) para liberar portas, CPU e RAM do host.
- HIGIENE DE DIRETÓRIOS TEMPORÁRIOS: Processamentos de áudio, imagem e vídeo devem sempre limpar arquivos intermediários gerados em pastas temporárias (`.tmp/`, `build/`, `scratch/`), preservando apenas o artefato final.
- BLINDAGEM DE WATCHER E BINÁRIOS PESADOS NO IDE: Em ambientes Windows com projetos contendo mídias, builds mobile ou backups, assegurar que a configuração global (`settings.json`) do Antigravity IDE contenha exclusão explícita no `files.watcherExclude` e `search.exclude` para extensões compactadas/binárias (`.zip`, `.rar`, `.7z`, `.apk`, `.aab`, vídeos, `.git/objects/**`), prevenindo travamento do editor, sobrecarga de I/O e quedas do assistente.
- BLINDAGEM DE DAEMONS JAVA/GRADLE E CANAL ESTÁVEL NO IDE: Em ambientes Windows com projetos híbridos ou mobile (Capacitor, Android), as configurações globais (`settings.json`) do Antigravity IDE e VS Code devem manter Java em modo leve (`"java.server.launchMode": "LightWeight"`), desativar importação e auto-detecção do Gradle (`"gradle.autoDetect": "off"`, `"java.import.gradle.enabled": false`) e fixar o canal do assistente em estável (`"geminicodeassist.updateChannel": "Default"` com `"extensions.autoUpdate": false`), prevenindo downloads recorrentes do backend, sobrecarga de I/O e travamentos por daemons concorrentes.

## 4. Engenharia de Software, Arquitetura e Manutenibilidade
- MODULARIZAÇÃO E SRP: Componentes React, rotas de API ou classes de serviço que ultrapassem **300 linhas** devem ser cirurgicamente modularizados, extraindo lógica de negócio para Custom Hooks, Services e schemas separados.
- INTEGRIDADE DE DEPENDÊNCIAS: PROIBIDO alterar lockfiles adicionando pacotes desnecessários sem consulta prévia. Priorizar bibliotecas nativas.
- OBSERVABILIDADE E TRATAMENTO DE ERROS: Expressamente PROIBIDO engolir exceções com `except: pass` ou `catch (e) {}` vazios. Todo bloco de erro deve registrar log contextual com `logger.warning` ou `logger.error`.
- PADRONIZAÇÃO TEMPORAL BRASIL (UTC-3) E RUNNERS CI/CD: Armazenamento em banco e logs sempre em **UTC** (`TIMESTAMPTZ` / ISO 8601). Exibições, relatórios e agendamento de crons em backend e CI/CD (GitHub Actions) devem converter explicitamente para `America/Sao_Paulo` (UTC-3). PROIBIDO utilizar `datetime.now().weekday()` ou `new Date().getDay()` crus em pipelines UTC para evitar desvio de +1 dia após as 21:00 BRT.
- RESILIÊNCIA EM TELEGRAM BOT APIS E MÍDIAS: Em `getUpdates`, processar mensagens novas (`message`, `channel_post`) e editadas (`edited_message`, `edited_channel_post`). Envio de notificações formatadas (Markdown/HTML) deve ter fallback imediato para texto puro se a API retornar HTTP 400 por caracteres especiais não escapados.
- TIMEOUTS DEFENSIVOS EM APIS SERVERLESS E INTEGRAÇÕES: Chamadas externas a serviços de terceiros, APIs governamentais (PNCP, Compras.gov, Receita Federal) disparadas de Edge/Serverless Functions devem implementar timeout defensivo estrito de 8 segundos (ex: `AbortSignal.timeout(8000)`), evitando timeout de 504 Gateway na plataforma.

## 5. Continuidade de Sessões e Documentação Obrigatória do Projeto
- Manter atualizadas as documentações na raiz de cada projeto:
  - `BLUEPRINT.md`: Arquitetura, fluxo de dados, estrutura de diretórios e integrações.
  - `README.md`: Apresentação, instalação/execução e stack. No final, incluir créditos obrigatórios:
    - Dev: [webappdesigner.com.br](https://webappdesigner.com.br)
    - LinkedIn: https://www.linkedin.com/company/webapp-designer
  - `PRD.md`: Requisitos, escopo, regras de negócio e funcionalidades.
  - `AGENTS.md`: Guia de contexto para agentes de IA, convenções, comandos e variáveis.
  - `PROGRESS.md`: Diário de bordo contendo: O que foi feito, Estado atual e Próximos passos.
- Em projetos com `PROGRESS.md`, consulte-o ao iniciar e continue o fluxo de onde parou.

## 6. Protocolo de Segurança: Ponto de Restauração e Backup de Produção (Obrigatório Pré-Push)
- TESTES OBRIGATÓRIOS PRÉ-SUGESTÃO (GATE DE NÃO-REGRESSÃO):
  - Executar bateria completa de testes e validações técnicas (tipagem estática, sintaxe, integridade de componentes, scripts funcionais) ANTES de sequer sugerir ao usuário backup, commit, push, build ou deploy.
- FLUXO OBRIGATÓRIO AO APROVAR PUSH:
  1. **Validação de Não-Regressão**: Confirmar 100% de aprovação nos testes técnicos.
  2. **Checklist SEO/GEO/AEO**: Em projetos web, auditar itens da seção 8 (Sincronia DOM x Schema, Favicon físico, `sitemap.xml`, `llms.txt`).
  3. **Sincronização com CI/CD**: Executar `git fetch` para verificar se runners de CI/CD geraram commits (`[skip ci]`). Se houver, aplicar `git merge origin/<branch>` ou rebase.
  4. **Rollback e Tag Remota no GitHub**: Gerar tag de rollback da versão ativa (`rollback-prod-YYYYMMDD-HHmm`) e enviar ao remoto (`git push origin <tag>`).
  5. **Cópia Local de Segurança com `.env`**: Gerar snapshot em `backup-producao/YYYY-MM-DD/` com código, infra, schemas e variáveis (`.env` / `.env.local`), ignorando `node_modules`, `.next`, caches, mídias e logs pesados.
  6. **Blindagem no `.gitignore`**: Garantir que `backup-producao/` esteja no `.gitignore`.
  7. **Política de Retenção (Rolling Backups)**: Manter no máximo os **5 snapshots mais recentes** em `backup-producao/`, excluindo automaticamente os mais antigos.
  8. **Prosseguimento**: Somente após os passos acima, executar o push e deploy aprovados.

## 7. Segurança de Dados, Segredos e Integridade do Sistema
- GESTÃO DE SEGREDOS E `.env`: NUNCA versionar arquivos `.env`, `.env.local` ou credenciais privadas. Registrar novas chaves apenas em `.env.example`.
- BLINDAGEM DE PAGAMENTOS E WEBHOOKS: Todo webhook financeiro (Mercado Pago, Stripe, Asaas, Cakto) deve validar a assinatura criptográfica (`x-signature` ou token) e implementar trava de idempotência no banco para evitar cobranças/liberações duplicadas.
- INTEGRIDADE DE BANCO (EXPAND AND CONTRACT): PROIBIDO executar comandos destrutivos (`DROP TABLE`, `DROP COLUMN`, `TRUNCATE` ou reset de schema) em bancos de produção. Mudanças devem ser retrocompatíveis: colunas novas devem ser `NULLABLE` ou com `DEFAULT` seguro.
- SINGLETON E POOLING EM SERVERLESS: Conexões com PostgreSQL Serverless (Neon, Supabase) no Next.js devem reutilizar instâncias globais via Singleton em `globalThis` (ex: `globalForDb.conn ?? postgres(DATABASE_URL, { max: 10, idle_timeout: 20 })`). PROIBIDO abrir pools crus por requisição.
- HIGIENE DE PAYLOADS E LGPD: PROIBIDO armazenar mídia bruta ou Base64 pesada em bancos relacionais de chat/leads. Áudios devem ser usados apenas em buffer efêmero para transcrição (STT), persistindo apenas texto e metadados mínimos.
- VALIDAÇÃO DE TIPOS: Antes de apresentar entregas, executar verificação estática de tipagem (`tsc --noEmit`, `py_compile`) para garantir zero erros.

## 8. Protocolo e Checklist de Otimização de Buscas (SEO, GEO, AEO, SERP e Google Search Console)
- OBRIGATÓRIO EM TODOS OS PROJETOS WEB E LANDING PAGES: Antes de aprovar push ou deploy, validar:
  1. **Documentação Oficial (Google Search Central & Schema.org)**: Verificar diretrizes oficiais para o tipo exato de Rich Result. PROIBIDO inventar propriedades ou enums inexistentes.
  2. **Sincronia DOM x Schema**: Toda marcação Schema.org em JSON-LD (`LocalBusiness`, `FAQPage`, `Service`, `Product`, `Organization`, `SoftwareApplication`) DEVE ter correspondência textual direta visível no corpo da página (`<body>`). PROIBIDO manter dados ricos ocultos ou divergentes.
  3. **Merchant Listings e Ofertas**:
     - `Product`: `image` (array de URLs absolutas em alta resolução), `brand` (`@type: "Brand"`) e `sku` são mandatórios.
     - `Offer`: Declarar `price`, `priceCurrency`, `availability` (`https://schema.org/InStock`), `validFrom` e `priceValidUntil`.
     - Frete (`shippingDetails`): Obrigatório mesmo para SaaS/digital (taxa 0 BRL, `DefinedRegion` com `addressCountry`, `deliveryTime` com `handlingTime` e `transitTime` zerados com `unitCode: "DAY"`).
     - Devolução (`hasMerchantReturnPolicy`): `returnMethod` deve usar valores válidos da enum Schema.org (ex: `https://schema.org/ReturnByMail`). NUNCA inventar valores como `ReturnOnline`.
  4. **Formatação de Vídeo (`VideoObject`)**: `uploadDate` no padrão ISO 8601 com fuso (`YYYY-MM-DDTHH:mm:ss-03:00` ou `Z`). Incluir `name`, `description`, `thumbnailUrl`, `contentUrl` e `embedUrl`.
  5. **Prontidão para IAs e Answer Engines (AEO/GEO)**: Manter `llms.txt` na raiz com serviços, localização, diferenciais e preços. Linkar no `<head>` (`<link rel="alternate" type="text/markdown" href="/llms.txt">`) e declarar `llms-txt: <URL>/llms.txt` no `robots.txt`. Liberar crawlers de IA (`GPTBot`, `PerplexityBot`, `Claude-Web`, `Google-Extended`, `anthropic-ai`, `cohere-ai`, `Applebot-Extended`).
  6. **Identidade Visual nas SERPs (Favicon Canônico)**: Presença física obrigatória de `public/favicon.ico` multi-camadas (16px a 256px) além do `icon.png`, eliminando erros 404 e garantindo CTR na busca mobile/desktop.
  7. **Metadados Geográficos e SEO Local (GEO)**: Definir meta tags (`geo.region`, `geo.placename`, `geo.position`, `ICBM`) e coordenadas no Schema.org, delimitando a área de atendimento (`areaServed`).
  8. **Integridade de Rastreamento**: Canonical único (`<link rel="canonical">`), `metadataBase` explícito no Next.js e gerador nativo dinâmico `sitemap.ts` com prioridade e `lastModified`. NUNCA incluir rotas privadas (`/dashboard`, `/painel`, `/admin`, `/checkout`, `/auth`) no `sitemap.xml`.

## 9. Governança, Validação Ativa e Resiliência de Modelos de IA
- AUDITORIA ATIVA DE MODELOS (ZERO MODELOS DESCONTINUADOS): Ao auditar ou refatorar projetos com APIs generativas (Gemini, Groq, OpenAI, Anthropic, OpenRouter), verificar se os identificadores de modelo no código continuam ativos e oficialmente suportados. PROIBIDO manter IDs obsoletos ou alucinados.
- TESTE REAL DE CONECTIVIDADE: Antes de homologar, consultar programaticamente o catálogo ativo (`client.models.list()`) e disparar prompt de teste real.
- FALLBACK MULTI-PROVEDOR EM CASCATA: Assistentes ou motores analíticos em produção devem implementar cascata resiliente (Primário ➔ Secundário ➔ Contingência) para absorver oscilações e limites de cota.
- BLINDAGEM DE LIMITES E TOKENS (OTPM / TPM): Declarar travas de `max_tokens`, `temperature` calibrada e timeouts em chamadas de API, prevenindo estouro de cota por minuto em planos sob demanda.
- OBSERVABILIDADE E PRIVACIDADE DE CREDENCIAIS: Registrar logs com latência, provedor de resolução e taxa de sucesso. PROIBIDO expor chaves de API, tokens ou dados pessoais em logs de erro.

## 10. Aprendizado Contínuo, Síntese de Soluções e Sugestão Ativa de Melhorias
- SÍNTESE ATIVA PÓS-RESOLUÇÃO: Ao solucionar problemas críticos, bugs ou gargalos, sintetizar os aprendizados essenciais (o que falhou, causa raiz e solução adotada).
- SUGESTÃO PRÓ-ATIVA DE INCORPORAÇÃO: Ao final de cada ciclo bem-sucedido, avaliar criticamente se a solução tem caráter geral e sugerir ativamente sua incorporação às diretrizes globais (`GEMINI.md`), regras ou docs.
- CICLO DE RETROALIMENTAÇÃO (FLYWHEEL DE CONHECIMENTO): Uma vez aprovada pelo usuário, a nova diretriz deve ser registrada de forma permanente nos repositórios de governança e no `.gemini/config/`.
- SINCRONIZAÇÃO MANDATÓRIA DE DIRETRIZES GLOBAIS: Ao atualizar o `GEMINI.md`, é OBRIGATÓRIO sincronizar simultaneamente:
  1. `C:\Users\TI\.gemini\config\rules\GEMINI.md` e `C:\Users\TI\.gemini\config\GEMINI.md`
  2. Repositório oficial de governança `C:\Users\TI\.gemini\config\rules\antigravity-optimization-rule\` com tag de rollback e push para `https://github.com/SRE-ARCHITECT/antigravity-optimization-rule`.
  3. Cópia local de espelho em `F:\Documents\GitClone\GEMINI MD\GEMINI.md` e `F:\Documents\GitClone\GEMINI MD\rules\GEMINI.md`.
- HIGIENE E NÃO-REGRESSÃO: Novas políticas não devem conflitar com as existentes, priorizando resiliência, segurança e respeito ao teto de 24.000 bytes do Antigravity.

## 11. Otimização Nativa Mobile e Gestão de Memória em Android (Kotlin / Jetpack Compose / Capacitor)
- REDUÇÃO DE PACOTES (ABIS E R8/PROGUARD): Em apps com visão computacional, IA ou código nativo C/C++ (ML Kit, OpenCV), declarar `ndk { abiFilters "armeabi-v7a", "arm64-v8a" }` para eliminar binários x86/x86_64 e ativar `minifyEnabled true`, `shrinkResources true` com `proguard-android.txt` headless, reduzindo o tamanho em mais de 60%.
- HIGIENE DE IMAGENS E MEMÓRIA (PREVENÇÃO DE WARNINGS GOOGLE PLAY): PROIBIDO download manual ou decodificação crua via `HttpURLConnection` ou `BitmapFactory.decodeStream()` diretamente no heap da JVM. Usar bibliotecas modernas com pooling e cache (ex: Coil via `AsyncImage`). Para visão/OCR (ML Kit), usar streaming direto via `InputImage.fromFilePath(context, uri)` sem carregar Bitmaps pesados no heap.
- GERAÇÃO NATIVA DE DOCUMENTOS SEM DEPENDÊNCIAS PESADAS: Relatórios e laudos em PDF devem ser gerados via API nativa `android.graphics.pdf.PdfDocument` com paginação dinâmica e medição vetorial via `TextPaint`, evitando bibliotecas Java legadas de 30MB (iText/Apache PDFBox).
- EXPORTAÇÃO E COMPARTILHAMENTO DE PDF EM APPS HÍBRIDOS: Gerar relatórios binários via worker/canvas offscreen e transmitir ao ambiente nativo via Capacitor Filesystem (`Filesystem.writeFile({ directory: Directory.Cache })`). Compartilhamento via `Share.share()` acionando `FileProvider` nativo. PROIBIDO abrir PDFs diretamente dentro de `<iframe>` ou WebViews do Android.
- DESBLOQUEIO DE ROLAGEM E HIGIENE DE MODAIS EM WEBVIEWS: Modais em WebViews não devem bloquear o scroll do container raiz (`document.body.style.overflow = 'hidden'`) sem restauração determinística ao fechar, prevenindo telas travadas no retorno via botão nativo voltar.
- COEXISTÊNCIA DE ADMOB BANNER COM MODAIS E ZERO ANÚNCIOS DE TESTE EM PRODUÇÃO: Em apps híbridos com AdMob, fixar banners em safe area inferior e implementar listener de eventos de anúncio com `padding-bottom` dinâmico. PROIBIDO utilizar IDs de blocos de teste em builds de release/produção.
- GATEWAY SERVERLESS COM DIAGNÓSTICO ATIVO PARA CLIENTES MÓVEIS: Apps que consomem microsserviços devem passar por rotas proxy com timeout defensivo de 8s, fallback automático e header estruturado de diagnóstico (`x-provider-resolved`), permitindo tolerância a falhas em redes móveis instáveis (3G/4G).

## 12. Copywriting de IA, Automação de Mídias Sociais e Higienização de Links (LinkedIn, X e Buffer)
- HIGIENIZAÇÃO DEFENSIVA DE LINKS (ZERO LINKS QUEBRADOS): Modelos de IA tendem a formatar links em Markdown (`[texto](url)`). APIs de redes sociais (LinkedIn, Buffer GraphQL/MCP, X) não interpretam Markdown no corpo do post, quebrando URLs. Proibir no prompt sintaxe Markdown em URLs e implementar sanitizador determinístico via Regex (`sanitize_social_text`) na camada de saída, removendo colchetes/parênteses colados e deduplicando URLs idênticas consecutivas.
- PADRÃO DE COPYWRITING EXECUTIVO PARA LINKEDIN (HEADLINE EM CAIXA ALTA & CONCISÃO):
  - Linha 1 Obrigatória (Headline de Parada): Frase curta em CAIXA ALTA sintetizando impacto direto ou tese contraintuitiva.
  - Linha 2 Obrigatória: Linha em branco (`\n\n`).
  - Corpo Analítico: 1 a 2 parágrafos com fatos objetivos, estatísticas ou implicações práticas. PROIBIDO clichês motivacionais ou saudações ("Olá rede", "Você sabia?").
  - Linha de Fechamento / CTA: Frase objetiva direcionando para leitura técnica, teste prático ou reflexão.
  - Links: URL limpa na última linha isolada, sem pontuação colada.
  - Hashtags: Máximo 3 a 5 tags específicas do setor no rodapé.
- BASES DE CONHECIMENTO EM JSON (ANTI-ALUCINAÇÃO): Ao gerar campanhas a partir de livros ou manuais, PROIBIDO inventar capítulos ou detalhes de memória. Registrar base estruturada em JSON (`book_reference.json` / `niche_reference.json`) contendo capítulos reais e princípios testados, sorteando elementos dinâmicos com 100% de fidelidade factual.
- ESCALABILIDADE MULTI-PRODUTO (ROUND-ROBIN DETERMINÍSTICO): Onde o catálogo ultrapassar o teto semanal de 7 dias, adotar ciclo Round-Robin contínuo determinístico baseado em dias decorridos de uma data de referência fixa (`(data_atual - data_base).days % N`), assegurando rotação simétrica sem reengenharia de crons.
- FIDELIDADE VISUAL E RENDERIZAÇÃO VETORIAL (ZERO ALUCINAÇÃO TIPOGRÁFICA): Para produtos com laudos técnicos, contratos ou dados estruturados (médicos, jurídicos ou fiscais), PROIBIDO depender exclusivamente de modelos de difusão de imagem (DALL-E, Imagen 3, Midjourney) para simular interfaces ou documentos em Português. Priorizar renderização vetorial das páginas reais em alta definição (`PyMuPDF`/`Pillow`) combinada com mockups reais da aplicação.
- BLINDAGEM DE ASSETS PÚBLICOS (ZERO ERROS 404 NO BUFFER/REDES SOCIAIS): Mídias e banners enviados a APIs de redes sociais NUNCA devem usar URLs de repositórios privados do GitHub (`raw.githubusercontent.com`), que exigem autenticação e causam HTTP 404, descartando a imagem. Hospedar mídias em endpoints públicos definitivos (CDN institucional, bucket R2/S3 público com CORS ou repo 100% público) e validar com status HTTP 200 via `requests.head()`.
- MATRIZ DE COMPATIBILIDADE DE IMAGEM MULTI-CANAL (WEBP VS. PNG/JPEG): Plataformas como Google Business Profile (GBP) rejeitam WebP via APIs de terceiros (Buffer). Fornecer WebP otimizado para LinkedIn e X, mantendo versão PNG ou JPEG de alta resolução para Google Business Profile.
- SEPARAÇÃO ENTRE ENGENHARIA INTERNA E PROPOSTA DE VALOR: PROIBIDO alimentar prompts de marketing para o público geral com jargões técnicos de implementação (ex: "OpenCV, Jetpack Compose, visao a 30fps, PyMuPDF, FastAPI"), exceto se o canal for para desenvolvedores. O prompt deve focar 100% na dor, alívio e benefício real do usuário final.
- CONFORMIDADE EM SAÚDE E LGPD (PRIVACIDADE ON-DEVICE): Apps de saúde/exames não devem ser promovidos como diagnósticos clínicos autônomos por IA. O marketing deve enfatizar facilidade de compreensão do paciente, organização e conformidade rigorosa com a LGPD via processamento 100% on-device (documentos salvos no próprio aparelho, sem tráfego de dados sensíveis na nuvem e sem compartilhamento).
