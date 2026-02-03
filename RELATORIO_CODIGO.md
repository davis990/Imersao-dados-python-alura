# 📊 Relatório Completo: Dashboard de Salários na Área de Dados

---

## 📋 Índice
1. [Visão Geral](#visão-geral)
2. [Instalação de Frameworks](#instalação-de-frameworks)
3. [Estrutura do Código](#estrutura-do-código)
4. [Análise Detalhada de Cada Item](#análise-detalhada-de-cada-item)
5. [Erros Encontrados e Correções](#erros-encontrados-e-correções)
6. [Como Funcionam os DataFrames](#como-funcionam-os-dataframes)
7. [Entendendo If/Else no Contexto](#entendendo-ifelse-no-contexto)
8. [Comportamento dos Gráficos com Filtros](#comportamento-dos-gráficos-com-filtros)

---

## 🎯 Visão Geral

Este dashboard foi desenvolvido usando **Streamlit** para criar uma aplicação web interativa que analisa dados salariais da área de dados. O aplicativo oferece filtros dinâmicos e visualizações de dados através de gráficos interativos usando **Plotly**.

**Objetivo**: Permitir que usuários explorem dados salariais por:
- Ano
- Nível de senioridade
- Tipo de contrato
- Tamanho da empresa

---

## 🔧 Instalação de Frameworks

### Frameworks Utilizados

1. **Pandas** - Manipulação e análise de dados
2. **Streamlit** - Framework para criar aplicações web
3. **Plotly** - Biblioteca para gráficos interativos

### Passo a Passo para Instalação

#### 1. Criar um Ambiente Virtual (Recomendado)

```bash
# Abrir PowerShell no diretório do projeto
cd C:\Users\conta\Documents\tentatuva

# Criar ambiente virtual
python -m venv .venv

# Ativar o ambiente virtual
.\.venv\Scripts\Activate.ps1
```

**Nota**: Se receber erro de execução de scripts, execute:
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser -Force
```

#### 2. Criar arquivo requirements.txt

```
pandas
streamlit
plotly
watchdog
```

#### 3. Instalar as Dependências

```bash
pip install -r requirements.txt
```

#### 4. Executar o Aplicativo

```bash
streamlit run app.py
```

### Versões Recomendadas

- **Python**: 3.8+
- **Pandas**: 2.0+
- **Streamlit**: 1.28+
- **Plotly**: 5.0+

---

## 📝 Estrutura do Código

```
app.py
├── Importações (linhas 1-3)
├── Configuração da Página (linhas 5-9)
├── Carregamento de Dados (linhas 11-12)
├── Criação de Filtros (linhas 15-30)
├── Aplicação de Filtros (linhas 33-37)
├── Título e Descrição (linhas 39-40)
├── Métricas Gerais (linhas 42-52)
├── Gráficos (linhas 54-128)
└── Dados Detalhados (linhas 130-131)
```

---

## 🔍 Análise Detalhada de Cada Item

### 1. **Importações (Linhas 1-3)**

```python
import pandas as pd
import streamlit as st
import plotly.express as px
```

| Biblioteca | Função |
|-----------|--------|
| **pandas** | Manipular arquivos CSV e criar DataFrames |
| **streamlit** | Criar a interface do aplicativo web |
| **plotly.express** | Criar gráficos interativos de alta qualidade |

---

### 2. **Configuração da Página (Linhas 5-9)**

```python
st.set_page_config(
    page_title="Dashboard de Salários na Área de Dados",
    page_icon="📊",
    layout="wide",
)
```

**O que faz**:
- `page_title`: Define o título que aparece na aba do navegador
- `page_icon`: Ícone exibido na aba (emoji)
- `layout="wide"`: Usa a largura total da tela (em vez de layout centrado)

---

### 3. **Carregamento de Dados (Linhas 11-12)**

```python
df = pd.read_csv("https://raw.githubusercontent.com/vqrca/dashboard_salarios_dados/refs/heads/main/dados-imersao-final.csv")
```

**O que faz**:
- Lê um arquivo CSV de uma URL do GitHub
- Armazena os dados em um DataFrame chamado `df`
- Este DataFrame contém todas as linhas e colunas dos dados originais

**DataFrame resultante**:
```
Colunas esperadas: ano, senioridade, contrato, tamanho_empresa, cargo, usd, remoto, residencia_iso3, ...
Linhas: centenas de registros de salários
```

---

### 4. **Criação de Filtros (Linhas 15-30)**

```python
# Exemplo - Filtro de Anos
anos_disponiveis = sorted(df['ano'].unique())
anos_selecionados = st.sidebar.multiselect('Ano', anos_disponiveis, default=anos_disponiveis)
```

**Passo a passo**:
1. `df['ano'].unique()` - Obtém valores únicos da coluna 'ano'
2. `sorted()` - Ordena alfabeticamente/numericamente
3. `st.sidebar.multiselect()` - Cria um filtro interativo na barra lateral
4. `default=anos_disponiveis` - Seleciona todos por padrão

**Resultado**: Uma lista interativa onde o usuário pode desselecionar filtros

---

### 5. **Aplicação de Filtros (Linhas 33-37)**

```python
df_filtrado = df[
    (df['ano'].isin(anos_selecionados)) &
    (df['senioridade'].isin(senioridade_selecionadas)) &
    (df['contrato'].isin(contratos_selecionados)) &
    (df['tamanho_empresa'].isin(tamanhos_selecionados))
]
```

**Como funciona**:
- `.isin()` - Verifica se o valor está na lista de selecionados
- `&` - Operador AND (todas as condições devem ser verdadeiras)
- **Resultado**: Um novo DataFrame com apenas as linhas que atendem TODOS os filtros

**Exemplo**:
```
Se o usuário seleciona:
- Anos: [2023, 2024]
- Senioridade: [Senior, Junior]

df_filtrado conterá apenas linhas onde:
  ano IN (2023, 2024) AND senioridade IN (Senior, Junior)
```

---

### 6. **Métricas Gerais (Linhas 42-52)**

```python
if not df_filtrado.empty:
    salario_medio = df_filtrado["usd"].mean()
    salario_maximo = df_filtrado["usd"].max()
    total_respondentes = df_filtrado.shape[0]
    cargos_mais_frequentes = df_filtrado["cargo"].mode()[0]
else:
    salario_medio, salario_maximo, total_respondentes, cargos_mais_frequentes = 0, 0, 0, ""
```

| Métrica | Função | Método |
|---------|--------|--------|
| **Salário Médio** | Média de todos os salários | `.mean()` |
| **Salário Máximo** | Maior salário | `.max()` |
| **Total de Respondentes** | Quantidade de linhas | `.shape[0]` |
| **Cargo Mais Frequente** | Valor que mais se repete | `.mode()[0]` |

---

### 7. **Exibição de Métricas (Linhas 54-57)**

```python
col1, col2, col3, col4 = st.columns(4)
col1.metric("Salário médio", f"${salario_medio:,.0f}")
col2.metric("Salário máximo", f"${salario_maximo:,.0f}")
col3.metric("Total de respondentes", f"{total_respondentes:,}")
col4.metric("Cargo mais frequente", f"{cargos_mais_frequentes}")
```

**O que faz**:
- Cria 4 colunas na mesma linha
- Cada coluna exibe uma métrica
- `f"${salario_medio:,.0f}"` - Formata como moeda com separadores de milhares

---

### 8. **Gráficos**

#### 8.1 Gráfico de Barras - Top 10 Cargos (Linhas 61-74)

```python
top_cargos = df_filtrado.groupby("cargo")["usd"].mean().nlargest(10).sort_values(ascending=True).reset_index()
```

**Passo a passo**:
1. `.groupby("cargo")` - Agrupa por cargo
2. `["usd"].mean()` - Calcula média de salário para cada cargo
3. `.nlargest(10)` - Pega os 10 maiores
4. `.sort_values(ascending=True)` - Ordena do menor para o maior
5. `.reset_index()` - Converte de Series para DataFrame

**Resultado**:
```
DataFrame com 2 colunas:
- cargo: nome do cargo
- usd: salário médio
```

**Por que `.reset_index()` é necessário**:
- Sem ele: Plotly recebe uma Series (1D), não consegue encontrar as colunas
- Com ele: Plotly recebe um DataFrame (2D) com colunas nomeadas

---

#### 8.2 Gráfico de Histograma - Distribuição de Salários (Linhas 78-84)

```python
grafico_hist = px.histogram(
    df_filtrado,
    x='usd',
    nbins=30,
    title="Distribuição de salários anuais"
)
```

**O que faz**:
- Divide os salários em 30 "bins" (faixas)
- Mostra quantas pessoas ganham em cada faixa
- Útil para ver se a distribuição é normal ou assimétrica

---

#### 8.3 Gráfico de Pizza - Proporção de Trabalho Remoto (Linhas 89-100)

```python
remoto_contagem = df_filtrado['remoto'].value_counts().reset_index()
remoto_contagem.columns = ['tipo_trabalho', 'quantidade']
```

**O que faz**:
- `.value_counts()` - Conta quantas vezes cada valor aparece
- Exemplo resultado:
  ```
  Remoto: 150
  Presencial: 100
  Híbrido: 50
  ```

---

#### 8.4 Gráfico de Mapa - Salários por País (Linhas 117-128)

```python
df_ds = df_filtrado[df_filtrado['cargo'] == 'Data Scientist']
media_ds_pais = df_ds.groupby('residencia_iso3')['usd'].mean().reset_index()
grafico_paises = px.choropleth(media_ds_pais, ...)
```

**O que faz**:
- Filtra apenas Data Scientists
- Agrupa por país (ISO3 code)
- Mostra salário médio em mapa com cores

---

## ❌ Erros Encontrados e Correções

### Erro 1: AttributeError - upgrade_layout()

**Sintoma**:
```
AttributeError: 'Figure' object has no attribute 'upgrade_layout'
```

**Linha afetada**: 73 (original)

**Problema**:
```python
grafico_cargos.upgrade_layout(title_x=0.1)  # ❌ ERRADO
```

**Causa**: Plotly não tem método `upgrade_layout()`, o correto é `update_layout()`

**Correção**:
```python
grafico_cargos.update_layout(title_x=0.1)  # ✅ CORRETO
```

---

### Erro 2: Parênteses Desbalanceados

**Sintoma**:
```
SyntaxError: invalid syntax
```

**Linha afetada**: 120-126 (original)

**Problema**:
```python
grafico_paises = px.choropleth(media_ds_pais,  # ❌ Abre parêntese
    locations='residencia_iso3',
    ...
    labels={...})  # ❌ Fecha parêntese da função, mas a indentação está errada
```

**Causa**: Falta de parêntese de fechamento correto

**Correção**:
```python
grafico_paises = px.choropleth(
    media_ds_pais,
    locations='residencia_iso3',
    ...
    labels={...}
)  # ✅ Parêntese correto
```

---

### Erro 3: ValueError - Coluna não encontrada

**Sintoma**:
```
ValueError: Value of 'y' is not the name of a column in 'data_frame'.
Expected one of ['usd'] but received: cargo
```

**Linha afetada**: 66-72 (no gráfico de barras)

**Problema**:
```python
top_cargos = df_filtrado.groupby("cargo")["usd"].mean().nlargest(10).sort_values(ascending=True)
# ❌ Retorna uma Series, não um DataFrame
# Series só tem 1 coluna: 'usd'
# Plotly procura por 'cargo' e não encontra

grafico_cargos = px.bar(top_cargos, x='usd', y='cargo')
```

**Causa**: `groupby()` + `mean()` + `nlargest()` retorna uma Series (índice simples), não um DataFrame

**Correção**:
```python
top_cargos = df_filtrado.groupby("cargo")["usd"].mean().nlargest(10).sort_values(ascending=True).reset_index()
# ✅ .reset_index() converte Series para DataFrame com colunas 'cargo' e 'usd'
```

**Visualização da diferença**:

| Antes (Series) | Depois (DataFrame) |
|---|---|
| `cargo: Python Developer -> 5000` | `cargo \| usd` |
| `Data Engineer -> 6000` | `Python Developer \| 5000` |
| | `Data Engineer \| 6000` |

---

## 📊 Como Funcionam os DataFrames

### O que é um DataFrame?

Um DataFrame é como uma tabela em Excel:

```
┌─────────┬────────────────┬──────┐
│ Index   │ cargo          │ usd  │
├─────────┼────────────────┼──────┤
│ 0       │ Data Scientist │ 5500 │
│ 1       │ Data Engineer  │ 6000 │
│ 2       │ Analyst        │ 4200 │
└─────────┴────────────────┴──────┘
```

### Operações Principais

#### 1. Seleção de Coluna

```python
df['cargo']  # Retorna uma Series (1 coluna)
df[['cargo', 'usd']]  # Retorna um DataFrame (2 colunas)
```

#### 2. Filtro (Conditional Selection)

```python
df[df['usd'] > 5000]  # Retorna linhas onde usd > 5000
df[df['cargo'] == 'Data Scientist']  # Retorna linhas onde cargo é 'Data Scientist'
```

#### 3. Múltiplos Filtros (AND/OR)

```python
# AND - todas as condições devem ser verdadeiras
df[(df['usd'] > 5000) & (df['cargo'] == 'Data Scientist')]

# OR - pelo menos uma condição deve ser verdadeira
df[(df['usd'] > 5000) | (df['cargo'] == 'Data Analyst')]
```

#### 4. Agregações

```python
df['usd'].mean()      # Média
df['usd'].max()       # Máximo
df['usd'].min()       # Mínimo
df['usd'].sum()       # Soma
df['usd'].count()     # Contagem
```

#### 5. GroupBy (Agrupamento)

```python
# Salário médio por cargo
df.groupby('cargo')['usd'].mean()
# Resultado: Series com cargo como índice e usd como valor

# Múltiplas agregações
df.groupby('cargo').agg({
    'usd': ['mean', 'max', 'min']
})
```

#### 6. Conversão de Series para DataFrame

```python
# Series
series = df.groupby('cargo')['usd'].mean()  # ← Tipo: Series

# DataFrame
df_novo = series.reset_index()  # ← Tipo: DataFrame
# Colunas: ['cargo', 'usd']
```

### No Contexto do Código

```python
# Linha 65-66: Carrega todos os dados
df = pd.read_csv(url)  # ← DataFrame original com milhares de linhas

# Linhas 33-37: Filtra os dados
df_filtrado = df[(df['ano'].isin(...)) & (...)]  # ← DataFrame filtrado

# Linhas 48-51: Calcula métricas a partir do DataFrame filtrado
salario_medio = df_filtrado["usd"].mean()  # ← Extrai coluna, calcula média
```

---

## ⚖️ Entendendo If/Else no Contexto

### Estructura Geral

```python
if condição:
    # Código executado se condição for VERDADEIRA
    ...
else:
    # Código executado se condição for FALSA
    ...
```

### No Código: Verificar se DataFrame está Vazio

```python
if not df_filtrado.empty:
    # Se há dados para exibir
    salario_medio = df_filtrado["usd"].mean()
    ...
else:
    # Se não há dados (filtros removeram tudo)
    salario_medio = 0
    ...
```

**Por que é importante**?

Quando o usuário aplica filtros muito restritivos, pode não haver dados:

```
Cenário: Usuário seleciona
- Ano: 2024
- Senioridade: Junior
- Contrato: Freelance
- Empresa: Startup

Resultado: df_filtrado = DataFrame vazio (0 linhas)
```

Se não usarmos `if/else`:
```python
salario_medio = df_filtrado["usd"].mean()  # ❌ Erro! DataFrame vazio não tem coluna 'usd'
```

Com `if/else`:
```python
if not df_filtrado.empty:
    salario_medio = df_filtrado["usd"].mean()  # ✅ Calcula normalmente
else:
    salario_medio = 0  # ✅ Define como 0 para evitar erro
    st.warning("Nenhum dado encontrado")  # ✅ Mostra aviso ao usuário
```

### Todos os If/Else no Código

| Linha | Condição | Verdadeiro | Falso |
|------|----------|-----------|-------|
| 45 | `not df_filtrado.empty` | Calcula métricas | Define como 0 |
| 63 | `not df_filtrado.empty` | Exibe gráfico barras | Mostra aviso |
| 78 | `not df_filtrado.empty` | Exibe histograma | Mostra aviso |
| 89 | `not df_filtrado.empty` | Exibe gráfico pizza | Mostra aviso |
| 110 | `not df_filtrado.empty` | Exibe mapa | Mostra aviso |

---

## 📈 Comportamento dos Gráficos com Filtros

### Fluxo Completo de Filtragem

```
1. Usuário abre o aplicativo
   ↓
2. Streamlit executa o código de cima para baixo
   ↓
3. Lê todos os dados da URL
   ↓
4. Cria filtros na barra lateral
   ↓
5. Usuário seleciona filtros (ex: Ano = 2024)
   ↓
6. Streamlit RE-EXECUTA o código inteiro com novo filtro
   ↓
7. df_filtrado recebe apenas dados de 2024
   ↓
8. Gráficos são atualizados automaticamente
```

### Exemplo Prático: Filtrar por Ano

**Estado Inicial**:
```
df contém: 2020, 2021, 2022, 2023, 2024
anos_selecionados = [2020, 2021, 2022, 2023, 2024] (todos selecionados)

df_filtrado contém: TODAS as linhas (5 anos × 1000 linhas = 5000 linhas)
salario_medio = $85.000
```

**Usuário deseleciona 2020-2023, deixa apenas 2024**:
```
df_filtrado contém: APENAS linhas de 2024 (1000 linhas)
salario_medio = $92.000 (maior porque 2024 tem salários mais altos)
```

**Novo resultado é exibido automaticamente**:
- Métricas atualizadas
- Gráficos redesenhados
- Tudo happens porque Streamlit re-executa o script

### Como Cada Gráfico Reage aos Filtros

#### 1. Gráfico de Barras - Top 10 Cargos

```
Filtro original: [2020, 2021, 2022, 2023, 2024]
Top 10 cargos de TODOS os anos

Filtro novo: [2024]
Top 10 cargos de APENAS 2024
↓
Os cargos mais bem pagos em 2024 podem ser diferentes de 2023
```

**Comportamento**:
- `df_filtrado` é recalculado com o novo filtro
- `groupby("cargo")` agrupa apenas dados filtrados
- Top 10 pode mudar completamente

#### 2. Gráfico de Histograma - Distribuição de Salários

```
Filtro original: Distribuição de 5000 salários (2020-2024)
Curva suave, pode ter vários picos

Filtro novo: Distribuição de 1000 salários (2024)
Curva diferente, dados mais concentrados
```

**Comportamento**:
- Histograma se redesenha com novo `df_filtrado`
- 30 bins redistribuem dados filtrados
- Forma da distribuição pode mudar significativamente

#### 3. Gráfico de Pizza - Proporção Remoto/Presencial

```
Filtro original (todos os anos): 
- Remoto: 40%
- Presencial: 35%
- Híbrido: 25%

Filtro novo (apenas 2024):
- Remoto: 65%  ← Aumentou em 2024
- Presencial: 20%
- Híbrido: 15%
```

**Comportamento**:
- `value_counts()` recalcula proporções
- Tamanho das fatias muda
- Percentuais atualizados

#### 4. Gráfico de Mapa - Salários de Data Scientists por País

```
Filtro original (todos os anos): 
- USA: $120.000
- UK: $95.000
- Brasil: $45.000
- Alemanha: $105.000

Filtro novo (apenas 2024):
- USA: $135.000  ← Aumentou em 2024
- UK: $102.000
- Brasil: $52.000
- Alemanha: $112.000

Cores do mapa se intensificam onde salários são maiores
```

**Comportamento**:
- Apenas Data Scientists do filtro selecionado
- Médias recalculadas por país
- Escala de cores atualizada (vermelho = maior, verde = menor)

### Ordem de Execução (Importante!)

```python
# 1️⃣ Dados brutos são carregados
df = pd.read_csv(url)

# 2️⃣ Filtros são criados (usuário interage aqui)
anos_selecionados = st.sidebar.multiselect('Ano', ...)

# 3️⃣ Dados são filtrados
df_filtrado = df[df['ano'].isin(anos_selecionados) & ...]

# 4️⃣ Gráficos usam df_filtrado
grafico = px.bar(df_filtrado, ...)

# ⚠️ Se colocar gráficos ANTES da filtragem, usará todos os dados!
```

---

## 🎯 Resumo Executivo

| Aspecto | Descrição |
|---------|-----------|
| **Objetivo** | Explorar dados salariais com filtros interativos |
| **Tecnologia** | Streamlit (web), Pandas (dados), Plotly (gráficos) |
| **Tipo de Aplicação** | Dashboard interativo em tempo real |
| **Principais Funcionalidades** | 4 filtros + 4 gráficos + métricas resumidas |
| **Linguagem** | Python 3.8+ |
| **Instalação** | `pip install -r requirements.txt` |
| **Execução** | `streamlit run app.py` |

---

## 📞 Recursos Adicionais

- [Documentação Streamlit](https://docs.streamlit.io/)
- [Documentação Pandas](https://pandas.pydata.org/docs/)
- [Documentação Plotly](https://plotly.com/python/)
- [Tutorial Plotly Graph Objects](https://plotly.com/python/graph-objects/)

---

**Documento criado em**: 2 de fevereiro de 2026  
**Versão**: 1.0  
**Status**: Completo ✅
