# Checklist viva — TCC

**Tema:** Fatores macroeconômicos para previsão de retornos e otimização de carteiras com ativos da B3
**Última atualização:** 05/09/2026

Legenda: `[ ]` pendente · `[-]` em andamento · `[x]` concluído

## Próxima prioridade

* [x] Criar e conectar o repositório privado do TCC no GitHub.
* [ ] Escrever o protocolo de dados e de execução da estratégia (uma página).
* [x] Baixar e testar a extração das cotações históricas da B3 para 2016–2025.
* [ ] Definir a fonte de preços ajustados e o tratamento de eventos corporativos.
* [x] Definir a fonte dos vértices da curva: B3, "Mercado de Derivativos - Taxas de Mercado para Swaps / Derivatives Market - Swap Market Rates".
* [x] Baixar os vértices diários desejados da B3 para construção do *term spread*.
* [x] Definir o tratamento dos vértices ausentes: quando o prazo desejado não possui observação, realizar interpolação linear entre os dois vértices observados que o cercam, atribuindo pesos inversamente proporcionais à distância até o prazo desejado.
* [x] Tratar a ausência do arquivo de 25/05/2018: preencher os vértices dessa data pela média simples dos dois dias úteis mais próximos.
* [ ] Definir precisamente quais dois vértices/maturidades formarão o *term spread* e como o spread será calculado.
* [ ] Buscar e ler artigos que fundamentem a escolha de cada variável macro (Selic, *term spread*, câmbio). As fontes e séries de dados já foram definidas; permanece pendente a fundamentação na literatura.
* [ ] **Finalizar a base que será utilizada na etapa de ML:** tratar eventos corporativos, construir retornos, definir variáveis finais, estabelecer regras de elegibilidade/liquidez e realizar as verificações de qualidade e ausência de *look-ahead*.

## Decisões de pesquisa

* [x] Frequência dos dados: diária.
* [x] Universo pretendido: todas as ações elegíveis da B3.
* [x] Modelo inicial: Random Forest.
* [x] Variáveis macro a considerar: Selic, inclinação da curva de juros e câmbio BRL/USD.
* [ ] Definir precisamente a variável de curva de juros que formará o *term spread*.
* [ ] Definir o retorno a prever e o horizonte de previsão.
* [ ] Definir o instante de formação e de implementação da carteira, evitando informação futura.
* [ ] Definir regras de elegibilidade e liquidez das ações, com justificativa na literatura/metodologia e atenção a viés de sobrevivência.
* [ ] Definir restrições da otimização (long-only, limite por ativo, etc.).
* [ ] Definir se custos de transação entram no exercício principal ou como robustez.

## Dados

* [ ] Mapear fonte, frequência, unidade e disponibilidade de cada série.
* [ ] Baixar cotações diárias históricas da B3 (2016–2025).
* [x] Obter série diária da Selic.
* [x] Obter série diária de câmbio (PTAX/BRL-USD) para 2016–2025; a variação será construída posteriormente como *feature*.
* [x] Obter os vértices necessários para o *term spread* a partir da publicação oficial da B3.
* [x] Interpolar linearmente os vértices desejados quando o prazo exato não estiver disponível no arquivo diário, utilizando os dois vértices observados que delimitam o prazo.
* [x] Tratar 25/05/2018, única data sem o arquivo correspondente, pela média simples dos dois dias úteis adjacentes.
* [ ] Montar calendário comum de pregões e variáveis macro.
* [ ] Criar painel `data–ativo` com preços, retornos, liquidez e preditores.
* [ ] Verificar dados ausentes, duplicidades, preços inválidos e baixa liquidez.
* [ ] Tratar proventos, desdobramentos e grupamentos de forma documentada.
* [ ] Definir e documentar as variáveis finais da base de ML.
* [ ] Avaliar individualmente as variáveis `premin`, `premax`, `totneg`, `voltot` e `fatcot`, definindo se serão utilizadas, para qual finalidade e com qual justificativa econômica/metodológica.
* [ ] Utilizar `preult` na construção dos retornos, caso seja confirmado como preço adequado para essa finalidade.
* [ ] Mapear e tratar corretamente o campo `especi` antes da construção definitiva dos retornos, identificando os diferentes eventos corporativos sinalizados.
* [ ] Definir uma série de preços economicamente consistente após o tratamento de eventos corporativos, evitando retornos artificiais causados por proventos ou alterações na estrutura de preços.
* [ ] Definir e documentar transformação/variação do câmbio.
* [ ] Definir defasagens das variáveis macroeconômicas de modo a evitar *look-ahead bias*.
* [ ] Definir as variáveis específicas de cada ativo que serão utilizadas pelo modelo, como retornos defasados e medidas de volatilidade histórica.
* [ ] Construir medidas de volatilidade/variância utilizando exclusivamente informação disponível até cada data de previsão.
* [ ] Definir regra objetiva de elegibilidade/liquidez das ações, preferencialmente apoiada em literatura acadêmica ou referência metodológica adequada.
* [ ] Avaliar o número de observações por ativo, período de negociação, dados ausentes, liquidez e continuidade da série antes de definir o universo final.
* [ ] Avaliar viés de sobrevivência e, se necessário, definir elegibilidade de forma dependente da data em vez de exigir histórico completo de 2016–2025.

