# Checklist viva — TCC

**Tema:** Fatores macroeconômicos para previsão de retornos e otimização de carteiras com ativos da B3  
**Última atualização:** 08/08/2026

Legenda: `[ ]` pendente · `[-]` em andamento · `[x]` concluído

## Próxima prioridade

- [x] Criar e conectar o repositório privado do TCC no GitHub.
- [ ] Escrever o protocolo de dados e de execução da estratégia (uma página).
- [ ] Baixar e testar a extração das cotações históricas da B3 para 2016–2025.
- [ ] Definir a fonte de preços ajustados e o tratamento de eventos corporativos.

## Decisões de pesquisa

- [x] Frequência dos dados: diária.
- [x] Universo pretendido: todas as ações elegíveis da B3.
- [x] Modelo inicial: Random Forest.
- [x] Variáveis macro a considerar: Selic, inclinação da curva de juros e câmbio BRL/USD.
- [ ] Definir precisamente a variável de curva de juros que formará o *term spread*.
- [ ] Definir o retorno a prever e o horizonte de previsão.
- [ ] Definir o instante de formação e de implementação da carteira, evitando informação futura.
- [ ] Definir regras de elegibilidade e liquidez das ações.
- [ ] Definir restrições da otimização (long-only, limite por ativo, etc.).
- [ ] Definir se custos de transação entram no exercício principal ou como robustez.

## Dados

- [ ] Mapear fonte, frequência, unidade e disponibilidade de cada série.
- [ ] Baixar cotações diárias históricas da B3 (2016–2025).
- [ ] Obter série diária da Selic.
- [ ] Obter série diária de câmbio (PTAX/BRL-USD) e sua variação.
- [ ] Obter as duas taxas necessárias para o *term spread*.
- [ ] Montar calendário comum de pregões e variáveis macro.
- [ ] Criar painel `data–ativo` com preços, retornos, liquidez e preditores.
- [ ] Verificar dados ausentes, duplicidades, preços inválidos e baixa liquidez.
- [ ] Tratar proventos, desdobramentos e grupamentos de forma documentada.

## Análise e modelos

- [ ] Produzir estatísticas descritivas e gráficos da base final.
- [ ] Implementar previsão-base sem ML.
- [ ] Implementar validação temporal com janelas móveis (*rolling*).
- [ ] Estimar Random Forest sem variáveis macro.
- [ ] Estimar Random Forest com variáveis macro.
- [ ] Estimar matriz de covariância e construir carteira média-variância.
- [ ] Implementar benchmarks.
- [ ] Calcular retorno, volatilidade, Sharpe, drawdown e giro.
- [ ] Avaliar efeito de custos de transação.
- [ ] Fazer testes de robustez.

## Texto e literatura

- [ ] Levantar artigos que apontam ganhos de ML frente a modelos econométricos.
- [ ] Levantar artigos que mostram limites, ausência de ganhos ou risco de *overfitting*.
- [ ] Redigir a justificativa equilibrada para uso de ML.
- [ ] Atualizar metodologia após as decisões e a base estarem fechadas.

## Organização e versionamento

- [x] Criar o repositório remoto privado no GitHub.
- [x] Conectar a pasta local do TCC ao repositório remoto.
- [x] Criar um arquivo `.gitignore` para não enviar dados brutos, arquivos temporários e credenciais.
- [x] Publicar a estrutura inicial, a checklist e as instruções do projeto.
- [x] Definir ambiente de execução: Google Colab com dados no Google Drive.

## Registro de avanços

- **08/08/2026:** projeto inicial revisado; escopo confirmado com dados diários, ações elegíveis da B3, Random Forest, otimização de carteira e variáveis Selic, curva de juros e câmbio.
- **08/08/2026:** repositório local inicializado e vinculado ao repositório privado `kayokfds/tcc-epge`; a primeira publicação permanece pendente de configuração de autenticação e da revisão dos arquivos a enviar.
- **08/08/2026:** repositório privado `kayokfds/tcc-epge` criado no GitHub e conexão da conta GitHub ao Codex confirmada visualmente pelo usuário.
- **08/08/2026:** criados `README.md` e `.gitignore` para documentar o projeto e evitar o versionamento de dados brutos, credenciais e arquivos temporários.
- **08/08/2026:** estrutura inicial publicada no GitHub, no repositório privado `kayokfds/tcc-epge` (branch `main`).
- **08/08/2026:** definido o uso de Google Colab para execução dos notebooks e Google Drive para armazenamento dos dados e resultados.

## Registro de decisões

Registre aqui toda decisão confirmada que detalhe ou altere o escopo. Cada entrada deve informar a data, a decisão, sua justificativa, o impacto no trabalho e as tarefas afetadas.

- **08/08/2026 — Frequência diária.** Decisão: utilizar dados e previsões diárias, em vez de mensais. Justificativa: o objetivo é otimização de carteiras em alta frequência de observação e o usuário definiu que essa é a abordagem adequada ao trabalho. Impacto: a coleta, a validação temporal, o rebalanceamento e as variáveis macro precisam respeitar um calendário diário e a disponibilidade da informação em cada pregão. Tarefas afetadas: protocolo de dados, construção do painel, definição do retorno-alvo e estratégia de execução.
- **08/08/2026 — Universo amplo de ações da B3.** Decisão: trabalhar com todas as ações elegíveis da B3, em vez de uma seleção reduzida de ativos. Justificativa: preservar o escopo e a proposta original do estudo. Impacto: são necessárias regras explícitas de elegibilidade, liquidez, entrada e saída de ativos, além de atenção a viés de sobrevivência. Tarefas afetadas: coleta das cotações, limpeza da base e definição do universo em cada data.
- **08/08/2026 — Manter otimização de carteira.** Decisão: preservar a otimização média-variância como etapa do trabalho, sem substituí-la por uma regra simplificada de seleção de ações. Justificativa: a otimização é elemento central da pergunta de pesquisa e da contribuição pretendida. Impacto: será necessário definir restrições, matriz de covariância, frequência de rebalanceamento e benchmarks adequados. Tarefas afetadas: especificação metodológica e implementação das carteiras.
- **08/08/2026 — Random Forest como modelo inicial; macroeconomia ampliada.** Decisão: iniciar com Random Forest e considerar Selic, inclinação da curva de juros e câmbio BRL/USD. Justificativa: manter um modelo de ML interpretável e compatível com o escopo, incorporando a recomendação de incluir câmbio no contexto brasileiro. Impacto: o teste central compara o mesmo método com e sem os fatores macroeconômicos. Tarefas afetadas: levantamento das séries, tratamento de defasagens e estimações.
- **08/08/2026 — Ambiente computacional e armazenamento.** Decisão: executar os notebooks no Google Colab, armazenar dados e resultados no Google Drive e versionar códigos, notebooks, documentação e checklist no GitHub. Justificativa: eliminar a necessidade de configurar ambiente local e manter os dados fora do repositório, preservando reprodutibilidade pelo código. Impacto: notebooks devem montar o Drive, trabalhar localmente na sessão quando possível e salvar versões no GitHub; dados brutos não devem ser enviados ao repositório. Tarefas afetadas: organização das pastas de dados, criação dos notebooks e rotina de versionamento.
