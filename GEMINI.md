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
- Assuma por padrão que o host está em uso concorrente (navegadores/mídia): mantenha concorrência baixa, sem builds pesados ou downloads em lote sem autorização.

## 2. Controle Estrito de Alterações e Autorizações Obrigatórias (Regra Inegociável)
- NUNCA fazer alterações em código, infraestrutura, integrações ou configurações de qualquer projeto sem autorização prévia e explícita do usuário.
- NUNCA fazer commits sem autorização explícita do usuário.
- NUNCA fazer pushs para repositórios remotos sem autorização explícita do usuário.
- NUNCA disparar ou realizar deploys sem autorização explícita do usuário.
- As diretrizes contidas neste documento (GEMINI.md) são prioritárias, mandatórias e devem ser rigorosamente respeitadas em todas as ações, comandos e projetos, sem qualquer exceção.

## 3. Otimizações para Windows e Contexto Operacional
- WINDOWS I/O: Ao buscar arquivos ou textos, limite a busca à pasta do projeto atual e utilize filtros de extensão específicos, evitando varrer pastas externas ou raízes de disco.
- POWERSHELL LIMPO E ROBUSTO: Evite encadear comandos shell pesados ou deixar processos contínuos (watchers/daemons) rodando em background no Windows.
- EXECUÇÃO SEGURA NO TERMINAL: PROIBIDO disparar comandos de terminal com scripts inline multi-linha complexos via `python -c` ou `node -e`. Testes e validações devem ser gravados em arquivos temporários/scratch antes de sua execução para evitar travamentos de console e falhas de encoding (`cp1252`).
- RESPEITO AO CONTEXTO E LOGS: Ignore arquivos de build, caches, binários e mapas (.map). Para arquivos de log (.log) locais, de repositórios ou servidores, inspecione apenas os trechos estritamente relevantes quando necessário para diagnosticar erros ou se solicitado pelo usuário.
- LIMPEZA DE PROCESSOS E RECURSOS: Ao concluir testes, validações ou ao finalizar a sessão, garanta o encerramento de servidores de desenvolvimento/teste e processos em background (Vite, Next.js, Uvicorn, watchers, etc.) para liberar portas, CPU e RAM do host.
- HIGIENE DE DIRETÓRIOS TEMPORÁRIOS: Processamentos de áudio, imagem e vídeo devem sempre limpar arquivos intermediários gerados em pastas temporárias (`.tmp/`, `build/`, `scratch/`) ao concluir a execução, preservando apenas o artefato final e relatórios de estado.

## 4. Engenharia de Software, Arquitetura e Manutenibilidade
- MODULARIZAÇÃO E PRINCÍPIO DE RESPONSABILIDADE ÚNICA (SRP): Evitar arquivos monolíticos. Componentes React, rotas de API ou classes de serviço que ultrapassem **300 linhas** devem ser cirurgicamente modularizados, extraindo lógica de negócio para Custom Hooks, Services e schemas de validação separados.
- INTEGRIDADE DE DEPENDÊNCIAS: PROIBIDO alterar lockfiles (`package-lock.json`, `poetry.lock`, `requirements.txt`) adicionando pacotes desnecessários ou versões incompatíveis sem consulta prévia. Priorizar bibliotecas nativas antes de instalar novos pacotes de terceiros.
- OBSERVABILIDADE E TRATAMENTO DE ERROS: Expressamente **PROIBIDO** engolir exceções com `except: pass` ou `catch (e) {}` vazios. Todo bloco de erro deve, obrigatoriamente, registrar log contextual com nível `logger.warning` ou `logger.error`.
- PADRONIZAÇÃO TEMPORAL BRASIL (UTC-3) E RUNNERS CI/CD: Armazenamento em banco de dados e logs sempre em **UTC** (`TIMESTAMPTZ` / ISO 8601). Exibições, relatórios, seleções de nicho/grade semanal e agendamento de crons em rotinas de backend e runners CI/CD (GitHub Actions, etc.) devem obrigatoriamente converter explicitamente para o fuso de Brasília (`America/Sao_Paulo` / UTC-3). Expressamente **PROIBIDO** utilizar `datetime.now().weekday()` ou `new Date().getDay()` crus em pipelines de CI/CD que operam em UTC, pois execuções noturnas após as 21:00 BRT sofrem desvio silencioso de +1 dia no calendário.
- RESILIÊNCIA EM TELEGRAM BOT APIS E MÍDIAS: Ao consumir atualizações via Telegram Bot API (`getUpdates`), é OBRIGATÓRIO mapear e processar tanto mensagens novas (`message`, `channel_post`) quanto postagens editadas (`edited_message`, `edited_channel_post`), garantindo que mídias ou legendas corrigidas pelo usuário (como inclusão de hashtags de nicho `#tag`) não sejam descartadas. Adicionalmente, todo envio de notificação formatada (`parse_mode: Markdown` ou `HTML`) deve implementar fallback defensivo imediato reenviando o payload como texto puro caso a API retorne HTTP 400 Bad Request por caracteres especiais não escapados.
- TIMEOUTS DEFENSIVOS EM APIS SERVERLESS E INTEGRAÇÕES EXTERNAS: Toda chamada externa a serviços de terceiros, APIs governamentais (PNCP, Compras.gov, Receita Federal) ou gateways disparada a partir de Edge/Serverless Functions (Next.js, Vercel, Cloudflare Workers) DEVE implementar obrigatoriamente timeout defensivo estrito de 8 segundos (ex: `AbortSignal.timeout(8000)`). Isso impede que a função trave até o teto da plataforma (10s a 15s) e retorne erro 504 Gateway Timeout, acionando em tempo hábil o fluxo de fallback ou resposta amigável ao usuário.

