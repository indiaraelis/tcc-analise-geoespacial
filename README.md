# tcc-analise-geoespacial

Este README está disponível em [Português](#portugues).

---

## GEOSPATIAL ANALYSIS OF CASAN CONSUMER DISSATISFACTIONS IN CAMPECHE AND RIO TAVARES NEIGHBORHOODS - FLORIANÓPOLIS

This repository contains the Python script developed in Google Colab for the pre-processing, analysis, and clustering of CASAN consumer dissatisfaction data, specifically for the Campeche and Rio Tavares neighborhoods in Florianópolis/SC (period 2021-2022). The central objective is to transform raw textual data into meaningful categories for spatial analysis within the context of a Course Completion Work (TCC).

The `Processamento de dados planilha CASAN - Colab.ipynb` notebook (or the name of your .ipynb file) details an iterative text processing workflow, from basic normalization to advanced filtering methods to identify the most relevant themes of complaints.

---

## 🚀 How to Use the Code

To run this notebook, you can follow the steps below:

1.  **Access Google Colab:** Open Google Colab in your browser (requires a Google account).
2.  **Upload the Notebook:**
    * Go to `File > Upload notebook`.
    * Select the `.ipynb` file from this repository.
3.  **Upload the Data:** In the first cell of the notebook, you will be prompted to upload the Excel file with the raw data (`Teste 2 Python.xlsx`).
4.  **Execute Cells:** Run the notebook cells sequentially. Some cells will export Excel files to the Colab environment, and the final cell will allow you to download the processed result to your computer.

---

## 📝 Detailed Notebook Description

The `Processamento de dados planilha CASAN - Colab.ipynb` notebook (or the name of your file) is structured into several cells, each responsible for a specific stage of the textual analysis process. Below, we detail the functionality of each one:

### **Cell 1: File Loading and Initial Visualization**

This cell is the starting point of the script, responsible for preparing the environment and loading the raw data.

* **Imports:** Imports the necessary libraries:
    * `google.colab.files`: To allow uploading files directly into the Colab environment.
    * `pandas`: Essential for data manipulation and analysis in DataFrame format.
* **File Loading:** Uses `files.upload()` to open an interactive interface where the user can select and send the Excel file (`Teste 2 Python.xlsx`) to the Colab execution environment.
* **Excel Reading:** The loaded file is read by `pd.read_excel(file_path)` and transformed into a pandas DataFrame (`df`), which will be the main data structure for future manipulations.
* **Initial Visualization:** `print(df.head())` displays the first 5 rows of the DataFrame, providing a preview of the data structure, including columns such as 'Logradouro', 'Distrito Operacional', 'Solicitação(data)', and, crucially, 'Motivo'.

### **Cell 2: Normalization, Counting, and Basic Grouping of Reasons**

This cell performs the first level of pre-processing and an initial attempt to categorize the reasons for dissatisfaction.

* **Step 2: Reason Normalization:** The 'Motivo' column is standardized using `.str.lower().str.strip()`. This converts all text to lowercase and removes extra whitespace, ensuring that "Reason A" and "reason a " are treated as the same item.
* **Step 3: Frequency Count:** `df['Motivo'].value_counts()` calculates the frequency of each unique normalized reason, revealing the most common themes in the database.
* **Step 4: Identification of Top 10 Reasons:** `motivos_contagem.nlargest(10).index.tolist()` extracts the 10 most frequent reasons for an initial analysis.
* **Step 5: Creation of 'Group' Column:** A new column `df['Grupo'] = None` is added to the DataFrame to store the grouping categories.
* **Step 6: Basic Grouping:** A loop iterates over the 10 most frequent reasons. If a reason in a row contains (case-insensitively) one of these top 10, the 'Grupo' column for that row is filled with the corresponding reason.
* **Step 7: Display Results:** The 10 most frequent reasons and their counts are printed, providing an initial overview of the most recurring issues (e.g., "customer complains about lack of service provided...", "customer complains about service delay...").
* **Step 8: Saving Results:** The processed DataFrame, including the new 'Grupo' column, is exported to an Excel file (`resultado_agrupado.xlsx`) for future consultation or use.

### **Cell 3: Download of Processed File**

A simple but essential cell for the workflow:

* **Download:** `files.download(output_file_path)` initiates the download of the `resultado_agrupado.xlsx` file (generated in the previous cell) directly to the user's local system.

### **Cell 4: Improving Filters - Word Count (Without Stopwords)**

This cell marks the beginning of a more granular approach, focusing on individual words to identify keywords, but without removing common Portuguese words.

* **Imports:** `pandas`, `collections.Counter` (for efficient counts), and `re` (for regular expressions, useful in word extraction) are imported.
* **Data Reading:** The original Excel file is loaded again to ensure a "clean state" for the new analysis.
* **Step 1: Normalization:** The 'Motivo' column is normalized (lowercase, no extra spaces) again.
* **Step 2: Word Extraction:** Uses regular expressions (`re.findall(r'\b\w+\b', motivo)`) to extract all words from each reason and groups them into a single list.
* **Step 3: Word Frequency Count:** `Counter(todas_as_palavras)` calculates the frequency of each extracted word.
* **Step 4: Top 10 Words:** The 10 most frequent words are identified and displayed. The initial output usually reveals many "stopwords" (e.g., 'cliente', 'de', 'do', 'que', 'protocolo'), which, despite being frequent, do not carry much thematic meaning.

### **Cell 5: Word Count with Stopwords**

Improving the analysis from the previous cell, this step introduces the removal of stopwords to focus on more relevant terms.

* **Stopword Definition:** A `stopwords` list is manually created, containing common Portuguese words that are considered noise for thematic analysis (e.g., 'de', 'do', 'da', 'que', 'a', 'e', 'o', 'com', 'para', 'não', 'cliente', 'reclama', 'protocolo').
* **Step 2: Word Filtering:** Words are extracted from the 'Motivo' column, but are now filtered to exclude any term present in the `stopwords` list.
* **Steps 3 and 4:** Frequency counting and identification of the 10 most frequent words are performed again, but with the cleaned word list.
* **Display Results:** The output now shows more significant and informative words, such as 'informa', 'servico', 'solicita', 'demora', 'fatura', which are better candidates for grouping.

### **Cell 6: Grouping by Relevant Terms (Variant 1)**

Based on the identified relevant terms, this cell performs a more specific grouping of reasons.

* **Selection of Terms for Grouping:** A `termos_agrupamento` list is manually defined (e.g., `['fatura', 'informa', 'serviço', 'demora']`), using insights from previous cells about the most significant words.
* **Grouping:** A loop iterates over the reasons. If a reason contains any of the `termos_agrupamento`, it is categorized into that group.
* **Count by Group:** The script displays the count of manifestations for each created group, such as 'demora', 'informa', 'serviço', and 'fatura'.

### **Cell 7: Grouping by Relevant Terms (Variant 2 - Excluding Relevant Ones)**

This cell further refines keyword identification, looking for new important terms by excluding those already considered "relevant" from the list of the 10 most frequent.

* **Definition of Relevant and Excluded Words:** An initial set of `palavras_relevantes` (relevant words) is defined (e.g., `['fatura', 'informa', 'deslocamento', 'demora', 'cavalete']`). These are the words that *definitely* form a group. An `palavras_excluidas` (excluded words) list is created from them.
* **Advanced Filtering:** Words are extracted from reasons, but the list of the 10 most frequent words is generated *excluding* those already relevant, allowing other important terms to "rise" in the frequency list.
* **Grouping:** Reasons are grouped based on the predefined `palavras_relevantes`. The goal is to see how these groups are distributed.

### **Cell 8: Grouping by Filtered Terms (In-depth)**

This cell expands the list of words to be excluded (stopwords) more aggressively to identify truly distinctive terms for grouping.

* **Expansion of the Exclusion List (`palavras_excluir`):** A much more comprehensive `palavras_excluir` list is created, including generic or redundant terms (e.g., 'atender', 'dizer', 'respondido', 'contato', 'receber', 'pode', 'recebido', 'esperar', 'em relação', 'avisar', etc.). The process is iterative, adding terms as they are identified as non-informative.
* **Refined Extraction and Filtering:** Words are extracted from reasons but filtered by this expanded exclusion list, as well as numbers and short words (< 3 characters).
* **Counting and Top 10:** The most frequent words after this rigorous filter are displayed. Terms like 'agilidade', 'execução', 'leitura', 'prazo', 'faturas' begin to stand out, indicating a more effective filter.
* **Dynamic Grouping:** For each word that is *not* in the exclusion list and is found in a reason, it is assigned as 'Grupo'. This aims to test a more "organic" grouping based on the most relevant remaining terms.

### **Cell 9 (and subsequent): Further Iterations and Refinements in the Exclusion List**

The subsequent cells from page 6 of the PDF (`Processamento de dados planilha CASAN - Colab.pdf`) are repetitions of the logic in Cell 8, with minor modifications to the `palavras_excluir` list.

* **Continued Expansion of `palavras_excluir`:** In each iteration, more generic or less informative words, observed in previous *outputs*, are added to the `palavras_excluir` list (e.g., 'informa', 'serviço', 'solicita', 'foi', 'referente', 'está' in one step; 'agilidade', 'execução', 'sua' in another).
* **Identical Processing:** The steps of normalization, word extraction and filtering, frequency counting, display of the most frequent, grouping, and counting of groups are repeated with the updated `palavras_excluir` list.
* **Objective:** This iterative process aims to continuously refine the keyword list used to group reasons, seeking to isolate the most discriminating and significant terms for CASAN's analysis. The outputs (`Palavras mais relevantes mencionadas em MOTIVO (excluindo palavras irrelevantes):` and `Contagem de manifestações por grupo:`) change with each iteration, reflecting this filter improvement. Terms such as 'demora', 'fatura', 'deslocamento', 'cavalete', 'leitura', 'prazo', 'faturas', 'reclamação', 'atraso', 'ainda' emerge as relevant after more rigorous filters.

### **Final Cell: Optimized Grouping with Predefined Groups and Export**

This section concludes the grouping process, using a predefined and optimized list of keywords to categorize reasons for dissatisfaction and export the results.

* **Definition of Words for Optimized Grouping:** `palavras_grupos = ['demora', 'fatura', 'deslocamento', 'cavalete', 'agilidade', 'execução', 'leitura', 'prazo', 'faturas', 'vazam', 'atraso']` defines the keywords that will be used to categorize the reasons. These are the words that, after refinement iterations, were considered most important and relevant for creating the final groups.
* **Creation of 'Group' Column:** The `df['Grupo']` column is initialized with null values.
* **Final Grouping of Reasons:** A loop iterates over each `palavra` defined in `palavras_grupos`. If a `palavra` is found in the text of a 'Motivo', that `palavra` is assigned to the 'Grupo' column for that row, categorizing it.
* **Displaying Group Count:** `contagem_grupos = df['Grupo'].value_counts()` calculates and prints the count of manifestations for each defined group, showing the final distribution of complaints by category.
* **Displaying Grouped Reasons:** `motivos_agrupados = df[df['Grupo'].notnull()][['Motivo', 'Grupo']]` filters and displays a subset of the DataFrame, showing only the manifestations that have been grouped, along with their respective groups.
* **Exporting Results:** The final DataFrame, with categorized reasons, is saved to a new Excel file (`motivos_agrupados.xlsx`).
* **Download:** The `motivos_agrupados.xlsx` Excel file is made available for download, allowing the results to be used in other tools (e.g., GIS for georeferencing and spatial analysis).

### **Final Considerations on the Grouping Process**

The cells following the final section in the PDF (`Processamento de dados planilha CASAN - Colab.pdf`, from page 10) demonstrate attempts to implement a more complex grouping, where a reason could belong to multiple groups (using lists in the 'Grupo' column). However, these approaches resulted in `TypeError` due to data type incompatibilities. The last section present in the PDF is an example of how to initialize a DataFrame with empty lists in the 'Grupo' column, serving as a demonstration of the theoretical approach for multiple groupings. The final method adopted for the TCC focused on assigning a single main group per reason, based on the detection of the most relevant keywords.

---

## 🔒 License

This open-source project is licensed under the **MIT License**. This means you are free to copy, modify, distribute, and use this code in private or commercial projects, provided that the copyright notice and license text are maintained.

For more details, please refer to the [LICENSE](https://www.google.com/search?q=LICENSE) file in this repository.

---

## 📊 Data Used

The consumer dissatisfaction data used in this project was provided by Companhia Catarinense de Águas e Saneamento (CASAN) specifically for academic research purposes, as part of the author's Course Completion Work (TCC). CASAN authorized the use and eventual public disclosure of this data in the context of this work.

It is important to note that the **MIT License** applied to this repository **covers only the source code** developed for geospatial analysis. The use of raw CASAN data must comply with the terms of the original authorization granted by them for this academic project.

---

<a name="portugues"></a>


# tcc-analise-geoespacial

Este README está disponível em [Inglês](#tcc-analise-geoespacial).

---

## ANÁLISE GEOESPACIAL DAS INSATISFAÇÕES DOS CONSUMIDORES DA CASAN NOS BAIRROS CAMPECHE E RIO TAVARES - FLORIANÓPOLIS

Este repositório contém o script Python desenvolvido no Google Colab para o pré-processamento, análise e agrupamento de dados de manifestações de insatisfação da CASAN, especificamente para os bairros Campeche e Rio Tavares, em Florianópolis/SC (período de 2021-2022). O objetivo central é transformar dados textuais brutos em categorias significativas para análise espacial no contexto de um Trabalho de Conclusão de Curso (TCC).

O notebook `Processamento de dados planilha CASAN - Colab.ipynb` (ou o nome do seu arquivo .ipynb) detalha um processo iterativo de tratamento de texto, desde a normalização básica até métodos de filtragem avançada para identificar os temas mais relevantes das reclamações.

---

## 🚀 Como Utilizar o Código

Para rodar este notebook, você pode seguir os passos abaixo:

1.  **Acesse o Google Colab:** Abra o Google Colab em seu navegador (requer uma conta Google).
2.  **Carregue o Notebook:**
    * Vá em `Arquivo > Fazer upload do notebook`.
    * Selecione o arquivo `.ipynb` deste repositório.
3.  **Faça o Upload dos Dados:** Na primeira célula do notebook, você será solicitado a fazer o upload do arquivo Excel com os dados brutos (`Teste 2 Python.xlsx`).
4.  **Execute as Células:** Execute as células do notebook sequencialmente. Algumas células exportarão arquivos Excel para o ambiente do Colab, e a célula final permitirá o download do resultado processado para o seu computador.

---

## 📝 Descrição Detalhada do Notebook

O notebook `Processamento de dados planilha CASAN - Colab.ipynb` (ou o nome do seu arquivo) é estruturado em várias células, cada uma responsável por uma etapa específica do processo de análise textual. Abaixo, detalhamos a funcionalidade de cada uma:

### **Célula 1: Carregamento do Arquivo e Visualização Inicial**

Esta célula é o ponto de partida do script, responsável por preparar o ambiente e carregar os dados brutos.

* **Importações:** Importa as bibliotecas necessárias:
    * `google.colab.files`: Para permitir o upload de arquivos diretamente no ambiente Colab.
    * `pandas`: Fundamental para manipulação e análise de dados em formato de DataFrame.
* **Carregamento do arquivo:** Utiliza `files.upload()` para abrir uma interface interativa, onde o usuário pode selecionar e enviar o arquivo Excel (`Teste 2 Python.xlsx`) para o ambiente de execução do Colab.
* **Leitura do Excel:** O arquivo carregado é lido por `pd.read_excel(file_path)` e transformado em um DataFrame do pandas (`df`), que será a estrutura de dados principal para as manipulações futuras.
* **Visualização inicial:** `print(df.head())` exibe as primeiras 5 linhas do DataFrame, oferecendo uma prévia da estrutura dos dados, incluindo colunas como 'Logradouro', 'Distrito Operacional', 'Solicitação(data)' e, crucialmente, 'Motivo'.

### **Célula 2: Normalização, Contagem e Agrupamento Básico de Motivos**

Esta célula realiza o primeiro nível de pré-processamento e uma tentativa inicial de categorização dos motivos de insatisfação.

* **Passo 2: Normalização dos Motivos:** A coluna 'Motivo' é padronizada utilizando `.str.lower().str.strip()`. Isso converte todo o texto para minúsculas e remove espaços em branco extras, garantindo que "Motivo A" e "motivo a " sejam tratados como o mesmo item.
* **Passo 3: Contagem de Frequência:** `df['Motivo'].value_counts()` calcula a frequência de cada motivo único normalizado, revelando os temas mais comuns na base de dados.
* **Passo 4: Identificação dos Top 10 Motivos:** `motivos_contagem.nlargest(10).index.tolist()` extrai os 10 motivos mais frequentes para uma análise inicial.
* **Passo 5: Criação da Coluna 'Grupo':** Uma nova coluna `df['Grupo'] = None` é adicionada ao DataFrame para armazenar as categorias de agrupamento.
* **Passo 6: Agrupamento Básico:** Um loop itera sobre os 10 motivos mais frequentes. Se um motivo de uma linha contiver (de forma insensível a maiúsculas/minúsculas) um desses top 10, a coluna 'Grupo' dessa linha é preenchida com o motivo correspondente.
* **Passo 7: Exibição de Resultados:** Os 10 motivos mais frequentes e suas contagens são impressos, fornecendo um panorama inicial das questões mais recorrentes (ex: "cliente reclama da falta de serviço prestado...", "cliente reclama na demora do serviço...").
* **Passo 8: Salvamento do Resultado:** O DataFrame processado, incluindo a nova coluna 'Grupo', é exportado para um arquivo Excel (`resultado_agrupado.xlsx`) para consulta ou uso posterior.

### **Célula 3: Download do Arquivo Processado**

Uma célula simples, mas essencial para a workflow:

* **Download:** `files.download(output_file_path)` inicia o download do arquivo `resultado_agrupado.xlsx` (gerado na célula anterior) diretamente para o sistema local do usuário.

### **Célula 4: Melhorando os Filtros - Contagem de Palavras (Sem Stopwords)**

Esta célula marca o início de uma abordagem mais granular, focando em palavras individuais para identificar termos-chave, mas sem remover palavras comuns da língua portuguesa.

* **Importações:** `pandas`, `collections.Counter` (para contagens eficientes) e `re` (para expressões regulares, útil na extração de palavras) são importadas.
* **Leitura de Dados:** O arquivo Excel original é carregado novamente para garantir um "estado limpo" para a nova análise.
* **Passo 1: Normalização:** A coluna 'Motivo' é normalizada (minúsculas, sem espaços extras) novamente.
* **Passo 2: Extração de Palavras:** Utiliza expressões regulares (`re.findall(r'\b\w+\b', motivo)`) para extrair todas as palavras de cada motivo e as agrupa em uma lista única.
* **Passo 3: Contagem de Frequência de Palavras:** `Counter(todas_as_palavras)` calcula a frequência de cada palavra extraída.
* **Passo 4: Top 10 Palavras:** As 10 palavras mais frequentes são identificadas e exibidas. O output inicial geralmente revela muitas "stopwords" (ex: 'cliente', 'de', 'do', 'que', 'protocolo'), que, apesar de frequentes, não carregam muito significado temático.

### **Célula 5: Contagem de Palavras com Stopwords**

Aprimorando a análise da célula anterior, esta etapa introduz a remoção de stopwords para focar em termos mais relevantes.

* **Definição de Stopwords:** Uma lista `stopwords` é criada manualmente, contendo palavras comuns em português que são consideradas ruído para a análise temática (ex: 'de', 'do', 'da', 'que', 'a', 'e', 'o', 'com', 'para', 'não', 'cliente', 'reclama', 'protocolo').
* **Passo 2: Filtragem de Palavras:** As palavras são extraídas da coluna 'Motivo', mas agora são filtradas para excluir qualquer termo presente na lista `stopwords`.
* **Passos 3 e 4:** A contagem de frequência e a identificação das 10 palavras mais frequentes são realizadas novamente, mas com a lista de palavras já limpa.
* **Exibição de Resultados:** O output agora mostra palavras mais significativas e informativas, como 'informa', 'servico', 'solicita', 'demora', 'fatura', que são melhores candidatas para agrupamento.

### **Célula 6: Agrupamento por Termos Relevantes (Variante 1)**

Com base nos termos relevantes identificados, esta célula realiza um agrupamento mais específico dos motivos.

* **Seleção de Termos para Agrupamento:** Uma lista `termos_agrupamento` é definida manualmente (ex: `['fatura', 'informa', 'serviço', 'demora']`), utilizando os insights das células anteriores sobre as palavras mais significativas.
* **Agrupamento:** Um loop itera sobre os motivos. Se um motivo contém algum dos `termos_agrupamento`, ele é categorizado nesse grupo.
* **Contagem por Grupo:** O script exibe a contagem de manifestações por cada grupo criado, como 'demora', 'informa', 'serviço' e 'fatura'.

### **Célula 7: Agrupamento por Termos Relevantes (Variante 2 - Excluindo Relevantes)**

Esta célula refina ainda mais a identificação de palavras-chave, procurando novos termos importantes ao excluir os já considerados "relevantes" da lista das 10 mais frequentes.

* **Definição de Palavras Relevantes e Excluídas:** Um conjunto inicial de `palavras_relevantes` é definido (ex: `['fatura', 'informa', 'deslocamento', 'demora', 'cavalete']`). Essas são as palavras que *definitivamente* formam um grupo. Uma lista `palavras_excluidas` é criada a partir delas.
* **Filtragem Avançada:** As palavras são extraídas dos motivos, mas a lista das 10 palavras mais frequentes é gerada *excluindo* as já relevantes, permitindo que outros termos importantes "subam" na lista de frequência.
* **Agrupamento:** Os motivos são agrupados com base nas `palavras_relevantes` predefinidas. O objetivo é ver como esses grupos se distribuem.

### **Célula 8: Agrupamento por Termos Filtrados (Aprofundado)**

Esta célula expande a lista de palavras a serem excluídas (stopwords) de forma mais agressiva para identificar termos verdadeiramente distintivos para o agrupamento.

* **Expansão da Lista de Exclusão (`palavras_excluir`):** Uma lista `palavras_excluir` muito mais abrangente é criada, incluindo termos genéricos ou redundantes (ex: 'atender', 'dizer', 'respondido', 'contato', 'receber', 'pode', 'recebido', 'esperar', 'em relação', 'avisar', etc.). O processo é iterativo, adicionando termos conforme eles são identificados como não-informativos.
* **Extração e Filtragem Refinada:** As palavras são extraídas dos motivos, mas filtradas por esta lista expandida de excluídos, além de números e palavras curtas (< 3 caracteres).
* **Contagem e Top 10:** As palavras mais frequentes após este filtro rigoroso são exibidas. Termos como 'agilidade', 'execução', 'leitura', 'prazo', 'faturas' começam a se destacar, indicando um filtro mais eficaz.
* **Agrupamento Dinâmico:** Para cada palavra que *não* está na lista de excluídos e é encontrada em um motivo, ela é atribuída como 'Grupo'. Isso visa testar um agrupamento mais "orgânico" baseado nos termos mais relevantes que restaram.

### **Célula 9 (e seguintes): Iterações e Refinamentos Adicionais na Lista de Exclusão**

As células subsequentes a partir da página 6 do PDF (`Processamento de dados planilha CASAN - Colab.pdf`) são repetições da lógica da Célula 8, com pequenas modificações na lista de `palavras_excluir`.

* **Expansão Continuada da `palavras_excluir`:** Em cada iteração, mais palavras genéricas ou menos informativas, observadas nos *outputs* anteriores, são adicionadas à lista `palavras_excluir` (ex: 'informa', 'serviço', 'solicita', 'foi', 'referente', 'está' em uma etapa; 'agilidade', 'execução', 'sua' em outra).
* **Processamento Idêntico:** Os passos de normalização, extração e filtragem de palavras, contagem de frequência, exibição dos mais frequentes, agrupamento e contagem dos grupos são repetidos com a lista `palavras_excluir` atualizada.
* **Objetivo:** Este processo iterativo visa refinar continuamente a lista de palavras-chave usadas para agrupar os motivos, buscando isolar os termos mais discriminatórios e significativos para a análise da CASAN. As saídas (`Palavras mais relevantes mencionadas em MOTIVO (excluindo palavras irrelevantes):` e `Contagem de manifestações por grupo:`) mudam a cada iteração, refletindo essa melhoria no filtro. Termos como 'demora', 'fatura', 'deslocamento', 'cavalete', 'leitura', 'prazo', 'faturas', 'reclamação', 'atraso', 'ainda' emergem como relevantes após os filtros mais rigorosos.

### **Célula Final: Agrupamento Otimizado com Grupos Predefinidos e Exportação**

Esta seção conclui o processo de agrupamento, utilizando uma lista predefinida e otimizada de palavras-chave para categorizar os motivos de insatisfação e exportar os resultados.

* **Definição de Palavras para Agrupamento Otimizado:** `palavras_grupos = ['demora', 'fatura', 'deslocamento', 'cavalete', 'agilidade', 'execução', 'leitura', 'prazo', 'faturas', 'vazam', 'atraso']` define as palavras-chave que serão usadas para categorizar os motivos. Estas são as palavras que, após as iterações de refinamento, foram consideradas mais importantes e relevantes para a criação dos grupos finais.
* **Criação da Coluna 'Grupo':** A coluna `df['Grupo']` é inicializada com valores nulos.
* **Agrupamento Final dos Motivos:** Um loop itera sobre cada `palavra` definida em `palavras_grupos`. Se uma `palavra` for encontrada no texto de um 'Motivo', essa `palavra` é atribuída à coluna 'Grupo' para aquela linha, categorizando-a.
* **Exibição da Contagem por Grupo:** `contagem_grupos = df['Grupo'].value_counts()` calcula e imprime a contagem de manifestações para cada um dos grupos definidos, mostrando a distribuição final das reclamações por categoria.
* **Exibição de Motivos Agrupados:** `motivos_agrupados = df[df['Grupo'].notnull()][['Motivo', 'Grupo']]` filtra e exibe um subconjunto do DataFrame, mostrando apenas as manifestações que foram agrupadas, juntamente com seus respectivos grupos.
* **Exportação dos Resultados:** O DataFrame final, com os motivos categorizados, é salvo em um novo arquivo Excel (`motivos_agrupados.xlsx`).
* **Download:** O arquivo Excel `motivos_agrupados.xlsx` é disponibilizado para download, permitindo que os resultados sejam utilizados em outras ferramentas (ex: SIG para georreferenciamento e análise espacial).

### **Considerações Finais sobre o Processo de Agrupamento**

As células posteriores à seção final no PDF (`Processamento de dados planilha CASAN - Colab.pdf`, a partir da página 10) demonstram tentativas de implementar um agrupamento mais complexo, onde um motivo poderia pertencer a múltiplos grupos (utilizando listas na coluna 'Grupo'). No entanto, essas abordagens resultaram em `TypeError` devido a incompatibilidades de tipo de dados. A última seção presente no PDF é um exemplo de como inicializar um DataFrame com listas vazias na coluna 'Grupo', servindo como uma demonstração da abordagem teórica para múltiplos agrupamentos. O método final adotado para o TCC focou na atribuição de um único grupo principal por motivo, baseado na detecção das palavras-chave mais relevantes.

---

## 🔒 Licença

Este projeto de código-fonte está licenciado sob a **Licença MIT**. Isso significa que você pode copiar, modificar, distribuir e usar este código em projetos privados ou comerciais, desde que o aviso de copyright e o texto da licença sejam mantidos.

Para mais detalhes, consulte o arquivo [LICENSE](https://www.google.com/search?q=LICENSE) neste repositório.

---

## 📊 Dados Utilizados

Os dados de manifestações de insatisfação utilizados neste projeto foram fornecidos pela Companhia Catarinense de Águas e Saneamento (CASAN) especificamente para fins de pesquisa acadêmica, como parte do Trabalho de Conclusão de Curso (TCC) da autora. A CASAN autorizou o uso e a eventual publicidade desses dados no contexto deste trabalho.

É importante notar que a **Licença MIT** aplicada a este repositório **cobre apenas o código-fonte** desenvolvido para a análise geoespacial. O uso dos dados brutos da CASAN deve estar em conformidade com os termos da autorização original concedida por eles para este projeto acadêmico.