## Tratamento da COTAHIST e eventos corporativos

* [x] Resolver a leitura dos arquivos COTAHIST 2016–2025.
* [x] Utilizar `pd.read_fwf` com os *colspecs* adequados.
* [x] Otimizar a leitura dos arquivos realizando o fatiamento antes da criação do DataFrame.
* [x] Corrigir a estratégia de concatenação para acumular os DataFrames em uma lista e realizar um único `pd.concat`, evitando problemas de memória.
* [ ] Explorar os valores únicos de `especi` e suas frequências.
* [ ] Conferir os códigos de `especi` com a documentação oficial da B3.
* [ ] Identificar quais eventos afetam diretamente a série de preços e/ou os retornos.
* [ ] Definir o tratamento de cada tipo de evento corporativo.
* [ ] Implementar o tratamento no código.
* [ ] Validar o tratamento em casos reais da base.
* [ ] Só remover `especi` da base final depois de concluir e validar o tratamento dos eventos.

## Construção da base para ML

A base final só deve ser congelada depois de concluídas as etapas abaixo:

1. Tratamento de eventos corporativos.
2. Construção de uma série de preços consistente.
3. Construção dos retornos.
4. Definição das variáveis finais da COTAHIST.
5. Transformação das variáveis macroeconômicas.
6. Construção das *features* específicas de cada ativo.
7. Definição das regras de elegibilidade e liquidez.
8. Diagnóstico da qualidade da base.
9. Verificação de *look-ahead bias*.
10. Definição final do painel `data–ativo`.
11. Congelamento da base que será utilizada nas estimações.

### Variáveis atualmente sob avaliação

* `preult`: potencialmente utilizada como base para construção dos retornos.
* `premin`: manter sob avaliação; pode ser utilizada posteriormente para construção de alguma medida intradiária, caso haja justificativa.
* `premax`: manter sob avaliação; pode ser utilizada posteriormente para construção de alguma medida intradiária, caso haja justificativa.
* `totneg`: número de negócios; potencialmente útil como medida/filtro de liquidez.
* `voltot`: volume financeiro negociado; potencialmente útil como medida/filtro de liquidez.
* `fatcot`: fator de cotação; avaliar necessidade e interpretação antes de decidir pela manutenção.
* `codbdi`: pode ser utilizado durante a classificação/filtragem dos ativos, mas não necessariamente fará parte das *features* finais.
* `codneg`: identificador do ativo.
* `especi`: utilizado no tratamento dos eventos corporativos e posteriormente possivelmente removido da base final.
* `data_referencia`: permanece como referência temporal da observação.

**Princípio:** nenhuma variável deve entrar na base final apenas porque está disponível. Para cada variável, deve ser possível responder: por que ela está no modelo, como foi construída e se sua informação estaria disponível no momento da previsão.

## Diagnóstico e estatísticas descritivas

