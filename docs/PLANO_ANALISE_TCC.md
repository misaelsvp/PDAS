# Plano de Análise TCC - Previsão de Demanda APS

## 📋 **Contexto do Projeto**

### **Mudança de Direção Estratégica**
- **Problema**: IVS não incorpora variáveis importantes para APS (doenças crônicas, morbidade)
- **Solução**: Focar em **Setores Censitários** como unidade de análise
- **Estratégia**: Combinar 10 variáveis do setor censitário + 7 indicadores Previne Brasil

### **Objetivo Final**
Desenvolver modelo preditivo de demanda por serviços de Atenção Primária à Saúde baseado em determinantes sociais da saúde.

---

## 📊 **Estrutura dos Dados**

### **1. Setor-Censitario-APS.xlsx (334 MB)**

#### **Aba "Dicionário"**
- **10 Variáveis Selecionadas** (relacionadas aos Determinantes Sociais da Saúde):
  - V00008: Crianças 0-9 anos
  - V00052: Domicílios degradados/inacabados
  - V00064: Abrigos/albergues para população em situação de rua
  - V00111: Domicílios com rede geral de água
  - V00201: Domicílios sem água encanada
  - V00238: Domicílios sem banheiro/sanitário
  - V00314: Esgoto em rio/lago/córrego/mar
  - V00401: Lixo em terreno baldio/encosta/área pública
  - V00901: Pessoas 15+ anos analfabetas
  - V01041: Pessoas 70+ anos

- **7 Indicadores Previne Brasil** (VPB01-VPB07):
  - VPB01: Gestantes com 6+ consultas pré-natal
  - VPB02: Gestantes com exames sífilis/HIV
  - VPB03: Gestantes com atendimento odontológico
  - VPB04: Mulheres com coleta citopatológico
  - VPB05: Crianças 1 ano vacinadas contra poliomielite
  - VPB06: Hipertensos com PA aferida no semestre
  - VPB07: Diabéticos com hemoglobina glicada solicitada

#### **Aba "Base"**
- Informações geográficas: região, UF, município, coordenadas
- Cobertura: Todo o Brasil
- Período: A confirmar (provavelmente 2020)

#### **Aba "dados"**
- **111 colunas** (todas as variáveis do censo)
- Dados por setor censitário com coordenadas precisas
- Estrutura: CD_MUN, MUNICIPIO, CD_DIST, DISTRITO, Subdistrito, SETOR, COORDS, LAT, LONG

#### **Aba "Cidades-APS"**
- Índices calculados por setor censitário:
  - Capital Humano
  - Infra Urbana  
  - Vulnerab. Saúde
  - Índice (geral)
- **Fórmulas identificadas**:
  - `=MÉDIA(SE(ÉNÚM(CN162246);CN162246); SE(ÉNÚM(CQ162246); CQ162246); ...)`
  - `=1-(DD162246/100)` (inversão dos indicadores Previne)
  - `=SEERRO(SE(DE162246/$DE$1 <1;DE162246/$DE$1;1); "X")`

#### **Aba "Índice"**
- Visualização com gráfico de vulnerabilidade por setor
- Pivot table com médias por município

### **2. Indicadores Previne Brasil.xlsx**

#### **Estrutura**
- **6 abas**: Dados + Página1-5 (cada aba = 1 indicador)
- **Período**: 2022 Q1 até 2025 Q1 (dados trimestrais)
- **Municípios**: 6 cidades de MG
  - BELA VISTA DE MINAS (310600)
  - BELO HORIZONTE (310620)
  - CONTAGEM (311860)
  - DIVINÓPOLIS (312230)
  - LAGOA SANTA (313760)
  - SETE LAGOAS (316720)

#### **Indicadores por Aba**
- **Dados**: VPB01 - Gestantes com 6+ consultas pré-natal
- **Página1**: VPB02 - Gestantes com exames sífilis/HIV
- **Página2**: VPB03 - Gestantes com atendimento odontológico
- **Página3**: VPB04 - Mulheres com coleta citopatológico
- **Página4**: VPB05 - Crianças 1 ano vacinadas contra poliomielite
- **Página5**: VPB06 - Hipertensos com PA aferida no semestre
- **Página6**: VPB07 - Diabéticos com hemoglobina glicada solicitada

---

## 🎯 **Estratégia de Análise**

### **1. Relacionamento dos Dados**
- **Problema**: Indicadores Previne são por município, setores censitários são mais granulares
- **Solução**: Atribuir valores municipais do Previne a todos os setores censitários do município
- **Justificativa**: Não é possível quebrar dados Previne a nível de setor censitário

### **2. Processo de Integração**
1. **Filtrar setores censitários** apenas dos 6 municípios de MG
2. **Carregar indicadores Previne** por município e trimestre
3. **Aplicar inversão** dos indicadores Previne: `1 - (valor/100)`
4. **Atribuir valores municipais** a todos os setores do município
5. **Combinar com variáveis** do setor censitário

### **3. Análise Temporal**
- **Foco**: Evolução dos indicadores por setor censitário ao longo do tempo
- **Período**: 2022 Q1 a 2025 Q1 (dados trimestrais)
- **Objetivo**: Identificar padrões temporais para previsão

---

## 📈 **Próximos Passos**

### **Imediato (para reunião de 3/10)**
1. **Carregar e explorar** dados do setor censitário
2. **Filtrar** apenas os 6 municípios de MG
3. **Integrar** com indicadores Previne Brasil
4. **Análise temporal** dos indicadores por setor
5. **Visualizações** da evolução temporal
6. **Preparar apresentação** para o professor

### **Médio Prazo**
1. **Análise exploratória** detalhada dos setores censitários
2. **Identificação de padrões** espaciais e temporais
3. **Desenvolvimento** de modelo preditivo
4. **Validação** e refinamento do modelo

---

## 🔍 **Questões em Aberto**

1. **Período temporal** dos dados do setor censitário (confirmar se é 2020)
2. **Metodologia** exata de cálculo dos índices (Capital Humano, Infra Urbana, Vulnerab. Saúde)
3. **Validação** da estratégia de atribuição de valores municipais aos setores
4. **Definição** da variável dependente para o modelo preditivo

---

## 📝 **Notas Importantes**

- **Escopo limitado**: Apenas 6 municípios de MG
- **Dados trimestrais**: Previne Brasil fornece dados por trimestre
- **Inversão necessária**: Indicadores Previne devem ser invertidos (1 - valor/100)
- **Granularidade**: Setores censitários são mais granulares que municípios
- **Integração**: Desafio de conectar dados municipais com setores censitários

---

*Arquivo criado em: 30/09/2025*
*Última atualização: 30/09/2025*
