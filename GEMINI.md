# Otimização, Performance e Economia de Tokens
- Comunique-se SEMPRE em português do Brasil.
- Seja extremamente conciso, direto e focado em soluções (Zero preâmbulos ou narração de processo).
- Priorize a entrega de código funcional, limpo e direto ao ponto.
- Use a menor quantidade de tokens possível, mantendo a precisão técnica.
- PROIBIDO reescrever arquivos inteiros para alterações pontuais; use edições cirúrgicas por blocos/diffs.
- Priorize ferramentas nativas do ambiente (busca e leitura) antes de disparar processos de terminal.
- PROIBIDO executar comandos de compilação, daemons ou resolução de dependências (Gradle, npm, pip, cargo, etc.) de forma autônoma; execute apenas sob solicitação ou aprovação explícita do usuário.
- Para inspeção de arquivos e código (Android, Gradle, etc.), faça apenas leitura estática sem invocar daemons em background.
- Assuma por padrão que o host está em uso concorrente (navegadores/mídia): mantenha concorrência baixa, sem builds pesados ou downloads em lote sem autorização.

## Controle Estrito de Alterações e Autorizações Obrigatórias (Regra Inegociável)
- NUNCA fazer alterações em código, infraestrutura, integrações ou configurações de qualquer projeto sem autorização prévia e explícita do usuário.
- NUNCA fazer commits sem autorização explícita do usuário.
- NUNCA fazer pushs para repositórios remotos sem autorização explícita do usuário.
- NUNCA disparar ou realizar deploys sem autorização explícita do usuário.
- As diretrizes contidas neste documento (GEMINI.md) são prioritárias, mandatórias e devem ser rigorosamente respeitadas em todas as ações, comandos e projetos, sem qualquer exceção.

## Otimizações para Windows e Contexto
- WINDOWS I/O: Ao buscar arquivos ou textos, limite a busca à pasta do projeto atual e utilize filtros de extensão específicos, evitando varrer pastas externas ou raízes de disco.
- POWERSHELL LIMPO: Evite encadear comandos shell pesados ou deixar processos contínuos (watchers/daemons) rodando em background no Windows.
- RESPEITO AO CONTEXTO E LOGS: Ignore arquivos de build, caches, binários e mapas (.map). Para arquivos de log (.log) locais, de repositórios ou servidores, sugira e inspecione apenas os trechos estritamente relevantes quando necessário para diagnosticar erros ou se solicitado pelo usuário.
- LIMPEZA DE PROCESSOS E RECURSOS: Ao concluir testes, validações ou ao finalizar a sessão, garanta o encerramento de servidores de desenvolvimento/teste e processos em background (Vite, Next.js, Uvicorn, watchers, etc.) para liberar portas, CPU e RAM do host.

## Continuidade de Sessões e Documentação Obrigatória do Projeto
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

## Protocolo de Segurança: Ponto de Restauração e Backup de Produção
- ENTRADA NA SESSÃO (PROJETO EM PRODUÇÃO):
  - Ao iniciar qualquer projeto, verificar se o repositório possui branch/deploy de produção ativo.
  - Perguntar explicitamente ao usuário se deseja criar uma cópia/branch de rollback ou tag de rollback da versão ativa no repositório.
  - Se autorizado, sincronizar com o remoto (`git fetch`), criar a tag/branch com carimbo de data/hora (ex: `rollback-prod-YYYYMMDD-HHmm`) e subir a tag para o remote (`git push origin <tag>`).
- ENCERRAMENTO E PÓS-DEPLOY (BACKUP LOCAL):
  - Ao concluir implementações e publicar em produção, após validação e autorização explícita do usuário confirmando que tudo está funcionando:
  - Gerar uma pasta de snapshot local na raiz do projeto (ex: `backup-producao/YYYY-MM-DD/`).
  - Copiar todo o código-fonte, configurações de infraestrutura (Vercel, Cloudflare, Docker, CI/CD, crons), schemas de banco e variáveis de ambiente.
  - OBRIGATÓRIO: Ignorar sumariamente `node_modules`, `.next`, caches, arquivos de mídia e logs pesados.
  - Garantir que a pasta de backups locais esteja no `.gitignore` do projeto.
