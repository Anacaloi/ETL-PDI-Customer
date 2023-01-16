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

  - Configurando o step "Data Grid" para o Input Manual. Tipo string permite mais tranformações
  
     ![Input Manual com Data Grid](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/1_Manual_Input.PNG)
    
  - Para importação de Arquivos csv usei o step Text Input
  
     ![Text Input para carregar múltiplos arquivos csv](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/2_Text_Input.png)
    
  - Input de arquivo Excel
  
     ![Input de Arquivo Excel](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/3_Excel_Input.png)
    
   - Input de Arquivo Zip
    
     ![Input de Arquivo Zip](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/4_Zip_File_Input.png)
    
    
- Limpeza
  - A Base unificada possui os registros US, USA, United States e United States of America no campo Country.
   
    ![Unificando a grafia de United States com Value Mapper](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/5_Value_Mapper.png)
    
  - Alguns registros do campo "City" possuem "#" precedendo o nome da cidade.
    
    ![Removendo caracteres especiais com Replace in String](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/6_Replace_in_String.png)
    
  - Alguns registros possuem erros de digitação como "Twxas", "Cakifornia", entre outros.
    
    ![Corrigindo erros de digitação com Fuzzy match](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/7_Fuzzy_Match.png)
   
  - Para substituir a coluna "State" pela coluna "State Name" utilizei o step "Select values"
    
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