* [ ] Calcular número de observações por ativo.
* [ ] Identificar primeiro e último pregão de cada ativo.
* [ ] Calcular percentual de dados ausentes por ativo.
* [ ] Identificar ativos com histórico excessivamente curto.
* [ ] Avaliar continuidade das séries.
* [ ] Avaliar número de pregões e frequência de negociação.
* [ ] Avaliar medidas de liquidez.
* [ ] Identificar retornos extremos e possíveis erros de dados.
* [ ] Verificar duplicidades.
* [ ] Verificar preços nulos, negativos ou inconsistentes.
* [ ] Calcular média, mediana, desvio-padrão, mínimo, máximo e quantis das principais variáveis.
* [ ] Produzir estatísticas descritivas da base final.
* [ ] Produzir gráficos das principais variáveis.
* [ ] Produzir análises descritivas das distribuições dos retornos.
* [ ] Avaliar a distribuição das variáveis macroeconômicas.
* [ ] Avaliar correlações relevantes entre variáveis.
* [ ] Definir critérios objetivos para excluir ativos problemáticos.

## Elegibilidade e liquidez

* [ ] Pesquisar literatura sobre critérios de seleção de ações e filtros de liquidez.
* [ ] Identificar métricas de liquidez adequadas ao mercado brasileiro.
* [ ] Avaliar o uso de `totneg` e `voltot` como critérios de liquidez.
* [ ] Definir se o filtro será baseado em volume financeiro, número de negócios, frequência de negociação ou combinação dessas métricas.
* [ ] Definir janela temporal utilizada para calcular a liquidez.
* [ ] Definir o momento em que o filtro de liquidez é calculado, evitando utilização de informação futura.
* [ ] Definir regra replicável de entrada e saída de ativos do universo.
* [ ] Verificar o impacto do filtro sobre o número de ações disponíveis.
* [ ] Avaliar viés de sobrevivência.
* [ ] Evitar definir o filtro com base no desempenho futuro dos ativos.
* [ ] Documentar a justificativa econômica e bibliográfica do critério escolhido.

## Features

* [ ] Definir retorno defasado de cada ativo.
* [ ] Definir número de defasagens.
* [ ] Definir janela de volatilidade histórica.
* [ ] Calcular desvio-padrão/variância móvel usando apenas dados passados.
* [ ] Avaliar outras medidas específicas de cada ativo que sejam justificáveis.
* [ ] Construir variação do câmbio.
* [ ] Definir transformação da Selic.
* [ ] Definir transformação do *term spread*.
* [ ] Definir defasagens das variáveis macroeconômicas.
* [ ] Garantir que todas as *features* estejam disponíveis antes do período do retorno previsto.
* [ ] Verificar *look-ahead bias* em cada variável.
* [ ] Definir e documentar a matriz final de *features*.

## Análise e modelos

* [ ] Produzir estatísticas descritivas e gráficos da base final.
* [ ] Implementar previsão-base sem ML.
* [ ] Implementar validação temporal com janelas móveis (*rolling*).
* [ ] Estimar Random Forest sem variáveis macro.
* [ ] Estimar Random Forest com variáveis macro.
* [ ] Definir hiperparâmetros e procedimento de seleção.
* [ ] Avaliar importância das variáveis.
* [ ] Comparar desempenho preditivo entre os modelos.
* [ ] Utilizar métricas adequadas de previsão, incluindo RMSE, MAE e, quando fizer sentido, R².
* [ ] Verificar estabilidade do desempenho ao longo do tempo.
* [ ] Evitar *overfitting* e seleção de hiperparâmetros com informação do período de teste.
* [ ] Documentar todo o procedimento de treinamento, validação e teste.

## Portfólio

* [ ] Definir como as previsões de retorno serão convertidas em pesos.
* [ ] Definir frequência de rebalanceamento.
* [ ] Definir janela utilizada para estimação da covariância.
* [ ] Estimar matriz de covariância.
* [ ] Construir carteira média-variância/Markowitz.
* [ ] Definir restrições da otimização.
* [ ] Avaliar carteira long-only.
* [ ] Definir limite máximo por ativo, caso aplicável.
* [ ] Definir tratamento de ativos que deixam de ser elegíveis.
* [ ] Definir benchmarks.
* [ ] Comparar com carteira equiponderada.
* [ ] Avaliar estratégia sem fatores macro.
* [ ] Avaliar estratégia com fatores macro.
* [ ] Definir tratamento de custos de transação.
* [ ] Calcular retorno acumulado.
* [ ] Calcular volatilidade.
* [ ] Calcular Sharpe.
* [ ] Calcular drawdown.
* [ ] Calcular turnover/giro.
* [ ] Avaliar estabilidade da carteira ao longo do tempo.

