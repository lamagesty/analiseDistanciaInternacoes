# analiseDistanciaInternacoes

# GeoHealthRouter: Análise de Fluxo de Pacientes (MG)

Este projeto tem como objetivo calcular a distância e o tempo de deslocamento entre os municípios de origem e destino das internações hospitalares do **CORE** e do **SUSFácil** em Minas Gerais. A partir desses dados, o sistema gera métricas estatísticas sobre o deslocamento real enfrentado pelos pacientes.

## Passo a Passo do Processo

1.  **Obtenção dos Dados**: O processo inicia com a carga das planilhas de internação (CORE e SUSFácil) referentes ao mês de junho.
2.  **Tratamento de Dados**:
    * Filtragem de registros inválidos (linhas sem município especificado).
    * Padronização dos nomes dos municípios (remoção de acentos e conversão para maiúsculas) para garantir o cruzamento correto.
    * Identificação de pares únicos (origem-destino) para evitar o processamento redundante.
3.  **Geolocalização**:
    * Download de uma base de coordenadas de municípios brasileiros (via GitHub).
    * Cruzamento das bases para atribuir latitude e longitude a cada município de origem e destino.
4.  **Cálculo de Rotas**:
    * Iteração sobre os pares únicos utilizando a API **OSRM** (Open Source Routing Machine).
    * **Otimização**: O código verifica se a origem é igual ao destino (distância = 0) e utiliza uma sessão persistente (`requests.Session`) para acelerar as requisições, respeitando os limites da API.
5.  **Análise Estatística**: Cálculo de métricas ponderadas pelo número de internações, como média, mediana e soma total de quilometragem percorrida pelos pacientes.
6.  **Exportação**: Geração de dois arquivos Excel finais com as informações de rota incorporadas aos dados originais.

## APIs e Bibliotecas Utilizadas

* **OSRM API (Open Source Routing Machine)**: Utilizada para calcular a distância e o tempo de condução entre coordenadas geográficas. É uma API gratuita, rápida e open source.
* **pandas**: Manipulação e tratamento dos DataFrames.
* **requests**: Realização das chamadas HTTP para a API do OSRM.
* **tqdm**: Criação da barra de progresso no terminal.
* **rapidfuzz**: (Opcional) Auxilia no tratamento e comparação de nomes de municípios.
* **unidecode**: Normalização de strings (remoção de acentos).

## Pré-requisitos
Certifique-se de ter instalado as dependências antes de rodar o script:
```bash
!pip install pandas openpyxl requests tqdm rapidfuzz unidecode

## Como utilizar
1. Coloque seu arquivo OrigemDestino_CORE_SUSFÁCIL.xlsx na mesma pasta do script.
2. Certifique-se de que os nomes das abas na planilha coincidem com os nomes definidos no código (CORE e SUSFácil).
3. Execute o script. Os arquivos de saída serão gerados automaticamente.
