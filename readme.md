# Tech Challenge — Fase 1: Análise de NPS em E-commerce

Projeto de análise de dados sobre satisfação e experiência de compra em um e-commerce. O trabalho investiga fatores associados à nota de recomendação dos clientes e identifica possíveis pontos de ruptura na entrega e no atendimento pós-compra.

## Objetivo de negócio

Compreender quais atritos da jornada estão associados à insatisfação e apoiar decisões de Operações, Logística, Atendimento, Produto e Marketing. A proposta é transformar a avaliação pós-compra em insights para priorizar melhorias na experiência do cliente.

## Perguntas analisadas

- Como os clientes se distribuem entre detratores, neutros e promotores?
- Quais indicadores apresentam maior associação com a nota de NPS?
- Em quais faixas de atraso, reclamações, contatos e tempo de resolução a proporção de detratores aumenta?
- Como o acúmulo de atritos se relaciona com a insatisfação?
- Há diferenças nos atrasos entre regiões ou relações com tentativas de entrega e frete?
- Como modelos de regressão estimam a nota de NPS a partir de indicadores operacionais?

## Base de dados

Os três notebooks utilizam o arquivo `desafio_nps_fase_1.csv`, lido por meio de um caminho relativo. Disponibilize esse CSV na mesma pasta dos notebooks antes de executar o projeto. A base não acompanha os três notebooks enviados nesta versão.

Principais variáveis utilizadas:

| Variável | Descrição |
| --- | --- |
| `nps_score` | Nota de recomendação do cliente, de 0 a 10 |
| `delivery_delay_days` | Dias de atraso na entrega |
| `complaints_count` | Quantidade de reclamações |
| `customer_service_contacts` | Quantidade de contatos com atendimento |
| `resolution_time_days` | Tempo de resolução em dias |
| `csat_internal_score` | Indicador interno de satisfação |
| `repeat_purchase_30d` | Indicador de recompra em 30 dias |
| `customer_region` | Região do cliente |
| `delivery_attempts` | Quantidade de tentativas de entrega |
| `freight_value` | Valor do frete |

### Classificação de NPS

- **Detratores:** notas de 0 a 6.
- **Neutros:** notas 7 e 8.
- **Promotores:** notas 9 e 10.

O NPS agregado corresponde ao percentual de promotores menos o percentual de detratores, com resultado entre -100 e 100. A nota individual `nps_score` e o NPS agregado são medidas distintas.

## Organização dos arquivos

| Arquivo | Conteúdo |
| --- | --- |
| `projeto_final_fase1.ipynb` | Notebook identificado como versão final; contém inspeção da base, classificação de NPS, comparações entre grupos, correlações e visualizações de atraso e região |
| `tech_challenge_01.ipynb` | Desenvolvimento da EDA com interpretação de negócio, pontos de ruptura, proporção de detratores por indicador, acúmulo de atritos e investigação logística |
| `Rascunho Tech Challenge.ipynb` | Contextualização de negócio, análises complementares e experimentos com regressão linear simples, regressão linear múltipla e Random Forest |
| `desafio_nps_fase_1.csv` | Base de dados necessária para execução |
| `requirements.txt` | Pacotes e versões do ambiente Python |
| `.gitignore` | Regras para excluir arquivos locais do versionamento |

O notebook de desenvolvimento foi enviado para revisão com o nome `tech_challenge_01(1).ipynb`; a tabela utiliza o nome `tech_challenge_01.ipynb` exibido na pasta do projeto. Se mantiver o sufixo `(1)`, ajuste essa referência.

## Etapas do trabalho

1. Contextualização do problema e escolha da variável de satisfação.
2. Inspeção das dimensões, tipos, valores ausentes e estatísticas descritivas.
3. Classificação dos clientes por faixa de NPS.
4. Comparação de médias, distribuições e correlações entre indicadores.
5. Investigação de pontos de ruptura e do efeito acumulado dos atritos.
6. Análise de possíveis fatores logísticos associados aos atrasos.
7. Experimentos de modelagem no notebook de rascunho.

