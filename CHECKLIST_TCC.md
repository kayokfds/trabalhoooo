# Checklist viva — TCC

**Tema:** Fatores macroeconômicos para previsão de retornos e otimização de carteiras com ativos da B3  
**Última atualização:** 08/08/2026

Legenda: `[ ]` pendente · `[-]` em andamento · `[x]` concluído

## Próxima prioridade

- [x] Criar e conectar o repositório privado do TCC no GitHub.
- [ ] Escrever o protocolo de dados e de execução da estratégia (uma página).
- [x] Baixar e testar a extração das cotações históricas da B3 para 2016–2025.
- [ ] Definir a fonte de preços ajustados e o tratamento de eventos corporativos.
- [x] Definir a fonte dos vértices da curva: B3, "Mercado de Derivativos - Taxas de Mercado para Swaps / Derivatives Market - Swap Market Rates".
- [x] Baixar os vértices diários desejados da B3 para construção do *term spread*.
- [x] Definir o tratamento dos vértices ausentes: quando o prazo desejado não possui observação, realizar interpolação linear entre os dois vértices observados que o cercam, atribuindo pesos inversamente proporcionais à distância até o prazo desejado.
- [x] Tratar a ausência do arquivo de 25/05/2018: preencher os vértices dessa data pela média simples dos dois dias úteis mais próximos.
- [ ] Definir precisamente quais dois vértices/maturidades formarão o *term spread* e como o spread será calculado.
- [ ] Buscar e ler artigos que fundamentem a escolha de cada variável macro (Selic, term spread, câmbio). As fontes e séries de dados já foram definidas; permanece pendente apenas a fundamentação na literatura.

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
- [x] Obter série diária da Selic.
- [x] Obter série diária de câmbio (PTAX/BRL-USD) para 2016–2025; a variação será construída posteriormente como feature.
- [x] Obter os vértices necessários para o *term spread* a partir da publicação oficial da B3.
- [x] Interpolar linearmente os vértices desejados quando o prazo exato não estiver disponível no arquivo diário, utilizando os dois vértices observados que delimitam o prazo.
- [x] Tratar 25/05/2018, única data sem o arquivo correspondente, pela média simples dos dois dias úteis adjacentes.
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

- **30/08/2026:** definida e validada a fonte dos vértices da curva de juros. Os dados foram obtidos da publicação oficial da B3 "Mercado de Derivativos - Taxas de Mercado para Swaps / Derivatives Market - Swap Market Rates", disponível na página de Pesquisa por Pregão.
- **30/08/2026:** os vértices desejados foram extraídos para todo o período. Quando o prazo exato necessário não estava disponível em determinado arquivo, a taxa foi obtida por interpolação linear entre os dois vértices disponíveis que cercavam o prazo desejado. Por exemplo, para obter a taxa de 252 DU quando estavam disponíveis apenas 250 e 260 DU, foram atribuídos pesos 8/10 à taxa de 250 DU e 2/10 à taxa de 260 DU.
- **30/08/2026:** foi identificada uma única data sem o arquivo correspondente, 25/05/2018. Para essa data, os valores dos vértices foram preenchidos pela média simples dos dois dias úteis mais próximos. Trata-se de uma única observação em todo o período, com impacto esperado desprezível sobre os resultados.
- **30/08/2026:** os valores dos vértices foram validados numericamente e apresentaram comportamento consistente com a estrutura observada da curva. A escolha final dos dois vértices que formarão o *term spread* permanece pendente.
- **30/08/2026:** validada a fonte de câmbio. Foi utilizada a série SGS 1 do Banco Central do Brasil, "Dólar americano (venda) - diário", obtida via API BCData/SGS para 01/01/2016–31/12/2025.
- **30/08/2026:** concluída a obtenção da série diária da Selic para o período do estudo. A série foi incorporada à base de variáveis macroeconômicas e sua transformação/defasagem será definida posteriormente na construção das features.
- **22/08/2026:** leitura dos arquivos COTAHIST (2016–2025) resolvida via `pd.read_fwf` com colspecs corretos; ajustado para leitura otimizada (fatiamento de string em Python puro antes de montar o DataFrame, para reduzir tempo de execução). Estratégia de concatenação corrigida para acumular em lista e concatenar uma única vez, evitando estouro de RAM.
- **22/08/2026:** identificado que o campo `especi` (não `indopc`) é o que sinaliza eventos de proventos (ex-dividendo, ex-juros, ex-bonificação, ex-subscrição, ex-grupamento) — tratamento ainda pendente de implementação.
- **22/08/2026:** identificado risco metodológico: Selic, term spread e câmbio são idênticos para todos os ativos em uma mesma data, portanto não diferenciam empresas entre si. Será necessário incluir features específicas de cada ativo (ex.: retornos defasados, volatilidade histórica) para que o modelo consiga diferenciar ativos — decisão de quais features usar ainda em aberto.
- **22/08/2026:** tentativa de obter o *term spread* via API da ANBIMA não teve êxito (endpoint sugerido por terceiros retornou erro 404; não foi confirmada a existência de API pública funcional para série histórica de ETTJ). Fonte do term spread permanece **não definida**.
- **22/08/2026:** nenhuma variável macro (Selic, term spread, câmbio) tem justificativa de literatura acadêmica levantada e confirmada até o momento. As explicações discutidas em chat são apenas hipóteses de trabalho do próprio Claude para fins de organização de ideias — não substituem revisão de literatura e não devem ser citadas como referência no texto do TCC.
- **08/08/2026:** projeto inicial revisado; escopo confirmado com dados diários, ações elegíveis da B3, Random Forest, otimização de carteira e variáveis Selic, curva de juros e câmbio.
- **08/08/2026:** repositório local inicializado e vinculado ao repositório privado `kayokfds/tcc-epge`; a primeira publicação permanece pendente de configuração de autenticação e da revisão dos arquivos a enviar.
- **08/08/2026:** repositório privado `kayokfds/tcc-epge` criado no GitHub e conexão da conta GitHub ao Codex confirmada visualmente pelo usuário.
- **08/08/2026:** criados `README.md` e `.gitignore` para documentar o projeto e evitar o versionamento de dados brutos, credenciais e arquivos temporários.
- **08/08/2026:** estrutura inicial publicada no GitHub, no repositório privado `kayokfds/tcc-epge` (branch `main`).
- **08/08/2026:** definido o uso de Google Colab para execução dos notebooks e Google Drive para armazenamento dos dados e resultados.