## Robustez

* [ ] Testar diferentes especificações de *term spread*.
* [ ] Testar diferentes horizontes de previsão, caso seja compatível com o escopo.
* [ ] Testar diferentes janelas de volatilidade.
* [ ] Testar diferentes critérios de liquidez.
* [ ] Avaliar sensibilidade às restrições da otimização.
* [ ] Avaliar impacto dos custos de transação.
* [ ] Avaliar estabilidade dos resultados em diferentes subperíodos.
* [ ] Avaliar outras especificações de Random Forest ou modelos adicionais somente se houver tempo e justificativa.
* [ ] Documentar quais testes são principais e quais são de robustez.

## Literatura

* [ ] Levantar artigos sobre previsão de retornos de ações com Machine Learning.
* [ ] Levantar artigos sobre Random Forest aplicado a previsão de retornos.
* [ ] Levantar artigos que apontam ganhos de ML frente a modelos econométricos.
* [ ] Levantar artigos que mostram limites, ausência de ganhos ou risco de *overfitting*.
* [ ] Levantar literatura sobre fatores macroeconômicos e retornos acionários.
* [ ] Buscar literatura que justifique a utilização da Selic como fator.
* [ ] Buscar literatura que justifique a utilização do *term spread* como fator.
* [ ] Buscar literatura que justifique a utilização do câmbio BRL/USD como fator.
* [ ] Buscar literatura/metodologias para justificar critérios de elegibilidade e liquidez.
* [ ] Buscar literatura sobre liquidez e retorno no mercado acionário brasileiro.
* [ ] Buscar literatura sobre viés de sobrevivência em bases de ações.
* [ ] Redigir justificativa equilibrada para utilização de Machine Learning.
* [ ] Atualizar metodologia após as decisões e a base estarem fechadas.

## Organização e versionamento

* [x] Criar o repositório remoto privado do TCC no GitHub.
* [x] Conectar a pasta local do TCC ao repositório remoto.
* [x] Criar um arquivo `.gitignore` para não enviar dados brutos, arquivos temporários e credenciais.
* [x] Publicar a estrutura inicial, a checklist e as instruções do projeto.
* [x] Definir ambiente de execução: Google Colab com dados no Google Drive.
* [ ] Manter o checklist atualizado conforme decisões metodológicas forem tomadas.
* [ ] Registrar decisões metodológicas importantes no histórico do projeto.
* [ ] Versionar notebooks e códigos após cada etapa relevante.

## Possíveis desenvolvimentos futuros

Estes itens **não fazem parte do núcleo mínimo do TCC neste momento** e só devem ser desenvolvidos caso haja tempo, sem comprometer a sequência principal do trabalho.

* Avaliar o uso de `premin` e `premax` para construção de medidas intradiárias, caso seja encontrada justificativa econômica/metodológica adequada.
* Avaliar outras variáveis disponíveis na COTAHIST que possam gerar *features* úteis.
* Explorar medidas adicionais de liquidez/microestrutura a partir de `totneg`, `voltot` e outras informações disponíveis.
* Avaliar modelos de Machine Learning adicionais além do Random Forest.
* Incorporar custos de transação de forma mais detalhada.
* Ampliar os testes de robustez.
* Avaliar especificações alternativas de fatores macroeconômicos.

Essas extensões devem permanecer subordinadas ao cronograma. O núcleo permanece:

**construção de uma base diária metodologicamente correta → previsão de retornos com Random Forest, com e sem fatores macroeconômicos → formação/otimização de carteiras → comparação de desempenho.**

## Registro de avanços

