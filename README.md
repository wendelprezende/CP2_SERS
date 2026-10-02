# APIs de Energia Renovável e Machine Learning

## Objetivo

Consultar APIs públicas de energia e aplicar aprendizado de máquina em
duas tarefas: classificar a fonte de geração de empreendimentos da ANEEL
e estimar a radiação solar horária em Petrolina (PE).

## Fontes e período dos dados

-   **ANEEL --- SIGA:** dados públicos de empreendimentos de geração.
    Foram consultadas as categorias UFV, EOL, UHE, PCH e CGH, agrupadas
    em Solar, Eólica e Hidráulica. A consulta não aplica filtro
    temporal; representa os registros disponíveis no SIGA no momento da
    execução.
-   **Open-Meteo --- Historical Weather API:** dados horários estimados
    por modelos/reanálise para Petrolina (PE), de **01/04/2025 a
    30/06/2025**, no horário local `America/Recife`.

## Como executar

1.  Instale Python 3 e as dependências:
    `pip install pandas numpy matplotlib scikit-learn`
2.  Abra `Aula_APIs_Energia_Renovavel_ML.ipynb` no Jupyter ou Google
    Colab.
3.  Execute as células na ordem, a partir de um ambiente limpo.
4.  O notebook consulta as APIs e gera os arquivos
    `aneel_classificacao_orange.csv` e `meteo_regressao_orange.csv`.

As APIs utilizadas são públicas e não exigem token para estas consultas.

## Resumo dos resultados

### Classificação

Foram utilizados 3.876 empreendimentos válidos. Com divisão
estratificada de 80% para treino e 20% para teste:

  Modelo                  Accuracy   Precision macro   Recall macro   F1 macro
  --------------------- ---------- ----------------- -------------- ----------
  Regressão Logística       0,8235            0,8275         0,8200     0,8182
  KNN                       0,9665            0,9677         0,9649     0,9662
  Random Forest             0,9755            0,9769         0,9741     0,9753

O Random Forest apresentou o maior F1 macro. As principais confusões
foram Solar → Hidráulica (7 casos), Solar → Eólica (5) e Eólica →
Hidráulica (4).

### Regressão

Foram analisados 1.001 registros horários, com 800 para treino e 201
para teste, mantendo a ordem temporal.

  Modelo                MAE (W/m²)   MSE ((W/m²)²)       R²
  ------------------- ------------ --------------- --------
  Random Forest              66,12        7.214,69   0,8462
  Árvore de Decisão          87,63       15.146,98   0,6772
  Regressão Linear          145,20       30.034,20   0,3598

O Random Forest foi selecionado pelo menor MAE no conjunto de teste. Na
Random Forest, a variável `hora` teve a maior importância entre as
entradas. A estimativa de radiação não representa diretamente a energia
elétrica produzida por um sistema fotovoltaico, pois a geração também
depende de características e perdas do sistema.

## Referências

-   ANEEL --- SIGA:
    https://dadosabertos.aneel.gov.br/dataset/siga-sistema-de-informacoes-de-geracao-da-aneel
-   ANEEL --- recurso utilizado:
    https://dadosabertos.aneel.gov.br/dataset/siga-sistema-de-informacoes-de-geracao-da-aneel/resource/11ec447d-698d-4ab8-977f-b424d5deee6a
-   Open-Meteo --- histórico:
    https://open-meteo.com/en/docs/historical-weather-api
