#  Processo de ETL de uma Base de Clientes

Projeto desenvolvido durante o curso 
Pentaho for ETL & Data Integration Masterclass 2022 - PDI 9
<br>[Link do Certificado](https://www.udemy.com/certificate/UC-e3f6be67-da69-4e6a-a610-b25b4cdb2c1b/)


## Objetivos
- Estruturar um proccesso de ETL do zero usando Pentaho e fontes de dados em diversos formatos;
- Fazer a limpeza dos dados unificando a grafia de país, corrigindo erros de digitação nos estados e removendo caracteres especiais;
- Fazer a validação dos dados corrigindo problemas de valores negativos e erros de input no campo Age.


## Metodologia

![Overview da Transformação](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/Transformacao.PNG)
- Carregamento

  - Configurando o step "Data Grid" para o Input Manual. Na primeira aba se configura a meta data, usei o tipo string para todos os campos pois ele permite mais tranformações. Na aba data é onde são manualmente inseridos os dados.
  
     ![Input Manual com Data Grid](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/1_Manual_Input.PNG)
    
  - Para importação de Arquivos csv usei o step Text Input. A pasta que contém os arquivos dos dados possue um arquivo dummy e os arquivos que trazem registros dos estados possuem nomenclatura "CustomerData_West_" + o nome do estado ao qual pertencem. Utilizei então a expressão RegEx "CustomerData_West_.*" para que apenas os arquivos com registros fossem importados. Na aba "Content" inseri o tipo de separador e na aba "Fields" configurei a meta data de maneira semelhante aos passos anteriores.
  
     ![Text Input para carregar múltiplos arquivos csv](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/2_Text_Input.png)
    
  - No input de arquivo Excel configurei o "Spread sheet type (engine)" para "Excel 2007 XLSX (Apache POI)", na aba "Sheets" selecionei a planilha a ser importada do arquivo e em "Fields" também configurei a meta data.
  
     ![Input de Arquivo Excel](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/3_Excel_Input.png)
    
   - Para input de Arquivo Zip também usei o step de input de arquivo texto. Na aba "Content" selecionei o separador dos campos do arquivo e "Compression" Zip. Configurei também a meta data na aba "Fields".
    
     ![Input de Arquivo Zip](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/4_Zip_File_Input.png)
    
   - Utilizei o step "sort rows" para ordenar os registros de cada arquivo pelo campo "Customer ID" para prepará-los para o próximo passo
   
   - Para unificar a base de dados usei o step "Sorted Merge" utilizando como parâmetro o campo "Customer ID"
    
    
- Limpeza
  - A Base unificada possui os registros US, USA, United States e United States of America no campo Country. Utilizei o step "Value Mapper" para transformar os registros do campo "Country" para United States.
   
    ![Unificando a grafia de United States com Value Mapper](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/5_Value_Mapper.png)
    
  - Alguns registros do campo "City" possuem "#" precedendo o nome da cidade. Utilizei o step "Replace in String" e configurei para que ele procurasse por "#" no campo "City" e o removesse deixando a opção "Replace with" vazia.
    
    ![Removendo caracteres especiais com Replace in String](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/6_Replace_in_String.png)
    
  - Alguns registros possuem erros de digitação como "Twxas", "Cakifornia", entre outros. Para corrigir esse tipo de problemas utilizei o step "Fussy Match" e o interliguei com um arquivo "Lookup for States" com a grafia correta dos estados. Configurei então o "Lookup Step" e e "Lookup Field" para "State Name" o campo a ser comparado da base principal ou "Main stream field" para "State". Este step possue diversos algoritmos de comparação, no caso do algoritmon de "Levenshtein" o valor da distancia equivale deleções, insersões ou substituições para que o alvo se transforme no valor de consulta. Por exemplo, texas está a uma distância de 2 de Texas. Observando os problemas da base escolhi o "Maximal value" de 2.
    
    ![Corrigindo erros de digitação com Fuzzy match](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/7_Fuzzy_Match.png)
   
  - Para substituir a coluna "State" pela coluna "State Name" com os registro de estado corrigidos utilizei o step "Select values"
    
    ![Substituindo a coluna State com os valores corrigidos](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/8_Seleciona_campo_State_corrigido.png)
    
    
- Validação e Debugging
  - Para converter campo Age para Integer utilizei o step select values, mas encontrei 2 problemas a serem tratados: registros de idade com a letra "o" ao invés de 0 como 6o e números negativos.
    
    ![Convertendo campo Age para Integer](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/9_Seleciona_idade_como_integer.png)
    
  - Para tratar os dados com erros de iput como "o" ao invés de "0" utilizei o step "Replace in string". Com os erros corrigidos passei novamente os dados pelo step de "Select values" para conversão do campo Age para Integer.
  
    ![Corrige "o" para "0"](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/10_Replace_in_String.png)
  
  - Para tratar os números negativos usei o step "Filter rows" e encaminhei os valores menores que 0 para tratamento separadamente
    
    ![Filtra valores negativos](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/11_Filter_Rows.png)
    
  - Fiz o tratamento dos registros de idade negativos através do step "Calculator". Utilizei o cálculo "Absolute value ABS".
   
    ![Calcula o valor absoluto](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/12_Calcula_Valor_Absoluto.png)
   
  - Substitui então a coluna "Age" pela coluna com os registros tratados do passo anterior com o step "Select values"
  
  - Uni os 3 segmentos dos dados com o step "Sorted merge"
  
  - Criei o output dos dados em Arquivo Excel como step "Microsoft Excel Output"
      ![Excel com os dados processados](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/13_Table_output.png)

## Resultados

![Base Unificada](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/Base_Unificada.png)
Após a importação múltiplos arquivos ".csv", arquivos de texto, input manual de dados com "Data Grid", arquivo excel e zip obtive a base unificada abaixo. Os dados retornaram alguns problemas que precisaram de limpeza.

![Base tratada](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/Base_Tratada.png)
Após tratamento o resultado foi um arquivo ".xls" tabela dimensão com dados de clientes. As inconsistências nos campos "Age", "Country", "City" e "State" foram corrigidas com uso de diversos steps do Pentaho Data Integration ou PDI.