### Modelagem exploratória

O rascunho utiliza `nps_score` como variável-alvo:

- **Regressão linear simples:** utiliza os dias de atraso na entrega.
- **Regressão linear múltipla:** inclui atraso, reclamações, tempo de resolução e contatos com atendimento.
- **Random Forest Regressor:** utiliza os mesmos quatro indicadores para explorar relações não lineares.

Os experimentos separam 80% dos registros para treino e 20% para teste, com `random_state=42`, e avaliam MSE, MAE e R². Essas etapas estão no rascunho e não estão incorporadas ao notebook identificado como final.

## Tecnologias

- Python e notebooks Jupyter.
- Pandas e NumPy para manipulação dos dados.
- Matplotlib, Seaborn e Plotly para visualizações.
- Scikit-learn para codificação de categorias e modelos de regressão.
- VS Code para edição e execução dos notebooks.

## Como executar no Windows / PowerShell

Abra o terminal na pasta do projeto.

### 1. Criar e ativar o ambiente virtual

O exemplo abaixo utiliza Python 3.12; use a versão compatível com o ambiente em que o projeto foi desenvolvido.

```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
```

### 2. Instalar as dependências

```powershell
python -m pip install -r requirements.txt
```

Para selecionar o ambiente como kernel no VS Code, instale `ipykernel` caso ele não esteja no arquivo de dependências:

```powershell
python -m pip install ipykernel
```

### 3. Preparar os dados e abrir o notebook

1. Coloque `desafio_nps_fase_1.csv` na pasta dos notebooks.
2. Abra o notebook desejado no VS Code, com as extensões Python e Jupyter instaladas.
3. Em **Select Kernel / Selecionar Kernel**, escolha o Python da `.venv`.
4. Execute as células na ordem em que aparecem.

### Estado atual de execução

O notebook `projeto_final_fase1.ipynb` ainda contém pontos que precisam ser ajustados para execução completa a partir de um kernel limpo:

- `ordem` é utilizada em boxplots antes de ser definida; mova sua definição para antes do primeiro uso.
- `nota_inteira` é utilizada em agrupamentos, mas sua criação não aparece nas células fornecidas; defina a coluna conforme a regra de análise ou utilize `nps_score` se essa for a intenção.
- A expressão `"nps_score".round()` tenta arredondar um texto; substitua o rótulo por uma string, como `plt.ylabel("Frequência")` no histograma.

O notebook também contém uma célula `!pip install xlrd`, embora a leitura apresentada utilize CSV. A instalação de dependências deve preferencialmente ser feita no ambiente antes da execução.

## Atualizar as dependências

Com a `.venv` do projeto ativa:

```powershell
python -m pip freeze > requirements.txt
```

Esse comando substitui o arquivo pela lista de pacotes e versões instalados no ambiente. Ele não identifica automaticamente quais pacotes são utilizados nos notebooks.

## Interpretação dos resultados

As análises exploratórias investigam associações entre indicadores operacionais e satisfação. Correlações e diferenças entre grupos não demonstram causalidade. Os pontos de ruptura identificados são hipóteses para investigação e priorização de melhorias.

Antes de utilizar os modelos em uma intervenção durante a jornada, verifique quais informações estarão disponíveis no momento da previsão. Indicadores de pós-compra podem ainda não existir nessa etapa.

## Versionamento

Mantenha código, notebooks, documentação e dependências no Git. Um `.gitignore` básico para este projeto é:

```gitignore
.venv/
__pycache__/
.ipynb_checkpoints/
.env
```

Para registrar a atualização da documentação e das dependências:

```powershell
git add README.md requirements.txt
git commit -m "Atualiza documentação e dependências do projeto"
git push
```

Se o arquivo do repositório continuar chamado `readme.md`, use esse nome no comando `git add`.
