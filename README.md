# VIGIA · Redes Neurais

FIAP · Global Solution 2026.2 · 1TIAPZ

- Caio Ryan Velten Alves · RM 572913
- João Vitor Fernandes Gomes · RM 573882
- Igor Cypriano Moreno da Silva · RM 570353

Avaliamos uma rede neural para identificar janelas com pelo menos 10 mm de chuva acumulada nas três horas seguintes. O VIGIA foi concebido para moradores que passam a pé por uma travessia sujeita a alagamento. A estimativa meteorológica regional complementa a leitura local do nível da água. Usamos a mesma [reanálise ERA5](https://open-meteo.com/en/docs/historical-weather-api) e a mesma divisão temporal de Machine Learning para comparar os resultados.

| Material | Conteúdo |
|---|---|
| [Vídeo de demonstração](https://drive.google.com/file/d/1wOi7uYgDDL4ATO8dc5K9xpFv59M8KZYa/view) | Apresentação de 3 min 28 s com arquitetura, treinamento, avaliação e inferência. |
| [Notebook executado](VIGIA-Redes-Neurais.ipynb) | Dados, arquitetura, três configurações, curvas, avaliação e demonstração. |
| [Série horária](dados/era5-sao-paulo-2018-2024.json) | Reanálise ERA5 para uma célula de São Paulo, distribuída pela Open-Meteo. |
| [Referência de Machine Learning](dados/referencia-ml.csv) | Métricas dos modelos clássicos avaliados no mesmo período de teste. |
| [Resultados](resultados-rn.json) | Comparação na validação e métricas de 2024. |
| [Demonstração](reproducao-rn.json) | Entradas, escores, cortes e chuva posterior para três casos de teste. |

## Reprodução

Com Python 3.13, execute na pasta do trabalho:

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m jupyter lab
```

O notebook inclui as saídas da execução. Para reproduzi-las, abra o arquivo no JupyterLab e execute todas as células.

## Resultados

Selecionamos a configuração C pela maior precisão média (AP) na validação. Ela usa duas camadas ocultas, regularização L2 e parada antecipada. No teste de 2024, a rede obteve AP de 0,159, identificou 15 das 54 janelas positivas e gerou 72 falsos avisos. A regressão logística de Machine Learning obteve AP de 0,172 no mesmo teste. Os escores da rede são usados com um corte escolhido na validação. A calibração como probabilidade ficou fora da avaliação.

Na seção **Inferência em uma janela inédita**, executamos a rede em uma leitura de 23/01/2024 às 15h UTC. Ela sinalizou uma janela positiva, e a reanálise registrou 10,7 mm acumulados nas três horas seguintes.