## 5. Continuidade de Sessões e Documentação Obrigatória do Projeto
- SEMPRE criar e manter atualizadas as documentações gerais e essenciais na raiz de cada projeto:
  - `BLUEPRINT.md`: Arquitetura do sistema, fluxo de dados, estrutura de diretórios e integrações.
  - `README.md`: Apresentação do projeto, instruções de instalação/execução e stack técnica.
    - **CRÉDITOS OBRIGATÓRIOS:** No final de todo `README.md`, sempre inserir:
      - Dev: [webappdesigner.com.br](https://webappdesigner.com.br)
      - LinkedIn: https://www.linkedin.com/company/webapp-designer
  - `PRD.md`: Requisitos de produto, escopo, regras de negócio e funcionalidades.
  - `AGENTS.md`: Guia de contexto para agentes de IA, convenções de código, comandos rápidos e variáveis.
  - `PROGRESS.md`: Diário de bordo cirúrgico contendo: O que foi feito, Estado atual e Próximos passos.
  - Demais documentações que julgar necessárias para clareza técnica e manutenção do projeto.
- Ao iniciar uma nova sessão em um projeto com `PROGRESS.md`, consulte-o como bússola de direção e continue o fluxo de onde parou sem necessidade de reexplicar o histórico.

## 6. Protocolo de Segurança: Ponto de Restauração e Backup de Produção (Obrigatório Pré-Push)
- FLUXO OBRIGATÓRIO PRÉ-PUSH (AO APROVAR PUSH):
  - Sempre que o usuário autorizar/aprovar o push, ANTES de executá-lo, é obrigatório:
    1. **Checklist de Otimização de Buscas (SEO/GEO/AEO)**: Em projetos web/landing pages, auditar obrigatoriamente os itens da seção 8 (Sincronia DOM x Schema, Favicon físico, `sitemap.xml` com lastmod atualizado, `llms.txt` e crawlers de IA).
    2. **Sincronização Obrigatória com Bots e CI/CD**: Executar obrigatoriamente `git fetch` para verificar se runners de CI/CD (GitHub Actions) ou commits web geraram alterações no remoto (`[skip ci]`). Caso existam commits à frente, aplicar `git merge origin/<branch>` ou rebase antes de prosseguir.
    3. **Rollback e Tag Remota no GitHub**: Gerar a cópia/branch de rollback e a tag da versão ativa em produção diretamente no repositório do GitHub (ex: `rollback-prod-YYYYMMDD-HHmm`) e enviar para o remoto (`git push origin <tag>`).
    4. **Cópia Local de Segurança com `.env`**: Gerar snapshot/cópia da versão na pasta local do projeto (ex: `backup-producao/YYYY-MM-DD/`), copiando todo o código-fonte, configurações de infraestrutura, schemas de banco e variáveis de ambiente (`.env` / `.env.local`), ignorando sumariamente `node_modules`, `.next`, caches, arquivos de mídia e logs pesados.
    5. **Blindagem no `.gitignore`**: Assegurar obrigatoriamente que a pasta `backup-producao/` esteja registrada no `.gitignore` do projeto para que os snapshots locais nunca sejam enviados ao GitHub.
    6. **Política de Retenção de Snapshots (Rolling Backups)**: Manter sempre no máximo os **5 snapshots mais recentes** em `backup-producao/`. Qualquer snapshot mais antigo que o 5º deve ser automaticamente excluído para evitar acúmulo de disco e rotação de credenciais.
    7. **Prosseguimento**: Somente após a conclusão com sucesso da tag/rollback no GitHub e do backup local com a política de retenção aplicada, prosseguir com o push, deploy e atualizações gerais.

## 7. Segurança de Dados, Segredos e Integridade do Sistema
- GESTÃO DE SEGREDOS E `.env`: NUNCA versionar ou commitar arquivos `.env`, `.env.local`, `.env.production` ou chaves de API / credenciais privadas (Supabase, Firebase, Neon, Mercado Pago, etc.) no Git. Ao adicionar variáveis novas, registrar apenas a chave sem o valor em `.env.example`.
- BLINDAGEM DE PAGAMENTOS E WEBHOOKS: Todo endpoint de webhook financeiro (Mercado Pago, Stripe, Asaas, Cakto) deve obrigatoriamente validar a assinatura criptográfica (`x-signature` ou token secreto) e implementar trava estrita de idempotência no banco de dados para evitar cobranças ou liberações duplicadas.
- INTEGRIDADE DE BANCO DE DADOS (EXPAND AND CONTRACT): PROIBIDO executar comandos destrutivos (`DROP TABLE`, `DROP COLUMN`, `TRUNCATE` ou reset de schema) em bancos de produção (Neon PostgreSQL, Supabase, etc.) de forma autônoma. Mudanças de schema devem ser sempre retrocompatíveis: colunas novas devem ser criadas inicialmente como `NULLABLE` ou com `DEFAULT` seguro.
- SINGLETON E POOLING OBRIGATÓRIO EM BANCOS SERVERLESS: Conexões com PostgreSQL Serverless (Neon, Supabase) em ambientes Next.js / Serverless devem obrigatoriamente reutilizar instâncias globais via padrão Singleton armazenado em `globalThis` (ex: `globalForDb.conn ?? postgres(DATABASE_URL, { max: 10, idle_timeout: 20 })`), sendo expressamente PROIBIDO abrir novas conexões ou pools crus a cada ciclo de execução de rota de API, evitando o esgotamento de conexões (*connection pool exhaustion*).
- HIGIENE DE PAYLOADS, TRANSCRIÇÕES E LGPD: Expressamente PROIBIDO armazenar arquivos de mídia brutos ou áudios pesados codificados em Base64 diretamente em colunas de bancos relacionais de chat/leads. Áudios recebidos devem ser consumidos em memória ou buffer efêmero apenas para transcrição (STT), persistindo no banco exclusivamente o texto transcrito e metadados contextuais estritamente necessários, respeitando os princípios de minimização de dados da LGPD.
- VALIDAÇÃO DE TIPOS E QUALIDADE: Antes de apresentar qualquer entrega como pronta para teste ou produção, realizar verificação estática de tipagem (`tsc --noEmit`, `py_compile` ou equivalente do projeto) para garantir zero erros de compilação.

## 8. Protocolo e Checklist de Otimização de Buscas (SEO, GEO, AEO, SERP e Google Search Console)
- OBRIGATÓRIO EM TODOS OS PROJETOS WEB E LANDING PAGES: Antes de aprovar qualquer push ou deploy, validar os seguintes pilares de visibilidade orgânica, motores de IA e conformidade técnica:
  1. **Consulta Obrigatória à Documentação Oficial (Google Search Central & Schema.org):**
     - Antes de implementar ou alterar dados estruturados, metadados ou regras de indexação, verificar SEMPRE as diretrizes oficiais do Google Search Central e a especificação do Schema.org para o tipo exato de Rich Result.
     - Expressamente PROIBIDO assumir ou inventar propriedades ou valores enumerados inexistentes na especificação oficial.
  2. **Sincronia DOM x Schema (Conformidade Estrita com o Google Search Central):**
     - Toda marcação Schema.org em JSON-LD (`LocalBusiness`, `FAQPage`, `Service`, `Product`, `Organization`, `SoftwareApplication`) DEVE ter correspondência textual direta, legível e visível para humanos no corpo da página (`<body>`).
     - Expressamente PROIBIDO manter dados estruturados ricos (ex: FAQs, cidades, serviços, preços) ocultos do usuário no DOM ou divergentes (ex: preço no Schema diferente do preço na tela).
  3. **Conformidade Estrita de Produtos e Listagens do Comerciante (Merchant Listings):**
     - Schema `Product`: Os campos `image` (array de URLs absolutas em alta resolução), `brand` (objeto `@type: "Brand"`) e `sku` (identificador único) são mandatórios para zerar avisos do Search Console.
     - Schema `Offer`: Declarar obrigatoriamente `price`, `priceCurrency`, `availability` (`https://schema.org/InStock`), `validFrom` (data de início de vigência) e `priceValidUntil`.
     - Políticas de Frete (`shippingDetails`): Obrigatório mesmo para produtos/serviços digitais ou SaaS (declarar taxa 0 BRL, `shippingDestination` com `@type: "DefinedRegion"` apontando `addressCountry`, e `deliveryTime` com `handlingTime` e `transitTime` ambos zerados com `unitCode: "DAY"`) para ativação digital imediata e zerar avisos no Search Console.
     - Políticas de Devolução (`hasMerchantReturnPolicy`): O campo `returnMethod` DEVE utilizar exclusivamente valores válidos da enumeração Schema.org (ex: `https://schema.org/ReturnByMail`). NUNCA inventar valores como `ReturnOnline`.
  4. **Formatação Estrita de Vídeo (`VideoObject`):**
     - O campo `uploadDate` DEVE conter data e hora completas no padrão ISO 8601 com fuso horário (ex: `YYYY-MM-DDTHH:mm:ss-03:00` ou `Z`).
     - Incluir obrigatoriamente `name`, `description`, `thumbnailUrl`, `uploadDate`, `contentUrl` e `embedUrl`.
  5. **Prontidão para IAs, Answer Engines e RAG (AEO / GEO):**
     - Manter e atualizar o arquivo de manifesto de IA `llms.txt` na raiz com serviços, localização, contatos, diferenciais e preços.
     - Garantir a descoberta do arquivo no `<head>` via `<link rel="alternate" type="text/markdown" href="/llms.txt" title="Contexto LLM">` e no `robots.txt` com a diretiva `llms-txt: <URL_ABSOLUTA>/llms.txt`.
     - Liberar explicitamente no `robots.txt` os crawlers das principais IAs generativas (`GPTBot`, `PerplexityBot`, `Claude-Web`, `Google-Extended`, `anthropic-ai`, `cohere-ai`, `Applebot-Extended`).
  6. **Identidade Visual nas SERPs (Favicon Físico Canônico e CTR):**
     - Presença física OBRIGATÓRIA de arquivo binário `public/favicon.ico` na raiz pública além dos ícones dinâmicos do framework (`icon.png`), eliminando retornos HTTP 404 em varreduras de robôs de busca.
     - Gerar sempre favicon multi-camadas (16px a 256px) conforme a documentação do Google Search Central, garantindo indexação imediata nas SERPs mobile e desktop.
  7. **Metadados Geográficos e SEO Local (GEO):**
     - Em negócios locais ou regionais, definir explicitamente as meta tags geográficas (`geo.region`, `geo.placename`, `geo.position`, `ICBM`) e coordenadas geográficas no Schema.org, delimitando com precisão a área de cobertura (`areaServed`).
  8. **Integridade de Rastreamento (Sitemap Dinâmico, Canonical e metadataBase):**
     - Garantir URL canônica única (`<link rel="canonical">`) apontando para o domínio definitivo.
     - Configurar `metadataBase` explícito no framework (ex: Next.js) para assegurar resolução de URLs absolutas em Open Graph, Twitter e canonicals.
     - Implementar gerador nativo dinâmico `sitemap.ts` com prioridade e `lastModified` atualizado para acionar a fila prioritária do Googlebot.
     - NUNCA incluir rotas privadas ou protegidas por autenticação (`/dashboard`, `/painel`, `/checkout`, `/admin`, `/auth`, `/login`) no `sitemap.xml`.

## 9. Governança, Validação Ativa e Resiliência de Modelos de IA
- AUDITORIA ATIVA DE MODELOS (ZERO MODELOS DESCONTINUADOS): Ao auditar, refatorar ou inicializar qualquer projeto que consuma APIs de IA generativa (Google Gemini, Groq, OpenAI, Anthropic, OpenRouter, xAI), é OBRIGATÓRIO verificar e validar se os identificadores de modelos (ex: `model: ...`) no código continuam ativos, funcionais e oficialmente suportados pelos provedores. É expressamente PROIBIDO manter identificadores obsoletos, descontinuados ou alucinados.
- TESTE REAL DE CONECTIVIDADE E CONSULTA AO CATÁLOGO OFICIAL: Antes de concluir a auditoria, entrega ou homologação de um projeto com IA, é OBRIGATÓRIO executar consulta programática ao endpoint de catálogo ativo da conta (`client.models.list()`) e disparar prompt de teste real. Proibido assumir nomes de modelos apenas com base em documentações genéricas externas sem confirmação de suporte na chave/plano configurado no projeto.
- PIPELINE DE FALLBACK MULTI-PROVEDOR OBRIGATÓRIO: Qualquer assistente conversacional, concierge ou motor de inferência analítica em produção deve implementar arquitetura de fallback em cascata (ex: Provedor Primário ➔ Provedor Secundário ➔ Provedor de Contingência), garantindo que picos de carga ou oscilações de serviço não interrompam o fluxo do usuário final.
- BLINDAGEM DE LIMITES E TOKENS (OTPM / TPM): Sempre declarar travas defensivas de `max_tokens`, parâmetros de `temperature` calibrados e timeouts em chamadas de API, impedindo que respostas verbosas excedam cotas por minuto (*Output Tokens Per Minute*) em planos gratuitos, básicos ou sob demanda.
- OBSERVABILIDADE E RESGUARDO DE CREDENCIAIS DE IA: Registrar logs contextuais contendo latência da requisição, provedor efetivo de resolução e taxa de sucesso do fallback, sendo expressamente PROIBIDO exibir chaves de API, tokens de autorização ou dados pessoais em logs de erro.

## 10. Aprendizado Contínuo, Síntese de Soluções e Sugestão Ativa de Melhorias
- SÍNTESE ATIVA PÓS-RESOLUÇÃO: Ao solucionar desafios críticos, bugs de integração de terceiros, gargalos de performance ou falhas de configuração em qualquer sessão, o assistente DEVE sintetizar os aprendizados essenciais obtidos (o que falhou, causa raiz e solução definitiva adotada).
- SUGESTÃO PRÓ-ATIVA DE INCORPORAÇÃO: Ao final de cada ciclo de entrega bem-sucedido ou auditoria, o assistente DEVE avaliar criticamente se as soluções implementadas possuem caráter geral e sugerir ativamente ao usuário a incorporação desses aprendizados nas diretrizes globais (`GEMINI.md`), nas regras (`rules/`) ou nas documentações dos projetos.
- CICLO DE RETROALIMENTAÇÃO (FLYWHEEL DE CONHECIMENTO): Uma vez aprovada pelo usuário, a nova diretriz deve ser registrada de forma permanente nos repositórios de governança (`antigravity-optimization-rule`) e nas configurações de ambiente (`.gemini/config/`), retroalimentando todos os projetos futuros para que nenhum erro conhecido seja repetido.
- HIGIENE E NÃO-REGRESSÃO: Novas políticas não devem conflitar com as existentes, priorizando sempre resiliência, segurança de dados, menor uso de tokens e conformidade com o ecossistema do usuário.

