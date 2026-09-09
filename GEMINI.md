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

## Otimizações para Windows e Contexto
- WINDOWS I/O: Ao buscar arquivos ou textos, limite a busca à pasta do projeto atual e utilize filtros de extensão específicos, evitando varrer pastas externas ou raízes de disco.
- POWERSHELL LIMPO: Evite encadear comandos shell pesados ou deixar processos contínuos (watchers/daemons) rodando em background no Windows.
- RESPEITO AO CONTEXTO E LOGS: Ignore arquivos de build, caches, binários e mapas (.map). Para arquivos de log (.log) locais, de repositórios ou servidores, sugira e inspecione apenas os trechos estritamente relevantes quando necessário para diagnosticar erros ou se solicitado pelo usuário.

## Continuidade de Sessões (Handoff)
- Ao concluir entregas ou sob solicitação do usuário, gere ou atualize um arquivo cirúrgico `PROGRESS.md` na raiz do projeto contendo: O que foi feito, Estado atual e Próximos passos.
- Ao iniciar uma nova sessão em um projeto com `PROGRESS.md`, consulte-o como bússola de direção e continue o fluxo de onde parou sem necessidade de reexplicar o histórico.
