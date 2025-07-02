# tcc-analise-geoespacial
ANÁLISE GEOESPACIAL DAS INSATISFAÇÕES DOS CONSUMIDORES DA CASAN NOS BAIRROS CAMPECHE E RIO TAVARES - FLORIANÓPOLIS

Entendido\! Com base na sua explicação detalhada do notebook e na sua intenção de subir o código para o GitHub, vou criar um `README.md` completo e "lindão" que você pode usar. Ele incluirá uma introdução, como rodar o código, uma descrição detalhada de cada célula (como você fez), e as conclusões do processo.

-----

```markdown
# Análise e Agrupamento de Motivos de Insatisfação - CASAN (TCC)

Este repositório contém o script Python desenvolvido no Google Colab para o pré-processamento, análise e agrupamento de dados de manifestações de insatisfação da CASAN, especificamente para os bairros Campeche e Rio Tavares, em Florianópolis/SC (período de 2021-2022). O objetivo central é transformar dados textuais brutos em categorias significativas para análise espacial no contexto de um Trabalho de Conclusão de Curso (TCC).

O notebook `Processamento de dados planilha CASAN - Colab.ipynb` (ou o nome do seu arquivo .ipynb) detalha um processo iterativo de tratamento de texto, desde a normalização básica até métodos de filtragem avançada para identificar os temas mais relevantes das reclamações.

## 🚀 Como Utilizar o Código

Para rodar este notebook, você pode seguir os passos abaixo:

1.  **Acesse o Google Colab:** Abra o Google Colab em seu navegador (requer uma conta Google).
2.  **Carregue o Notebook:**
    * Vá em `Arquivo > Fazer upload do notebook`.
    * Selecione o arquivo `.ipynb` deste repositório.
3.  **Faça o Upload do Dados:** Na primeira célula do notebook, você será solicitado a fazer o upload do arquivo Excel com os dados brutos (`Teste 2 Python.xlsx`).
4.  **Execute as Células:** Execute as células do notebook sequencialmente. Algumas células exportarão arquivos Excel para o ambiente do Colab, e a célula final permitirá o download do resultado processado para o seu computador.

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
