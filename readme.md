# Tech Challenge — Fase 1: Análise de NPS em E-commerce

Projeto de análise da satisfação de clientes de um e-commerce, com análise exploratória dos dados e segmentação de detratores por K-Means.

## Acesso ao projeto — orientações para avaliação

**Repositório do projeto:** [https://github.com/caylakson/Tech-Challenge-1](https://github.com/caylakson/Tech-Challenge-1)

O endereço acima é disponibilizado neste README para permitir o acesso ao repositório a partir dos arquivos anexados na entrega.

1. Extraia o ZIP entregue e consulte este `readme.md` para conhecer a organização do trabalho e as instruções de execução.
2. Acesse o link do repositório para consultar os arquivos versionados do projeto.
3. Consulte `EDA.ipynb` para a análise exploratória e `modelo.ipynb` para a segmentação dos detratores.
4. Para assistir à apresentação gravada, abra `Apresentacao_Video.ppsx` no Microsoft PowerPoint em modo de apresentação. **O vídeo está gravado nos próprios slides.**

Os notebooks, a base de dados, as dependências e a apresentação estão incluídos no pacote de entrega.

## Objetivo

Investigar quais fatores da jornada de compra estão associados à satisfação e à classificação como detrator, identificando atritos em entrega e atendimento e apoiando propostas de melhoria da experiência do cliente.

## Apresentação com vídeo

**O vídeo de apresentação do trabalho está no arquivo [`Apresentacao_Video.ppsx`](Apresentacao_Video.ppsx), com a gravação nos próprios slides.**

Baixe o arquivo e abra-o no Microsoft PowerPoint em modo de apresentação para assistir à gravação. A extensão `.ppsx` corresponde a uma apresentação de slides do PowerPoint. Caso a reprodução não inicie automaticamente, utilize os controles de mídia dos slides. A visualização do arquivo no repositório pode não reproduzir o conteúdo gravado.

## Arquivos do projeto

| Arquivo | Conteúdo |
| --- | --- |
| `EDA.ipynb` | Contextualização do negócio, análise exploratória, classificação NPS, outliers, correlações, comparação entre grupos, acúmulo de atritos e investigação logística |
| `modelo.ipynb` | Segmentação de detratores com padronização das variáveis e K-Means, análise dos perfis e propostas de ação |
| `desafio_nps_fase_1.csv` | Base de dados utilizada pelos dois notebooks |
| `Apresentacao_Video.ppsx` | Apresentação do trabalho com vídeo gravado nos slides |
| `requirements.txt` | Dependências diretas dos notebooks e kernel Python para execução no VS Code |
| `readme.md` | Documentação do projeto |

## Base de dados e NPS

A base contém 2.500 registros. Entre os indicadores analisados estão `nps_score`, `delivery_delay_days`, `complaints_count`, `customer_service_contacts`, `resolution_time_days`, `csat_internal_score`, `repeat_purchase_30d`, `customer_region`, `delivery_attempts` e `freight_value`.

Na classificação usual, as notas de 0 a 6 representam detratores; 7 e 8, neutros; e 9 e 10, promotores. O NPS agregado é calculado por **% de promotores − % de detratores**, variando de −100 a 100. Ele é diferente da nota individual `nps_score`.

A base possui notas decimais. A EDA arredonda as notas para classificar os clientes, enquanto o modelo seleciona detratores pelo critério `nps_score <= 6.9`. Os critérios precisam ser alinhados antes de comparar os totais dos notebooks.

## Análise exploratória

O `EDA.ipynb` examina a distribuição do NPS, valores extremos, relações entre indicadores operacionais e satisfação, diferenças entre grupos e acúmulo de atritos. Também investiga região, tentativas de entrega, frete e recompra.

Com o arredondamento adotado na EDA, foram identificados 80,04% de detratores e NPS agregado de −74,12. Atrasos, reclamações e contatos com atendimento são indicadores destacados para aprofundamento.

## Segmentação de detratores

O `modelo.ipynb` utiliza `StandardScaler` e `KMeans` com três clusters, `random_state=42` e `n_init=10`. As variáveis usadas são:

- Dias de atraso na entrega.
- Quantidade de contatos com atendimento.
- Tempo de resolução.
- Quantidade de reclamações.
- CSAT interno.

O notebook separa os detratores em treino (80%) e teste (20%). A padronização e o K-Means são ajustados no treino e aplicados ao teste. Os perfis propostos são **Atrito leve**, **Suporte lento** e **Alto atrito**, acompanhados de sugestões de ação. A clusterização agrupa clientes por similaridade; não é um modelo de previsão da nota de NPS.

## Como executar

Ambiente de referência: **Python 3.12.9**. No Windows, abra o PowerShell na pasta do projeto:

```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

No VS Code, instale as extensões Python e Jupyter. Abra os notebooks e selecione o kernel da `.venv`.

1. Mantenha `desafio_nps_fase_1.csv` na pasta dos notebooks.
2. Execute o notebook com a pasta do projeto como diretório de trabalho.
3. Execute primeiro `EDA.ipynb` e depois `modelo.ipynb`, com as células de cada notebook em ordem.

Os notebooks leem o CSV de forma independente; não é necessário compartilhar variáveis entre eles.

### Ajuste necessário no notebook de modelo

O `modelo.ipynb` utiliza `train_test_split`, mas não contém sua importação. Antes de executar a célula de separação, acrescente à célula de importações:

```python
from sklearn.model_selection import train_test_split
```

Instalar o scikit-learn não substitui essa importação no código.

## Dependências

O `requirements.txt` lista as bibliotecas utilizadas nos notebooks e o `ipykernel`, com versões do ambiente local consultado. As dependências transitivas são instaladas pelo pip. Bibliotecas da distribuição padrão, como `pathlib`, não precisam ser instaladas.

## Limitações e interpretação

As relações encontradas na EDA são descritivas e não demonstram causalidade. O arredondamento das notas e os critérios de classificação precisam ser confirmados com a origem dos dados. Os limites exploratórios de atrito não representam pontos de ruptura estatisticamente validados.

Os perfis e as propostas de ação do K-Means são hipóteses para avaliação de negócio. A separação em treino e teste, isoladamente, não comprova a qualidade ou a estabilidade dos clusters.

