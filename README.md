# desafio_dio---powerbi_Dashboard-Ecommerce
Processo de Construção do Diagrama (Esquema Estrela)

1. **Importação e Preparação (Power Query)**:
   - Carreguei Financial Sample como `Financials_origem` (backup oculto).
   - Criei IDs consistentes (ex.: ID_produto como número via condicional: Carretera=0, Montana=1 etc.).
   - Tratei nulls (substituir por 0/"Desconhecido") e tipos de dados (Text → Number).

2. **Criação das Tabelas**:
   - **F_Vendas** (fato): Units Sold, Sales, Profit, Date, Segment etc. + SK_ID (índice).
   - **D_Produtos**: SUMMARIZE por produto + colunas DAX (médias, mediana).
   - **D_Produtos_Detalhes**: Detalhes (Discount Band, Sale Price).
   - **D_Descontos**: DISTINCT(Discount, Discount Band).
   - **D_Categorias**: DISTINCT(Segment).
   - **D_Calendario**: DAX `CALENDAR(MIN(F_Vendas[Date]), MAX(F_Vendas[Date]))` + Ano/Mês.

3. **Relacionamentos (Modelo)**:
   - F_Vendas no centro: ID_produto → D_Produtos (1:*), Date → D_Calendario (*:1).
   - Ativos/Single direction. Resolvi ambiguidade desativando paths extras.

## Etapas, Funcionalidades e Funções DAX

### Etapas do Projeto
1. **ETL**: Transformei tabela única em estrela (fato + dimensões).
2. **Modelagem**: Relacionamentos para filtros cruzados (produto → vendas por data).
3. **DAX**: Medidas para KPIs + colunas calculadas em dimensões.
4. **Validação**: Slicers testados (ex.: [Segment] filtra [Total Vendas]).

### Funcionalidades Principais
- **KPIs Dinâmicos**: Totais/médias atualizam por slicers (data/produto).
- **Análise por Produto**: D_Produtos mostra max/min vendas.
- **Temporal**: YTD/ano anterior via D_Calendario.
- **Rentabilidade**: Margem % = Lucro / Vendas.

### Funções DAX Utilizadas
| Função       | Exemplo | Finalidade |
|--------------|---------|------------|
| **SUM/AVG** | `SUM(F_Vendas[Sales])` | Totais/médias vendas. |
| **CALCULATE** | `CALCULATE(AVERAGE(...), ALLEXCEPT(...))` | Contexto por produto. |
| **AVERAGEX** | `AVERAGEX(FILTER(...), [Units Sold])` | Média com filtro custom. |
| **DIVIDE**  | `DIVIDE([Lucro], [Vendas], 0)` | Margem % segura (evita /0). |
| **MEDIAN/MAX/MIN** | `MEDIAN(F_Vendas[Sales])` | Estatísticas descritivas. |
| **CALENDAR** | `CALENDAR(MIN/MAX Date)` | Tabela temporal dinâmica. |



Desenvolvido por Mara Costa - Business Analyst | Power BI | Fortaleza/CE.

