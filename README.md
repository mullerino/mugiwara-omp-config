# Mugiwara OMP Config

Snapshot da configuração global da tripulação Mugiwara para OMP 18.12.3.

## Conteúdo

- `config.yml`: modelo padrão, roteamento dos agentes, limites de delegação, isolamento e aprovação.
- `RULES.md`: instruções globais carregadas pelo OMP.
- `Orchestrator.md`: cópia legível do papel e do fluxo do Luffy.
- `agents/`: definições dos agentes da tripulação e os arquivos nativos preservados na configuração de origem.

## Modelos e limites definidos

- Agente principal: `openai-codex/gpt-6.1-sol:medium`.
- Jinbe, Franky, Nami, Robin e Zoro: `openai-codex/gpt-6-luna:max`.
- Concorrência máxima: 2 agentes; profundidade de delegação: 1.

## Instalação no Windows

1. Instale a mesma versão do OMP e autentique o provedor `openai-codex` nessa máquina. A autenticação não faz parte deste repositório.
2. Feche o OMP.
3. Faça backup do conteúdo existente em `%USERPROFILE%\.omp\agent`.
4. Copie `config.yml`, `RULES.md`, `Orchestrator.md` e a pasta `agents` para `%USERPROFILE%\.omp\agent`.
5. Abra o OMP e confirme os modelos e agentes selecionados.

Para outros sistemas, o diretório padrão é `~/.omp/agent`.

## Dados excluídos

Este repositório não contém banco de autenticação, credenciais, histórico, sessões, cache, `models.db`, `agent.db`, `mcp.json` ou configurações de projetos. Faça login separadamente em cada máquina. Configurações locais do projeto e opções de inicialização podem sobrepor valores globais.

`config.yml` preserva as opções da máquina de origem, inclusive `dev.autoqaConsent: granted` e `tools.approvalMode: write`; revise-as antes de instalar em outra máquina.
