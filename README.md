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

https://github.com/JosiTubaroski/Processamento_Dados_AWS/blob/main/img/06_Json.png
