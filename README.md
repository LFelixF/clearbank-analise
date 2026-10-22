# 📊 Processamento e Análise de Transações Financeiras

Este projeto consiste em um pipeline em Python (desenvolvido em Jupyter Notebook) para **leitura, limpeza, validação, consolidação de métricas mensais e detecção de anomalias (transações suspeitas)** a partir de uma base de dados bruta em formato CSV (`transacoes.csv`). O resultado consolidado é exibido no console e exportado para um arquivo estruturado em JSON (`relatorio.json`).

---

## 🎯 Funcionalidades

1. **Ingestão de Dados (`ler_transacoes`)**:
   - Realiza a leitura do arquivo `transacoes.csv` utilizando `csv.DictReader` com codificação `UTF-8`, convertendo cada linha em um dicionário.
   - Possui tratamento de exceção para arquivos inexistentes (`FileNotFoundError`).

2. **Limpeza e Validação de Dados (`validar_transacao`, `validar_formato_data_transacao`, `validar_valor_transacao`)**:
   - **ID**: Verifica se o identificador pode ser convertido para um número inteiro (`int`).
   - **Data**: Valida se a data está no formato `YYYY-MM-DD` e a normaliza para o período mensal `YYYY-MM`.
   - **Valor**: Substitui vírgulas por pontos, converte para `float` e garante que o valor seja estritamente positivo (`> 0`), não nulo (`NaN`) e não infinito (`inf`).
   - **Cliente**: Garante que o campo `cliente_id` não esteja vazio.
   - **Tipo de Transação**: Valida se o tipo pertence exclusivamente a `credito` ou `debito`.
   - Registros que falham em qualquer critério são descartados com segurança (retornam `None`).

3. **Resumo de Qualidade dos Dados (`exibir_resumo_validacao`)**:
   - Contabiliza e exibe o total de registros lidos, a quantidade de linhas válidas e o total de linhas descartadas durante a etapa de limpeza.

4. **Consolidação e Relatório Mensal (`gerar_relatorio`)**:
   - Agrupa as transações válidas por mês (`YYYY-MM`) em ordem cronológica e calcula:
     - Quantidade total de transações (`qtde_transacoes`)
     - Total de créditos (`credito`)
     - Total de débitos (`debito`)
     - Saldo líquido do mês (`credito - debito`)
     - Ticket médio das transações (`media_transacoes`)
     - Maior e menor valor movimentado no período (`transacao_maior_valor` e `transacao_menor_valor`)

5. **Detecção de Transações Suspeitas (`identifica_transacao_suspeita`)**:
   - Filtra e sinaliza automaticamente transações cujo valor ultrapasse o limite de **R$ 10.000,00**, facilitando a auditoria de movimentações atípicas.

6. **Exibição e Exportação (`exibir_relatorio`, `salvar_json`)**:
   - Imprime no terminal o relatório mensal formatado no padrão monetário brasileiro (`R$ X,XX`) e a listagem de transações suspeitas.
   - Exporta os indicadores mensais consolidados para o arquivo `relatorio.json`.

---

## 🛠️ Tecnologias Utilizadas

- **Python 3.13+**
- **Jupyter Notebook**
- Bibliotecas padrão do Python (sem necessidade de dependências externas):
  - `csv` (leitura de arquivos tabulares)
  - `json` (exportação de dados estruturados)
  - `math` (validação numérica contra `NaN` e `inf`)
  - `datetime` (validação e formatação de datas)

---

## 📁 Estrutura do Repositório

```text
├── desafio-final.ipynb   # Notebook principal com todo o fluxo de processamento e análise
├── transacoes.csv        # Base de dados de entrada (contendo registros válidos e inconsistências para teste)
├── relatorio.json        # Arquivo gerado automaticamente com as métricas mensais consolidadas
└── README.md             # Documentação do projeto
```

---

## 🚀 Como Executar o Projeto

1. **Clone este repositório**:
   ```bash
   git clone <URL_DO_SEU_REPOSITORIO>
   cd <NOME_DA_PASTA>
   ```

2. **Abra o arquivo `desafio-final.ipynb`**:
   - Você pode utilizar o **VS Code** (com a extensão *Jupyter*), **Jupyter Lab** ou **Jupyter Notebook**.

3. **Execute as células sequencialmente**:
   - Certifique-se de que o arquivo `transacoes.csv` esteja no mesmo diretório do notebook.
   - Ao executar a última célula (`main()`), o resumo da validação e o relatório serão exibidos na saída padrão e o arquivo `relatorio.json` será atualizado.

---

## 📈 Exemplo de Saída

```text
Resumo da limpeza:

    - Total de linhas: 120
    - Linhas válidas: 105
    - Linhas inválidas: 15
    
===== RELATÓRIO MENSAL =====
Mês: 2026-01
        Transações: 21
        Total crédito: R$ 20984,09
        Total débito:  R$ 9358,94
        Saldo:         R$ 11625,15
        Média:         R$ 1444,91
        Maior valor:   R$ 5835,15
        Menor valor:   R$ 142,20
...
===== TRANSAÇÕES SUSPEITAS =====
ID: 17 | Cliente: CLI012 | Data: 2026-05 | Valor: R$ 20426,91
ID: 68 | Cliente: CLI001 | Data: 2026-05 | Valor: R$ 19506,81
ID: 97 | Cliente: CLI001 | Data: 2026-05 | Valor: R$ 19150,28
```

---

## 📝 Contexto Acadêmico

Projeto desenvolvido como parte da avaliação do módulo de **Análise de Dados e Inteligência de Negócios com IA** da **Rocketseat**.