* **05/09/2026:** definida como prioridade imediata a finalização da base que alimentará o ML. A sequência de trabalho passa a ser: tratamento de eventos corporativos (`especi`) → construção de série de preços/retornos economicamente consistente → definição das variáveis finais → diagnóstico da base e definição objetiva do universo elegível/liquidez → construção das features → congelamento da base final para ML. A estimação e a otimização ficam para depois dessa etapa.
* **05/09/2026:** decidido manter, por enquanto, variáveis como `premin`, `premax`, `totneg`, `voltot` e `fatcot` sob avaliação, sem incluí-las automaticamente na base final. Cada variável deverá ter finalidade econômica/metodológica clara e, quando relevante, justificativa na literatura.
* **05/09/2026:** definido que `preult` será utilizado na construção dos retornos, sujeito à validação do tratamento de eventos corporativos e da definição final da série de preços.
* **05/09/2026:** definido que o filtro de elegibilidade/liquidez deverá ser objetivo e justificável por literatura acadêmica ou referência metodológica adequada, evitando cortes arbitrários e considerando o risco de viés de sobrevivência.
* **05/09/2026:** definidos como próximos passos imediatos o levantamento dos códigos de `especi`, a conferência desses códigos com a documentação da B3 e o tratamento dos eventos corporativos antes da construção definitiva dos retornos.
* **05/09/2026:** possíveis variáveis adicionais de COTAHIST, *features* de liquidez/microestrutura, modelos adicionais e custos de transação mais detalhados foram registrados como desenvolvimentos futuros, caso o cronograma permita.
* **30/08/2026:** definida e validada a fonte dos vértices da curva de juros. Os dados foram obtidos da publicação oficial da B3 "Mercado de Derivativos - Taxas de Mercado para Swaps / Derivatives Market - Swap Market Rates", disponível na página de Pesquisa por Pregão.
* **30/08/2026:** os vértices desejados foram extraídos para todo o período. Quando o prazo exato necessário não estava disponível em determinado arquivo, a taxa foi obtida por interpolação linear entre os dois vértices disponíveis que cercavam o prazo desejado. Por exemplo, para obter a taxa de 252 DU quando estavam disponíveis apenas 250 e 260 DU, foram atribuídos pesos 8/10 à taxa de 250 DU e 2/10 à taxa de 260 DU.
* **30/08/2026:** foi identificada uma única data sem o arquivo correspondente, 25/05/2018. Para essa data, os valores dos vértices foram preenchidos pela média simples dos dois dias úteis mais próximos. Trata-se de uma única observação em todo o período, com impacto esperado desprezível sobre os resultados.
* **30/08/2026:** os valores dos vértices foram validados numericamente e apresentaram comportamento consistente com a estrutura observada da curva. A escolha final dos dois vértices que formarão o *term spread* permanece pendente.
* **30/08/2026:** validada a fonte de câmbio. Foi utilizada a série SGS 1 do Banco Central do Brasil, "Dólar americano (venda) - diário", obtida via API BCData/SGS para 01/01/2016–31/12/2025.
* **30/08/2026:** concluída a obtenção da série diária da Selic para o período do estudo. A série foi incorporada à base de variáveis macroeconômicas e sua transformação/defasagem será definida posteriormente na construção das features.
* **22/08/2026:** leitura dos arquivos COTAHIST (2016–2025) resolvida via `pd.read_fwf` com colspecs corretos; ajustado para leitura otimizada (fatiamento de string em Python puro antes de montar o DataFrame, para reduzir tempo de execução). Estratégia de concatenação corrigida para acumular em lista e concatenar uma única vez, evitando estouro de RAM.
* **22/08/2026:** identificado que o campo `especi` (não `indopc`) é o que sinaliza eventos de proventos (ex-dividendo, ex-juros, ex-bonificação, ex-subscrição, ex-grupamento) — tratamento ainda pendente de implementação.
* **22/08/2026:** identificado risco metodológico: Selic, *term spread* e câmbio são idênticos para todos os ativos em uma mesma data, portanto não diferenciam empresas entre si. Será necessário incluir *features* específicas de cada ativo (ex.: retornos defasados, volatilidade histórica) para que o modelo consiga diferenciar ativos — decisão de quais *features* usar ainda em aberto.
* **22/08/2026:** tentativa de obter o *term spread* via API da ANBIMA não teve êxito (endpoint sugerido por terceiros retornou erro 404; não foi confirmada a existência de API pública funcional para série histórica de ETTJ). Fonte do *term spread* permanece **não definida**.
* **22/08/2026:** nenhuma variável macro (Selic, *term spread*, câmbio) tem justificativa de literatura acadêmica levantada e confirmada até o momento. As explicações discutidas em chat são apenas hipóteses de trabalho do próprio Claude para fins de organização de ideias — não substituem revisão de literatura e não devem ser citadas como referência no texto do TCC.
* **08/08/2026:** projeto inicial revisado; escopo confirmado com dados diários, ações elegíveis da B3, Random Forest, otimização de carteira e variáveis Selic, curva de juros e câmbio.
* **08/08/2026:** repositório local inicializado e vinculado ao repositório privado `kayokfds/tcc-epge`; a primeira publicação permanece pendente de configuração de autenticação e da revisão dos arquivos a enviar.
* **08/08/2026:** repositório privado `kayokfds/tcc-epge` criado no GitHub e conexão da conta GitHub ao Codex confirmada visualmente pelo usuário.
* **08/08/2026:** criados `README.md` e `.gitignore` para documentar o projeto e evitar o versionamento de dados brutos, credenciais e arquivos temporários.
* **08/08/2026:** estrutura inicial publicada no GitHub, no repositório privado `kayokfds/tcc-epge` (branch `main`).
* **08/08/2026:** definido o uso de Google Colab para execução dos notebooks e Google Drive para armazenamento dos dados e resultados.

