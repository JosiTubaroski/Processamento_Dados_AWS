<div> 
<p><a href="https://github.com/JosiTubaroski/Introducao_Engenharia_Dados/blob/main/README.md">Introdução a Engenharia de Dados</a></p>
</div> 


# Análise de Dados de Log de Acesso em Tempo Real com AWS

### Cenário:

Uma empresa de mídia streaming quer analisar dados de log de acesso dos usuários para identificar padrões de comportamento em tempo real e gerar alertas para possíveis problemas, como interrupções no serviço ou picos de uso inesperados. Os logs incluem informações como:

- Endereço IP do usuário.
- Hora do acesso.
- Tipo de dispositivo.
- Erros no sistema.

Os dados são gerados em grandes volumes e precisam ser processados continuamente para fornecer insights em tempo real.


### Objetivo:

Construir um pipeline de processamento de dados em tempo real usando serviços AWS para capturar, processar e analisar os dados, além de armazenar para consultas futuras.

### Arquitetura e Ferramentas Utilizadas:

1. Amazon Kinesis Data Streams: Captura e ingere os dados de log em tempo real.
2. AWS Lambda: Processa os dados em tempo real conforme eles chegam.
3. Amazon S3: Armazena os dados brutos e processados para análises históricas.
4. Amazon Athena: Permite consultas ad-hoc nos dados armazenados no S3.
5. Amazon Elasticsearch Service (OpenSearch): Indexa os dados para busca rápida e visualização.
6. Amazon CloudWatch: Monitora a saúde do pipeline e gera alertas.
7. Amazon QuickSight: Cria painéis e relatórios com os dados processados.

### Passo a Passo:

#### 1. Ingestão de Dados com Kinesis Data Streams

- Os logs de acesso são enviados em tempo real para o Kinesis Data Streams a partir de aplicativos web, mobile e servidores.
- Cada evento (registro de log) é um JSON contendo informações como:

 <img src="https://github.com/JosiTubaroski/Processamento_Dados_AWS/blob/main/img/06_Json.png">

#### 2. Processamento em Tempo Real com AWS Lambda

- Uma função AWS Lambda consome os dados do Kinesis e executa transformações básicas, como:
  - Validar o formato do log.
  - Identificar erros críticos.
  - Enviar métricas específicas para o CloudWatch.

Exemplo de código Lambda em Python:

 <img src="https://github.com/JosiTubaroski/Processamento_Dados_AWS/blob/main/img/07_Lambda.png">

 #### Código Explicado

 1. Importação de Bibliotecas

<img src="https://github.com/JosiTubaroski/Processamento_Dados_AWS/blob/main/img/09_Codigo_Explicado_Biblioteca.png">

 - import json: Importa o módulo Python que lida com JSON (JavaScript Object Notation). Esse módulo é usado para converter strings JSON em objetos Python (dicionários, listas, etc.) e vice-versa.

2. Definição da Função lambda_handler

<img src="https://github.com/JosiTubaroski/Processamento_Dados_AWS/blob/main/img/10_def_lambda.png">

- def lambda_handler: Define a função principal que será executada quando a AWS Lambda for acionada.
- Parâmetros:
  -   event: Contém os dados do evento que acionaram a função. No contexto de AWS, isso pode ser mensagens do Amazon Kinesis, SQS, ou outro serviço.
  -   context: Contém informações sobre a execução da função Lambda, como tempo limite e memória disponível.
 
 3. Iteração sobre os Registros do Evento

<img src="https://github.com/JosiTubaroski/Processamento_Dados_AWS/blob/main/img/11_Registros_Eventos.png">

- event['Records']: Geralmente, para eventos vindos de fontes como Kinesis ou DynamoDB Streams, os dados do evento estão armazenados na lista Records.
- for record: Itera sobre cada registro na lista Records.

4. Processamento do Payload

<img src="https://github.com/JosiTubaroski/Processamento_Dados_AWS/blob/main/img/12_Playload.png">

- record['Data']: Cada registro contém os dados relevantes, geralmente em formato codificado ou como uma string JSON.
- json.loads(): Converte a string JSON em um objeto Python, normalmente um dicionário.

5. Verificação de Erros

<img src="https://github.com/JosiTubaroski/Processamento_Dados_AWS/blob/main/img/13_Ver_erro.png">
  
- payload.get('error_code'): Verifica se a chave error_code existe no dicionário payload. Caso exista, retorna o valor associado; caso contrário, retorna None.
- Ação em caso de erro:
  - Se a chave error_code existir, o código considera que ocorreu um erro crítico.
  - O erro é registrado no console com print.

6. Processamento de Dados

<img src="https://github.com/JosiTubaroski/Processamento_Dados_AWS/blob/main/img/14_Processamento_Payload.png">

- process_data(payload): Envia o payload para uma função chamada process_data, que não está definida neste trecho do código.
  - Essa função provavelmente realiza alguma transformação ou encaminhamento dos dados para outra etapa do pipeline.

- Fluxo Geral

  - 1. A função Lambda é acionada por um evento contendo uma lista de registros.
  - 2. Cada registro na lista Records é processado individualmente.
  - 3. Os dados são extraídos e decodificados de JSON.
  - 4. Se um erro crítico for detectado (indicado pela presença de error_code), ele é registrado no console.
  - 5. Os dados são enviados para outra função (process_data) para continuar o processamento.
   

#### 3. Armazenamento dos Dados no Amazon S3

- Dados brutos e processados são armazenados no Amazon S3 para consultas históricas e auditoria.
- Configurações de ciclo de vida no S3 movem dados antigos para o S3 Glacier para reduzir custos.

#### 4. Indexação e Busca com OpenSearch

- Os dados processados são enviados para o Amazon OpenSearch Service.
- Permite buscas rápidas, como identificar picos de erros ou filtrar logs por região.

#### 5. Consultas Ad-hoc com Amazon Athena

- O Amazon Athena é configurado para consultar os dados no S3 usando o SQL.
- Exemplo de consulta:

 <img src="https://github.com/JosiTubaroski/Processamento_Dados_AWS/blob/main/img/08_Select.png">  

#### 6. Monitoramento e Alertas com CloudWatch

- Amazon CloudWatch recebe métricas e logs do Lambda e Kinesis para monitorar a saúde do pipeline.
- Configurações de alarmes notificam a equipe sobre problemas, como atrasos no processamento.

#### 7. Visualização com Amazon QuickSight

- Conecta-se ao S3, OpenSearch e Athena para criar painéis interativos.
- Exemplos de visualizações:
  - Gráfico de barras mostrando acessos por tipo de dispositivo.
  - Linha do tempo de erros detectados.
  - Mapas de calor indicando regiões com mais acessos.
 
### Benefícios:

1. Processamento em Tempo Real: Através do Kinesis e Lambda, eventos são processados assim que chegam.
2. Armazenamento Escalável: O S3 permite armazenar grandes volumes de dados a baixo custo.
3. Consultas Flexíveis: Athena possibilita explorar dados rapidamente sem a necessidade de ETLs complexos.
4. Busca Rápida: OpenSearch facilita a localização de informações específicas.
5. Visualizações Poderosas: QuickSight oferece insights acionáveis em dashboards personalizáveis.
6. Escalabilidade Automática: Os serviços AWS escalam automaticamente com o aumento do volume de dados.

Essa arquitetura ilustra como as ferramentas da AWS podem ser integradas para criar um pipeline de Big Data escalável e eficiente para análise de dados em tempo real e histórico.

