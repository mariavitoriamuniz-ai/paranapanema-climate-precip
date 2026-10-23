# Mudança na Precipitação da Bacia do Paranapanema sob o Cenário RCP8.5

[English](README.md) | **Português**

Trabalho da disciplina *Mudanças Climáticas* — Engenharia de Recursos Hídricos e do Meio Ambiente, Universidade Federal Fluminense (UFF).

## Resumo

Este trabalho analisa a precipitação mensal simulada na UGRH Paranapanema (São Paulo/Paraná, ~100.800 km²) em três períodos de um ensemble de 40 membros de um modelo climático global: um período histórico inicial (HIST, 1850–1919), o século XX (20C, 1920–2005) e um cenário futuro de altas emissões (RCP8.5, 2006–2100). As anomalias mensais em relação ao ciclo sazonal do HIST foram analisadas quanto a tendência de longo prazo, estacionariedade (testes ADF e KPSS), variabilidade, memória temporal e distribuição. No membro selecionado, HIST e 20C se comportam como séries estacionárias e sem tendência linear, enquanto no RCP8.5 aumentam a média, a variância e a cauda superior da precipitação mensal, e o teste KPSS rejeita a estacionariedade da média.

## Dados

Os três conjuntos são **saídas de modelo climático** (nenhuma observação é usada), fornecidos na disciplina como precipitação diária média na bacia:

| Período | Anos | Conteúdo |
|---|---|---|
| HIST | 1850–1919 | Simulação do período histórico inicial (climatologia de referência) |
| 20C | 1920–2005 | Simulação do século XX, 40 membros |
| RCP8.5 | 2006–2100 | Projeção de altas emissões, mesmos 40 membros |

Os arquivos de dados não são redistribuídos aqui; ver [`data/README.md`](data/README.md) (em inglês).

## Métodos

1. **Pré-processamento:** remoção do dia 29 de fevereiro (padronização do calendário) e agregação dos valores diários em totais mensais.
2. **Seleção do membro:** a climatologia de 12 meses de cada um dos 40 membros (20C) foi correlacionada com a climatologia do HIST; o membro de maior correlação (`member_103`, r = 0,9993) foi usado no 20C e no RCP8.5. Usou-se um único membro, e não a média do ensemble, para preservar uma variabilidade realista.
3. **Anomalias:** precipitação mensal menos a climatologia mensal do HIST.
4. **Tendência e sinal de baixa frequência:** tendência linear por mínimos quadrados das anomalias mensais e médias móveis de 13 meses e de 10 anos.
5. **Estacionariedade:** testes Augmented Dickey-Fuller (H0: raiz unitária) e KPSS (H0: estacionariedade em nível), α = 0,05.
6. **Variabilidade e memória:** variância móvel de 10 anos (120 meses), ACF e PACF até 36 defasagens.
7. **Distribuição:** boxplots e estimativas de densidade kernel das anomalias.

Implementado em Python (pandas, NumPy, statsmodels, matplotlib, seaborn).

## Resultados

**Ciclo sazonal (HIST):** verão chuvoso e inverno seco, de 215 mm em janeiro a 17 mm em julho.

| Estatística (precipitação mensal) | HIST | 20C | RCP8.5 |
|---|---:|---:|---:|
| Média (mm) | 97,9 | 94,2 | 102,8 |
| Desvio-padrão (mm) | 76,3 | 76,0 | 84,5 |
| Máximo (mm) | 324,9 | 307,3 | 378,8 |
| Tendência linear das anomalias (mm/mês por mês) | −0,001 | −0,002 | +0,021 |

| Período | ADF (raiz unitária) | KPSS (estacionariedade em nível) | Interpretação |
|---|---|---|---|
| HIST | rejeitada (p < 0,01) | não rejeitada (p ≥ 0,10) | estacionária |
| 20C | rejeitada (p < 0,01) | não rejeitada (p ≥ 0,10) | estacionária |
| RCP8.5 | rejeitada (p < 0,01) | rejeitada (p ≤ 0,01) | sem raiz unitária, mas com média não constante (tendência) |

Principais resultados:

- **HIST e 20C** não apresentam tendência linear relevante; a média do 20C fica um pouco abaixo da referência HIST.
- **RCP8.5** apresenta tendência positiva (≈ +25 mm/mês por século), com anomalias médias decenais chegando a cerca de +25 mm/mês no fim do século.
- **A variabilidade aumenta** no RCP8.5: a variância móvel de 10 anos atinge valores bem acima dos do HIST e do 20C, e a distribuição tem cauda superior mais pesada (mais meses muito chuvosos).

<p align="center">
  <img src="figures/anomaly_trend.png" width="760" alt="Média móvel de 10 anos das anomalias mensais de precipitação para HIST, 20C e RCP8.5">
  <br><em>Média móvel de 10 anos das anomalias mensais de precipitação (em relação à climatologia do HIST).</em>
</p>

<p align="center">
  <img src="figures/distribution.png" width="760" alt="Boxplots e curvas de densidade das anomalias mensais de precipitação para HIST, 20C e RCP8.5">
  <br><em>Distribuição das anomalias mensais: boxplots (esquerda) e curvas de densidade (direita).</em>
</p>

## Limitações

- **Apenas saídas de modelo.** Todos os períodos são simulações, sem observações nem correção de viés; os valores absolutos descrevem o clima do modelo, não o clima observado na bacia.
- **Um único membro do ensemble.** Os resultados vêm de uma realização; parte do sinal pode refletir a variabilidade interna do modelo. Todos os membros têm climatologias sazonais muito parecidas (r ≈ 0,99), então o critério de seleção os distingue pouco.
- **Série média da bacia.** Diferenças espaciais dentro da bacia não são representadas.
- **Significância das tendências.** As tendências lineares são descritivas (inclinação por mínimos quadrados); não foi aplicado teste de significância a elas. Alguns p-valores do KPSS estão nos limites da tabela do teste (0,01 e 0,10).

## Como reproduzir

O notebook roda no **Google Colab** e lê os três arquivos `.parquet` do Google Drive.

1. Coloque `paranapanema-hist.parquet`, `paranapanema-20c.parquet` e `paranapanema-rcp85.parquet` em `MyDrive/Mudanças Climáticas/`.
2. Abra o notebook no Colab e execute todas as células em ordem.

## Autoria

- **Maria Vitória Berrêdo Muniz** — Universidade Federal Fluminense (UFF), Niterói
- Disciplina: *Mudanças Climáticas*, ministrada pelo Prof. André Belém

## Licença

Código sob a [Licença MIT](LICENSE). Os dados climáticos foram fornecidos na disciplina e não estão incluídos.