## Registro de decisões

Registre aqui toda decisão confirmada que detalhe ou altere o escopo. Cada entrada deve informar a data, a decisão, sua justificativa, o impacto no trabalho e as tarefas afetadas.

* **05/09/2026 — Finalização da base antes do ML.** Decisão: priorizar a construção e validação da base final antes de iniciar a estimação do Random Forest ou a otimização das carteiras. A sequência inclui tratamento de eventos corporativos, construção dos retornos, seleção das variáveis, diagnóstico da base, regras de elegibilidade/liquidez, construção das features e verificação de *look-ahead*. Justificativa: a qualidade e a definição econômica da base são condições anteriores à estimação. Impacto: ML e otimização ficam deliberadamente para uma etapa posterior.
* **05/09/2026 — Variáveis COTAHIST sob avaliação.** Decisão: manter `premin`, `premax`, `totneg`, `voltot` e `fatcot` sob avaliação, sem assumir que todas entrarão na estimação. Justificativa: cada variável precisa apresentar utilidade econômica/metodológica e justificativa adequada antes de compor a base final. Impacto: a definição final das features será feita após o tratamento da série de preços e o diagnóstico da base.
* **05/09/2026 — Elegibilidade e liquidez.** Decisão: definir o filtro do universo de ações por critérios objetivos e justificáveis na literatura ou em referência metodológica adequada. Justificativa: evitar escolhas arbitrárias e tornar o universo replicável. Impacto: será necessário pesquisar a literatura e avaliar o viés de sobrevivência antes de congelar o universo.
* **05/09/2026 — Desenvolvimentos futuros.** Decisão: registrar variáveis adicionais de COTAHIST/liquidez, features de microestrutura, modelos adicionais e custos de transação mais detalhados como possíveis extensões, caso haja tempo. Justificativa: preservar o foco do núcleo do TCC sem descartar extensões potencialmente úteis. Impacto: essas extensões não são requisitos para avançar à etapa principal de ML.
* **05/09/2026 — Tratamento de eventos antes dos retornos.** Decisão: tratar `especi` e os eventos corporativos sinalizados antes de construir a série definitiva de preços e retornos. Justificativa: eventos corporativos podem produzir variações de preço que não representam retorno econômico do investidor. Impacto: a construção do retorno não será feita simplesmente a partir da variação bruta de `preult` sem antes definir o tratamento adequado.
* **05/09/2026 — Variáveis específicas dos ativos.** Decisão: incluir, além dos fatores macroeconômicos comuns a todos os ativos, *features* específicas de cada ação, como retornos defasados e medidas de volatilidade histórica. Justificativa: fatores macro idênticos para todos os ativos em uma mesma data não permitem, isoladamente, diferenciar as empresas. Impacto: a etapa de *feature engineering* será necessária antes da estimação do Random Forest.
* **30/08/2026 — Fonte dos vértices da curva.** Decisão: utilizar os vértices diários divulgados pela B3 no arquivo "Mercado de Derivativos - Taxas de Mercado para Swaps" para construir a variável de curva de juros. Justificativa: trata-se de fonte oficial da B3 e de uma publicação específica de taxas de mercado para swaps. Impacto: os vértices da B3 passam a ser a fonte primária da curva utilizada no estudo.
* **30/08/2026 — Interpolação dos vértices.** Decisão: quando o prazo desejado não estiver diretamente disponível no arquivo diário, estimar sua taxa por interpolação linear entre os dois vértices observados que o cercam. Os pesos são determinados pela distância do prazo desejado em relação aos dois vértices. Justificativa: permite obter taxas para os prazos padronizados escolhidos sem extrapolar a curva. Impacto: os vértices utilizados no cálculo do *term spread* podem ser obtidos de forma consistente mesmo quando seus prazos exatos não são divulgados no arquivo.
* **30/08/2026 — Ausência do arquivo de 25/05/2018.** Decisão: para a única data sem o arquivo correspondente, utilizar a média simples dos valores dos dois dias úteis adjacentes para cada vértice. Justificativa: trata-se de uma única data dentro do período de 2016–2025, de modo que o impacto sobre os resultados é esperado como desprezível. Impacto: permite manter a série diária completa sem alterar significativamente sua composição.
* **30/08/2026 — Fonte do câmbio.** Decisão: utilizar a série SGS 1 do Banco Central do Brasil, dólar americano (venda), frequência diária, obtida via API BCData/SGS. Justificativa: fonte oficial do Banco Central e série diária disponível durante todo o período de estudo. Impacto: estabelece a fonte primária da variável cambial; sua transformação em variação será realizada posteriormente na etapa de construção das features.
* **08/08/2026 — Frequência diária.** Decisão: utilizar dados e previsões diárias, em vez de mensais. Justificativa: o objetivo é otimização de carteiras em alta frequência de observação e o usuário definiu que essa é a abordagem adequada ao trabalho. Impacto: a coleta, a validação temporal, o rebalanceamento e as variáveis macro precisam respeitar um calendário diário e a disponibilidade da informação em cada pregão. Tarefas afetadas: protocolo de dados, construção do painel, definição do retorno-alvo e estratégia de execução.
* **08/08/2026 — Universo amplo de ações da B3.** Decisão: trabalhar com todas as ações elegíveis da B3, em vez de uma seleção reduzida de ativos. Justificativa: preservar o escopo e a proposta original do estudo. Impacto: são necessárias regras explícitas de elegibilidade, liquidez, entrada e saída de ativos, além de atenção a viés de sobrevivência. Tarefas afetadas: coleta das cotações, limpeza da base e definição do universo em cada data.
* **08/08/2026 — Manter otimização de carteira.** Decisão: preservar a otimização média-variância como etapa do trabalho, sem substituí-la por uma regra simplificada de seleção de ações. Justificativa: a otimização é elemento central da pergunta de pesquisa e da contribuição pretendida. Impacto: será necessário definir restrições, matriz de covariância, frequência de rebalanceamento e benchmarks adequados. Tarefas afetadas: especificação metodológica e implementação das carteiras.
* **08/08/2026 — Random Forest como modelo inicial; macroeconomia ampliada.** Decisão: iniciar com Random Forest e considerar Selic, inclinação da curva de juros e câmbio BRL/USD. Justificativa: manter um modelo de ML interpretável e compatível com o escopo, incorporando a recomendação de incluir câmbio no contexto brasileiro. Impacto: o teste central compara o mesmo método com e sem os fatores macroeconômicos. Tarefas afetadas: levantamento das séries, tratamento de defasagens e estimações.
* **08/08/2026 — Ambiente computacional e armazenamento.** Decisão: executar os notebooks no Google Colab, armazenar dados e resultados no Google Drive e versionar códigos, notebooks, documentação e checklist no GitHub. Justificativa: eliminar a necessidade de configurar ambiente local e manter os dados fora do repositório, preservando reprodutibilidade pelo código. Impacto: notebooks devem montar o Drive, trabalhar localmente na sessão quando possível e salvar versões no GitHub; dados brutos não devem ser enviados ao repositório. Tarefas afetadas: organização das pastas de dados, criação dos notebooks e rotina de versionamento.
