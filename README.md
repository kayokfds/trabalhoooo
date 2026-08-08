# TCC EPGE — Fatores Macroeconômicos e Otimização de Carteiras

Repositório do Trabalho de Conclusão de Curso de Kayo Francisco da Silva, na FGV EPGE.

## Pergunta de pesquisa

A incorporação de taxa de juros, inclinação da curva de juros e câmbio em um modelo Random Forest melhora o desempenho fora da amostra de carteiras média-variância formadas diariamente com ações da B3?

## Escopo atual

- Período planejado: 2015–2025;
- Dados e previsões: diários;
- Universo: todas as ações elegíveis da B3;
- Modelo inicial: Random Forest;
- Variáveis macroeconômicas: Selic, inclinação da curva de juros e câmbio BRL/USD;
- Construção das carteiras: otimização média-variância, sujeita a restrições a definir.

## Organização

- `CHECKLIST_TCC.md`: acompanhamento, tarefas e registro de decisões;
- `AGENTS.md`: instruções de continuidade do projeto;
- `data/`: dados locais — não versionados, exceto arquivos pequenos de exemplo;
- `src/`: código de coleta, tratamento, modelagem e otimização;
- `notebooks/`: análises exploratórias;
- `outputs/`: tabelas, gráficos e resultados reproduzíveis;
- `docs/`: documentos de apoio e texto da monografia.

## Reprodutibilidade e dados

Dados brutos, credenciais e arquivos temporários não devem ser enviados ao GitHub. O repositório deve conter código e documentação suficientes para reproduzir o processamento a partir das fontes originais.
