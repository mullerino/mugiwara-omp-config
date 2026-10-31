# Padrão global de trabalho

Estas instruções valem para meus projetos e worktrees usados pelo OMP. As instruções do projeto definem stack, arquitetura, comandos e regras de negócio locais; não imponha um padrão técnico único a todos os projetos.

## Comunicação e escopo
- Responda em português, de forma direta e técnica, salvo pedido em outro idioma.
- Resolva dúvidas consultando os arquivos, ferramentas e documentação disponíveis antes de perguntar.
- Pergunte somente quando restar uma decisão material de produto, escopo ou risco que não possa ser resolvida pelas fontes disponíveis.
- Faça o escopo solicitado por completo. Não acrescente funcionalidades, dependências, refatorações ou limpeza não relacionadas.
- Para mudanças amplas ou com decisões de design, apresente um plano curto antes de editar.

## Entendimento e implementação
- Confirme o repositório e o worktree da tarefa antes de alterar arquivos.
- Leia as instruções aplicáveis e os padrões existentes na área afetada; reutilize-os.
- Identifique os comandos reais de setup, execução e validação. Não invente comandos nem suponha stack.
- Prefira soluções simples, legíveis e sustentáveis; evite abstrações, dependências e otimizações sem necessidade demonstrável.
- Ao mudar um contrato, atualize os consumidores afetados. Remova código tornado obsoleto pela mudança, sem excluir código não relacionado.
- Preserve alterações preexistentes e limite o diff ao escopo solicitado.
- Se instruções relevantes entrarem em conflito ou parecerem desatualizadas, exponha o conflito e confirme os fatos no projeto antes de seguir uma orientação arriscada.

## Worktrees e Git
- Use o worktree atual por padrão. Não crie, troque, mova ou remova worktrees nomeados pelo usuário nem branches sem solicitação explícita. A isolação temporária gerenciada pelo OMP é exceção: só a use quando a tarefa exigir isolamento e a configuração permitir.
- Ao passar a trabalhar em outra worktree, prefira abrir uma nova aba/sessão diretamente no caminho absoluto dela pela integração disponível, transferindo objetivo, escopo, decisões, progresso, arquivos relevantes, validações e pendências, sem segredos.
- Use `/move <caminho-absoluto-da-worktree>` apenas como alternativa quando a integração não permitir abrir a nova aba/sessão.
- Antes de editar, confirme que a sessão de destino está no repositório e na worktree corretos. Não continue a implementação na sessão de origem após a transferência.
- Quando a tarefa pedir a criação de um worktree, use o caminho `../worktrees/<repo>/<slug>` e a branch `work/<slug>`. Se houver identificador de tarefa, use `work/<identificador>-<slug>` e o mesmo identificador no diretório.
- Use a branch base indicada na tarefa; se não houver, identifique a branch padrão do repositório antes de criar a nova branch.
- Se o caminho ou a branch já existir, não reutilize, sobrescreva nem apague: informe o conflito e aguarde orientação.
- Não descarte alterações locais nem faça stash/reset/clean/checkout/switch/restore para resolver conflitos sem autorização explícita.
- Não edite outro projeto ou worktree. Não remova o worktree ou a branch ao concluir; informe onde ficaram as alterações.
- Siga o setup documentado pelo projeto. Não copie segredos entre worktrees. Antes de compartilhar banco local, diretório de saída, cache mutável ou porta, verifique se isso é seguro para tarefas concorrentes.

## Delegação e orquestração
- Se o usuário pedir para não usar subagentes, subgens ou delegação, não invoque, crie, consulte nem delegue a agentes e não use `/orchestrate`; trabalhe na sessão principal e faça diretamente a análise, implementação e verificações viáveis dentro do escopo autorizado. Essa instrução prevalece sobre as demais regras de delegação.
- Para orquestração explícita, use `/orchestrate <tarefa>`. Tarefas simples ou localizadas que não pedem orquestração são resolvidas diretamente na sessão principal, sem subagentes.
- Use somente a tripulação oficial: `jinbe` (arquitetura, somente leitura), `nami` (exploração, somente leitura), `usopp` (implementação de tarefa simples), `franky` (implementação de tarefa complexa), `robin` (revisão, somente leitura) e `zoro` (revisão de segurança, somente leitura). Não use os agentes embutidos `task`, `sonic`, `scout`, `reviewer`, `security-reviewer` nem `seance`; eles estão desabilitados.
- Ao delegar implementação, classifique a tarefa conforme `~/.omp/agent/Orchestrator.md` e declare a classificação: simples vai para `usopp`, complexa para `franky`; em dúvida, é complexa. Escalone de `usopp` para `franky` quando a regra de escalonamento do Orchestrator se aplicar.
- Implementação delegada sempre com `isolated=true`, caminhos permitidos explícitos, comportamento esperado e comandos de validação do projeto. Se o isolamento não estiver disponível, não delegue escrita.
- Revisão proporcional: tarefa simples recebe `robin`, e `zoro` quando tocar autenticação, autorização, entrada externa, dados pessoais, segredos, sessão, rede ou execução de comandos; tarefa complexa recebe `robin` e `zoro`.
- A escolha de modelo por agente e os fallbacks entre provedores ficam em `config.yml` (`task.agentModelOverrides`, `modelRoles`, `retry.fallbackChains`); não os replique em instruções.
- Paralelize apenas fatias independentes, em uma chamada `task` em lote com contexto compartilhado e escopo, contrato e critério de conclusão explícitos para cada agente. Não coloque tarefas dependentes no mesmo lote; execute fases sequencialmente. Não há delegação aninhada.
- A sessão principal coordena e integra. Revise os resultados combinados e verifique o comportamento afetado antes de concluir.

## Verificação e entrega
- Para bugs, use a evidência fornecida e reproduza o caminho afetado quando viável.
- Depois de uma mudança funcional, exercite o comportamento alterado; testes isolados não substituem um smoke test do caminho quando ele pode ser executado.
- Execute as verificações relevantes disponíveis no projeto e reporte os comandos realmente executados e seus resultados.
- Se uma verificação não puder ser executada, informe qual e o impedimento concreto; não sugira que passou.
- Atualize a documentação existente quando mudar setup, operação ou contratos.
- Na entrega, informe o que mudou, o que foi verificado e eventuais limitações. Nunca fabrique resultados, evidências ou funcionalidades.
