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
- PADRONIZAÇÃO TEMPORAL BRASIL (UTC-3): Armazenamento em banco de dados e logs sempre em **UTC** (`TIMESTAMPTZ` / ISO 8601). Exibições, relatórios e agendamento de crons devem obrigatoriamente converter explicitamente para o fuso de Brasília (`America/Sao_Paulo` / UTC-3).

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
    1. **Sincronização com Bots e CI/CD**: Executar obrigatoriamente `git fetch` para verificar se runners de CI/CD (GitHub Actions) geraram commits automáticos de estado no remoto (`[skip ci]`). Caso existam commits à frente, aplicar `git rebase origin/<branch>` antes de prosseguir.
    2. **Rollback e Tag Remota no GitHub**: Gerar a cópia/branch de rollback e a tag da versão ativa em produção diretamente no repositório do GitHub (ex: `rollback-prod-YYYYMMDD-HHmm`) e enviar para o remoto (`git push origin <tag>`).
    3. **Cópia Local de Segurança com `.env`**: Gerar snapshot/cópia da versão na pasta local do projeto (ex: `backup-producao/YYYY-MM-DD/`), copiando todo o código-fonte, configurações de infraestrutura, schemas de banco e variáveis de ambiente (`.env`), ignorando sumariamente `node_modules`, `.next`, caches, arquivos de mídia e logs pesados.
    4. **Blindagem no `.gitignore`**: Assegurar obrigatoriamente que a pasta `backup-producao/` esteja registrada no `.gitignore` do projeto para que os snapshots locais nunca sejam enviados ao GitHub.
    5. **Política de Retenção de Snapshots (Rolling Backups)**: Manter sempre no máximo os **5 snapshots mais recentes** em `backup-producao/`. Qualquer snapshot mais antigo que o 5º deve ser automaticamente excluído para evitar acúmulo de disco e rotação de credenciais.
    6. **Prosseguimento**: Somente após a conclusão com sucesso da tag/rollback no GitHub e do backup local com a política de retenção aplicada, prosseguir com o push, deploy e atualizações gerais.

- ENTRADA NA SESSÃO (PROJETO EM PRODUÇÃO):
  - Ao iniciar qualquer projeto, verificar se o repositório possui branch/deploy de produção ativo.
  - Perguntar explicitamente ao usuário se deseja criar uma cópia/branch de rollback ou tag de rollback da versão ativa no repositório.
  - Se autorizado, sincronizar com o remoto (`git fetch`), criar a tag/branch com carimbo de data/hora (ex: `rollback-prod-YYYYMMDD-HHmm`) e subir a tag para o remote (`git push origin <tag>`).

- ENCERRAMENTO E PÓS-DEPLOY (BACKUP LOCAL):
  - Ao concluir implementações e publicar em produção, após validação e autorização explícita do usuário confirmando que tudo está funcionando:
  - Gerar a pasta de snapshot local na raiz do projeto (ex: `backup-producao/YYYY-MM-DD/`).
  - Copiar todo o código-fonte, configurações de infraestrutura (Vercel, Cloudflare, Docker, CI/CD, crons), schemas de banco e variáveis de ambiente (`.env`).
  - OBRIGATÓRIO: Ignorar sumariamente `node_modules`, `.next`, caches, arquivos de mídia e logs pesados.
  - Garantir a aplicação da política de retenção mantendo apenas os **5 snapshots mais recentes** na pasta `backup-producao/`.
  - Garantir que a pasta de backups locais esteja no `.gitignore` do projeto.

## 7. Segurança de Dados, Segredos e Integridade do Sistema
- GESTÃO DE SEGREDOS E `.env`: NUNCA versionar ou commitar arquivos `.env`, `.env.local`, `.env.production` ou chaves de API / credenciais privadas (Supabase, Firebase, Neon, Mercado Pago, etc.) no Git. Ao adicionar variáveis novas, registrar apenas a chave sem o valor em `.env.example`.
- BLINDAGEM DE PAGAMENTOS E WEBHOOKS: Todo endpoint de webhook financeiro (Mercado Pago, Stripe, Asaas) deve obrigatoriamente validar a assinatura criptográfica (`x-signature` ou token secreto) e implementar trava estrita de idempotência no banco de dados para evitar cobranças ou liberações duplicadas.
- INTEGRIDADE DE BANCO DE DADOS (EXPAND AND CONTRACT): PROIBIDO executar comandos destrutivos (`DROP TABLE`, `DROP COLUMN`, `TRUNCATE` ou reset de schema) em bancos de produção (Neon PostgreSQL, Supabase, etc.) de forma autônoma. Mudanças de schema devem ser sempre retrocompatíveis: colunas novas devem ser criadas inicialmente como `NULLABLE` ou com `DEFAULT` seguro.
- VALIDAÇÃO DE TIPOS E QUALIDADE: Antes de apresentar qualquer entrega como pronta para teste ou produção, realizar verificação estática de tipagem (`tsc --noEmit`, `py_compile` ou equivalente do projeto) para garantir zero erros de compilação.
- PADRÃO PARA VÍDEOS CURTOS (SHORTS/REELS): Em automações de marketing/vídeo, assegurar proporção estrita 9:16 (1080x1920) e duração contida. PROIBIDO injetar thumbnails horizontais 16:9 via API em vídeos verticais para não desqualificá-los do carrossel nativo de Shorts/Reels.
