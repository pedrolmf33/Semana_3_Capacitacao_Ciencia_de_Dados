# 🐍 Capacitação em Ciência de Dados (Semana 3) — Introdução a Python: Data Cleaning, EDA e Visualização (Case Campe Supply)

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Cleaning%20%26%20EDA-150458?logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-Data%20Processing-013243?logo=numpy&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-Data%20Visualization-4C72B0)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Plotting-11557c)
![Status](https://img.shields.io/badge/Status-Conclu%C3%ADdo-brightgreen)

Bem-vindo(a) ao repositório da **Terceira Semana da Capacitação de Ciência de Dados**!

Este projeto documenta o fluxo completo de trabalho em Ciência de Dados utilizando a linguagem **Python**, abordando desde o diagnóstico e tratamento de uma base corporativa severamente corrompida (*Data Cleaning*), passando pela Análise Exploratória de Dados (*EDA*) voltada a negócios, até a criação de gráficos executivos e *Storytelling* com dados.

---

## 🛠️ Tecnologias e Bibliotecas Utilizadas

Abaixo estão listadas as bibliotecas fundamentais empregadas em cada etapa do ciclo de tratamento e análise dos dados:

| Biblioteca | Versão / Módulo | Finalidade no Projeto |
| :--- | :--- | :--- |
| **`pandas`** | Análise e Manipulação | Leitura e exportação de planilhas (`read_excel`, `to_excel`), diagnóstico estrutural (`info`, `isna`), filtragem, operações vetorizadas em strings (`.str`), engenharia de atributos temporais (`.dt`), agregações e tabelas dinâmicas (`groupby`, `pivot_table`). |
| **`numpy`** | Computação Científica | Manipulação e imputação de valores ausentes (`np.nan`), condições lógicas vetorizadas e operações numéricas de suporte. |
| **`difflib`** | `get_close_matches` (Módulo Nativo) | Correção automatizada de erros de digitação (*typos*) e correspondência difusa (*fuzzy matching*) de nomes categóricos em relação aos padrões de referência. |
| **`matplotlib`** | `pyplot` | Configuração estrutural das figuras (dimensões `figsize`, eixos, títulos, formatação de escalas monetárias e controle de layout). |
| **`seaborn`** | Visualização Estatística | Criação de gráficos analíticos de alto nível: linhas temporais (`lineplot`), rankings categóricos (`barplot`), dispersão e detecção visual de anomalias (`boxplot`) e mapas de calor (`heatmap`). |

---

## 🎯 Contexto de Negócio: O Desafio "Campe Supply"

A **Campe Supply** é uma distribuidora que opera nas regiões **Sul, Sudeste e Nordeste**, comercializando produtos de quatro grandes setores (*Congelados, Laticínios, Bebidas e Carnes*) através dos canais de **Atacado, Atacarejo e Varejo**.

A diretoria comercial solicitou uma análise estratégica do desempenho de vendas, mas a planilha bruta original (`Campe_Supply_Corrompido.xlsx`, contendo **1.690 linhas e 13 colunas**) apresentava graves problemas estruturais e de integridade:

* **Colunas de ruído ("lixo") e duplicatas mascaradas:** Colunas com códigos aleatórios (`UNNAMED: 0`, `coluna_inutil_0`, `coluna_inutil_1`) que impediam a detecção direta de registros duplicados.
* **Poluição textual e erros de digitação:** Mais de 140 variações ortográficas nas colunas categóricas (ex.: `"Cvrnes"`, `"kuína"`, `"Vacessa"`, `"Attcarejo"`, `"tarejo"`).
* **Corrupção em variáveis financeiras:** Colunas numéricas lidas como texto (`object`) devido à infiltração de caracteres alfabéticos (ex.: `"98.275200000000b1"`, `"135u7164"`, `"75z76"`).
* **Outliers extremos e quebras de regras matemáticas:** Violação da equação fundamental do negócio ($\text{Lucro} = \text{Faturamento} \times \text{Margem de Lucro}$), com lucros irreais na casa dos milhões e faturamentos inflados.
* **Dados ausentes (`NaN` / `NaT`):** Cerca de 5% de valores nulos espalhados por todas as variáveis relevantes da base.

---

## 🗂️ Estrutura do Repositório

```text
├── Campe_Supply_Corrompido.xlsx                              # Base de dados bruta original (1.690 x 13)
├── README.md                                                 # Documentação completa do projeto
├── Exercicios/                                               # Desenvolvimento prático da capacitação
│   ├── Aula_01_Exercicios_Setup_e_Diagnostico.ipynb          # Etapa 1: Diagnóstico estrutural e limpeza inicial
│   ├── Aula_02_Exercicios_Limpeza_Textos_e_Funcoes.ipynb     # Etapa 2: Padronização textual e imputação lógica
│   ├── Aula_03_Exercicios_Limpeza_Numerica_e_Outliers.ipynb  # Etapa 3: Tratamento numérico, outliers e datas
│   ├── Aula_04_Exercicios_Analise_Exploratoria_Negocio.ipynb # Etapa 4: KPIs, agrupamentos, rankings e matrizes
│   ├── Aula_05_Exercicios_Visualizacao_e_Storytelling.ipynb  # Etapa 5: Visualização gráfica e insights visuais
│   └── Campe_Supply_Limpo.xlsx                               # Base tratada e auditada (1.543 x 12)
└── Respostas/                                                # Gabaritos de referência do treinamento
```

---

## 🛠️ Pipeline de Limpeza e Tratamento dos Dados (*Data Cleaning*)

Todo o processo de tratamento foi desenvolvido nos cadernos da pasta `Exercicios/`, seguindo uma metodologia estruturada em três etapas progressivas:

### 1️⃣ Etapa 1: Setup, Importação e Diagnóstico Estrutural (`Aula_01`)
1. **Inspeção Inicial:** Carregamento da aba `'Campe Supply'` com `pd.read_excel()` e levantamento de tipos e volumetria com `.shape` e `.info()`. Confirmou-se que 12 das 13 colunas estavam tipadas como `object` (apenas `data da venda` era `datetime64[ns]`).
2. **Mapeamento de Nulos:** Quantificação absoluta e percentual de valores ausentes. A coluna `UNNAMED: 0` apresentava **98,17%** de nulos (1.659 linhas vazias), enquanto as colunas de negócio tinham entre **4,73% e 5,21%** de dados faltantes (80 a 88 linhas).
3. **Desmascaramento da 1ª Onda de Duplicatas:**
   * Antes da remoção das colunas inúteis, `df.duplicated().sum()` retornava **0 duplicatas**, pois as strings aleatórias de `coluna_inutil_0` e `coluna_inutil_1` tornavam cada linha artificialmente única.
   * Após o descarte (`df.drop`) de `['UNNAMED: 0', 'coluna_inutil_0', 'coluna_inutil_1']`, **35 duplicatas primárias** foram reveladas e eliminadas com `df.drop_duplicates()`, reduzindo a base para **1.655 linhas e 10 colunas**.
4. **Padronização de Cabeçalhos (*snake_case*):** Tratamento vetorial dos nomes das colunas com `.str.strip().str.lower().str.replace(' ', '_').str.replace('ã', 'a')`, resultando em: `['data_da_venda', 'setor', 'produto', 'vendedor', 'regiao', 'estado', 'cliente', 'faturamento', 'margem_de_lucro', 'lucro']`.

---

### 2️⃣ Etapa 2: Limpeza de Texto, Funções e Cruzamento Lógico (`Aula_02`)
1. **Diagnóstico de Categorias:** Inspeção com `.nunique()` e `.value_counts()` revelou alta fragmentação por erros de digitação: `setor` (139 valores únicos), `produto` (152), `vendedor` (152), `regiao` (127), `estado` (160) e `cliente` (149).
2. **Correção Ortográfica Automatizada (`difflib`):**
   * Criação da função `corrigir_palavras()` utilizando `difflib.get_close_matches(palavra, lista_oficial, n=1, cutoff=0.6)` aplicada via `.apply()`.
   * Todas as variações incorretas foram mapeadas automaticamente para as categorias oficiais da empresa:
     * **Setores:** `Congelados`, `Latícinio`, `Bebidas`, `Carnes`
     * **Produtos:** `Queijo`, `Sorvete`, `Frango`, `Refrigerantes`, `Cervejas`, `Bovina`, `Leite`, `Lasanha`, `Pizza`, `Suína`, `Sucos`, `Coalhada`
     * **Vendedores:** `Vanessa`, `Amaral`, `Leo`, `Daniele`, `Jonas`, `Evandro`
     * **Regiões:** `Sudeste`, `Nordeste`, `Sul`
     * **Estados:** `Rio de Janeiro`, `Minas Gerais`, `Bahia`, `Rio Grande do Norte`, `São Paulo`, `Sergipe`, `Rio Grande do Sul`, `Paraná`, `Santa Catarina`
     * **Clientes:** `Atacarejo`, `Varejo`, `Atacado`
3. **Recuperação Inteligente de Nulos (Cruzamento Lógico):**
   * Utilizando a hierarquia de negócio ($\text{Produto} \rightarrow \text{Setor}$ e $\text{Estado} \rightarrow \text{Região}$), aplicou-se `.fillna()` combinado com `.map()` sobre dicionários de relacionamento.
   * Os nulos de `setor` caíram de **85 para apenas 4**, e os de `regiao` caíram de **83 para apenas 5**.
4. **Tratamento de Nulos Categóricos Remanescentes:** Os registros onde tanto o nível micro (`produto`/`estado`) quanto o macro (`setor`/`regiao`) estavam ausentes foram preenchidos com a categoria `'Não informado'` para evitar perda de registros financeiros válidos.

---

### 3️⃣ Etapa 3: Limpeza Numérica, Regras Matemáticas e Outliers (`Aula_03`)
1. **Coerção Numérica:** Identificaram-se **33 strings corrompidas** em `faturamento`, **29** em `margem_de_lucro` e **32** em `lucro`. A conversão com `pd.to_numeric(..., errors='coerce')` transformou esses ruídos em `NaN` para posterior reconstrução matemática, elevando o total temporário de nulos para 115 (`faturamento`), 106 (`margem_de_lucro`) e 116 (`lucro`).
2. **Detecção e Correção Cirúrgica de 5 Outliers / Erros Matemáticos:**
   * Através do filtro de consistência `abs(faturamento * margem_de_lucro - lucro) > 0.01`, foram isoladas e corrigidas **5 anomalias graves**:
     * **Índices 304 e 1304 (Lucro inflado na casa dos milhões):** Valores de `R$ 2.027.828,00` e `R$ 3.260.000,00` (maiores que o próprio faturamento). Corrigidos recalculando $\text{Lucro} = \text{Faturamento} \times \text{Margem}$.
     * **Índices 975 e 1426 (Margem zerada com lucro positivo):** Margens registradas como `0.00` em vendas lucrativas. Corrigidas recalculando $\text{Margem} = \text{Lucro} / \text{Faturamento}$.
     * **Índice 572 (Faturamento multiplicado por 10):** Faturamento de `R$ 8.581,60` incompatível com a margem (`0.06`) e lucro (`R$ 51,49`). Corrigido recalculando $\text{Faturamento} = \text{Lucro} / \text{Margem}$ (`R$ 858,16`).
3. **Imputação Matemática e Estatística:**
   * Preenchimento cruzado das células `NaN` onde 2 das 3 variáveis financeiras estavam presentes usando a equação do negócio, reduzindo os nulos de `faturamento` (115 $\rightarrow$ 20), `margem_de_lucro` (106 $\rightarrow$ 16) e `lucro` (116 $\rightarrow$ 13).
   * Nos poucos casos restantes (onde mais de uma variável financeira era nula na mesma linha), aplicou-se a imputação pela **mediana** (`.fillna(df[col].median())`) e arredondamento para 2 casas decimais (`.round(2)`), zerando os nulos numéricos.
4. **2ª Onda de Duplicatas e Engenharia de Recursos (*Feature Engineering*):**
   * Com textos e números corrigidos, linhas idênticas que antes divergiam apenas por erros de digitação tornaram-se visíveis. Uma nova chamada de `df.drop_duplicates()` removeu **112 duplicatas ocultas**, consolidando a base limpa em **1.543 transações únicas**.
   * Extração das variáveis temporais `mes` (`dt.month`) e `trimestre` (`dt.quarter`) a partir de `data_da_venda`.
   * Exportação da base higienizada para `Exercicios/Campe_Supply_Limpo.xlsx` (**1.543 linhas e 12 colunas**).

---

## 📊 Análise Exploratória de Dados — EDA (`Aula_04`)

Com a base `Campe_Supply_Limpo.xlsx` auditada, foram calculados os indicadores estratégicos utilizando `.groupby()`, `.agg()`, `pd.pivot_table()` e `.query()`:

### 📌 1. KPIs Globais da Campe Supply
| Indicador | Resultado Consolidado |
| :--- | :--- |
| **Faturamento Total** | **R$ 1.378.357,89** |
| **Lucro Líquido Total** | **R$ 165.048,52** |
| **Ticket Médio por Venda** | **R$ 893,30** |
| **Margem de Lucro Média** | **11,95%** |

### 📌 2. Ranking de Performance por Vendedor
| Vendedor | Qtd. Vendas | Faturamento Total (R$) | Lucro Total (R$) | Ticket Médio (R$) | Margem Média |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **1º Vanessa** | 439 | 402.237,69 | **49.249,80** | **916,26** | 12% |
| **2º Amaral** | 324 | 283.260,92 | **33.154,40** | 874,26 | 12% |
| **3º Daniele** | 222 | 196.345,64 | **24.940,44** | 884,44 | 12% |
| **4º Leo** | 239 | 211.490,96 | **24.578,85** | 884,90 | 12% |
| **5º Jonas** | 126 | 112.586,05 | **13.436,79** | 893,54 | 12% |
| **6º Evandro** | 116 | 104.935,55 | **11.579,90** | 904,62 | 11% |

> 💡 **Insight Comercial:** **Vanessa** lidera isoladamente em volume de vendas (439), faturamento (`R$ 402,2 mil`), lucro (`R$ 49,2 mil`) e também detém o **maior Ticket Médio (`R$ 916,26`)**. Além disso, observa-se que **Daniele** supera **Leo** em lucro total (`R$ 24,9 mil` contra `R$ 24,5 mil`) mesmo tendo realizado 17 vendas a menos, demonstrando maior eficiência em margem.

### 📌 3. Desempenho por Setor e Top Produtos
* **Por Setor:** A operação é equilibrada entre os 4 setores, com **Congelados** na liderança (`R$ 360.053,52` de faturamento e `R$ 43.689,70` de lucro, com ticket médio de `R$ 925,59`), seguido por **Laticínios** (`R$ 346.812,53` / lucro de `R$ 41.292,04`), **Carnes** (`R$ 333.302,99` / lucro de `R$ 40.026,95`) e **Bebidas** (`R$ 335.303,47` / lucro de `R$ 39.645,80`).
* **Produtos Carros-Chefe:** **Sorvete** (`R$ 209,9 mil` de faturamento / `R$ 25,9 mil` de lucro) e **Queijo** (`R$ 198,2 mil` de faturamento / `R$ 23,7 mil` de lucro) são os dois produtos mais rentáveis da distribuidora, seguidos por *Cervejas*, *Frango*, *Refrigerantes*, *Carne Bovina* e *Leite*.

### 📌 4. Matriz Região vs. Canal de Cliente e Filtros VIP
* A Tabela Dinâmica (`pivot_table`) demonstrou forte equilíbrio entre as regiões **Nordeste** (`R$ 451,7 mil`) e **Sudeste** (`R$ 450,2 mil`), seguidas pelo **Sul** (`R$ 405,3 mil`).
* No **Nordeste**, o canal de maior faturamento é o **Varejo** (`R$ 162,4 mil`), enquanto no **Sudeste** a liderança é do **Atacado** (`R$ 151,9 mil`), e no **Sul** destaca-se o **Atacarejo** (`R$ 138,4 mil`).
* O filtro avançado `.query("regiao == 'Sudeste' and faturamento > 1500")` permitiu isolar as transações **VIP do Sudeste** (com tickets acima de `R$ 2.000,00`), revelando que todas ocorreram no mês de **dezembro** nos estados de Minas Gerais e Rio de Janeiro.

---

## 📈 Visualização de Dados e Storytelling (`Aula_05`)

Para comunicar os resultados de forma executiva utilizando `matplotlib` e `seaborn`, foram construídas quatro visualizações principais:

1. **Gráfico de Linha (`sns.lineplot`) — Sazonalidade Mensal do Faturamento:**
   * Evidencia o comportamento sazonal das vendas ao longo dos 12 meses do ano, revelando estabilidade entre janeiro e outubro e um **forte pico sazonal nos meses de novembro e dezembro** ( provavelmente impulsionado pelas festas de fim de ano e vendas VIP).
2. **Gráfico de Barras Horizontais (`sns.barplot`) — Ranking de Lucro por Vendedor:**
   * Apresenta a comparação direta da geração de lucro por vendedor com rótulos financeiros formatados na extremidade de cada barra (`ax.bar_label`), destacando a liderança de Vanessa e Amaral.
3. **Boxplot (`sns.boxplot`) — Distribuição do Ticket por Setor:**
   * Exibe a mediana, quartis e dispersão dos valores de faturamento em cada setor, permitindo visualizar os *outliers* reais de vendas de alto valor (acima de `R$ 1.500,00`) sem a distorção que seria causada pelo erro de digitação de `R$ 8.581,60` removido na Aula 03.
4. **Mapa de Calor (`sns.heatmap`) — Matriz Estado vs. Canal de Cliente:**
   * Cruza a intensidade de faturamento entre os 9 estados de atuação e os 3 canais de venda (*Atacado, Atacarejo e Varejo*), facilitando a identificação visual rápida dos focos de maior receita regional.

---

## 🚀 Como Executar o Projeto

1. **Clone o repositório:**
   ```bash
   git clone <url-do-repositorio>
   cd capacitacao_python_DATA-CLEANING
   ```
2. **Instale as dependências necessárias:**
   ```bash
   pip install pandas numpy matplotlib seaborn jupyter
   ```
3. **Inicie o Jupyter Notebook ou JupyterLab (ou abra no VS Code / PyCharm):**
   ```bash
   jupyter notebook
   ```
4. Navegue até a pasta `Exercicios/` e execute os notebooks sequencialmente da **Aula 01** à **Aula 05**.
