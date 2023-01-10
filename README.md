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
![Input de Arquivo Excel](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/3_Excel_Input.png)
![Input de Arquivo Zip](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/4_Zip_File_Input.png)
- Limpeza
![Unificando a grafia de United States com Value Mapper](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/5_Value_Mapper.png)
![Removendo caracteres especiais com Replace in String](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/6_Replace_in_String.png)
![Corrigindo erros de digitação com Fuzzy match](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/7_Fuzzy_Match.png)
![Substituindo a coluna State com os valores corrigidos](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/8_Seleciona_campo_State_corrigido.png)
- Validação e Debugging
![Convertendo campo Age para Integer](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/9_Seleciona_idade_como_integer.png)
![Corrige "o" para "0"](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/10_Replace_in_String.png)
![Filtra valores negativos](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/11_Filter_Rows.png)
![Calcula o valor absoluto](https://github.com/Anacaloi/ETL-PDI-Customer/blob/main/img/12_Calcula_Valor_Absoluto.png)


## Resultados