## Registro de decisões

Registre aqui toda decisão confirmada que detalhe ou altere o escopo. Cada entrada deve informar a data, a decisão, sua justificativa, o impacto no trabalho e as tarefas afetadas.

- **30/08/2026 — Fonte dos vértices da curva.** Decisão: utilizar os vértices diários divulgados pela B3 no arquivo "Mercado de Derivativos - Taxas de Mercado para Swaps" para construir a variável de curva de juros. Justificativa: trata-se de fonte oficial da B3 e de uma publicação específica de taxas de mercado para swaps. Impacto: os vértices da B3 passam a ser a fonte primária da curva utilizada no estudo.
- **30/08/2026 — Interpolação dos vértices.** Decisão: quando o prazo desejado não estiver diretamente disponível no arquivo diário, estimar sua taxa por interpolação linear entre os dois vértices observados que o cercam. Os pesos são determinados pela distância do prazo desejado em relação aos dois vértices. Justificativa: permite obter taxas para os prazos padronizados escolhidos sem extrapolar a curva. Impacto: os vértices utilizados no cálculo do *term spread* podem ser obtidos de forma consistente mesmo quando seus prazos exatos não são divulgados no arquivo.
- **30/08/2026 — Ausência do arquivo de 25/05/2018.** Decisão: para a única data sem o arquivo correspondente, utilizar a média simples dos valores dos dois dias úteis adjacentes para cada vértice. Justificativa: trata-se de uma única data dentro do período de 2016–2025, de modo que o impacto sobre os resultados é esperado como desprezível. Impacto: permite manter a série diária completa sem alterar significativamente sua composição.
- **30/08/2026 — Fonte do câmbio.** Decisão: utilizar a série SGS 1 do Banco Central do Brasil, dólar americano (venda), frequência diária, obtida via API BCData/SGS. Justificativa: fonte oficial do Banco Central e série diária disponível durante todo o período de estudo. Impacto: estabelece a fonte primária da variável cambial; sua transformação em variação será realizada posteriormente na etapa de construção das features.
- **08/08/2026 — Frequência diária.** Decisão: utilizar dados e previsões diárias, em vez de mensais. Justificativa: o objetivo é otimização de carteiras em alta frequência de observação e o usuário definiu que essa é a abordagem adequada ao trabalho. Impacto: a coleta, a validação temporal, o rebalanceamento e as variáveis macro precisam respeitar um calendário diário e a disponibilidade da informação em cada pregão. Tarefas afetadas: protocolo de dados, construção do painel, definição do retorno-alvo e estratégia de execução.
- **08/08/2026 — Universo amplo de ações da B3.** Decisão: trabalhar com todas as ações elegíveis da B3, em vez de uma seleção reduzida de ativos. Justificativa: preservar o escopo e a proposta original do estudo. Impacto: são necessárias regras explícitas de elegibilidade, liquidez, entrada e saída de ativos, além de atenção a viés de sobrevivência. Tarefas afetadas: coleta das cotações, limpeza da base e definição do universo em cada data.
- **08/08/2026 — Manter otimização de carteira.** Decisão: preservar a otimização média-variância como etapa do trabalho, sem substituí-la por uma regra simplificada de seleção de ações. Justificativa: a otimização é elemento central da pergunta de pesquisa e da contribuição pretendida. Impacto: será necessário definir restrições, matriz de covariância, frequência de rebalanceamento e benchmarks adequados. Tarefas afetadas: especificação metodológica e implementação das carteiras.
- **08/08/2026 — Random Forest como modelo inicial; macroeconomia ampliada.** Decisão: iniciar com Random Forest e considerar Selic, inclinação da curva de juros e câmbio BRL/USD. Justificativa: manter um modelo de ML interpretável e compatível com o escopo, incorporando a recomendação de incluir câmbio no contexto brasileiro. Impacto: o teste central compara o mesmo método com e sem os fatores macroeconômicos. Tarefas afetadas: levantamento das séries, tratamento de defasagens e estimações.
- **08/08/2026 — Ambiente computacional e armazenamento.** Decisão: executar os notebooks no Google Colab, armazenar dados e resultados no Google Drive e versionar códigos, notebooks, documentação e checklist no GitHub. Justificativa: eliminar a necessidade de configurar ambiente local e manter os dados fora do repositório, preservando reprodutibilidade pelo código. Impacto: notebooks devem montar o Drive, trabalhar localmente na sessão quando possível e salvar versões no GitHub; dados brutos não devem ser enviados ao repositório. Tarefas afetadas: organização das pastas de dados, criação dos notebooks e rotina de versionamento.
