## Detalhes do levantamento de custos da Infra - Etapa 2

# Estimativa de Preços da AWS (29/01/2025)

## Resumo da Estimativa

| **Descrição**               | **Custo**     |
| --------------------------- | ------------- |
| **Custo Inicial**           | 0,02 USD      |
| **Custo Mensal**            | 1.350,76 USD  |
| **Custo Total em 12 meses** | 16.209,14 USD |
| **Inclui Custo Inicial**    | Sim           |

## Estimativa Detalhada

| **Nome do Serviço**                      | **Região**            | **Custo Inicial** | **Custo Mensal** | **Descrição**             | **Resumo da Configuração**                                                                                                                                                                                                                                         |
| ---------------------------------------- | --------------------- | ----------------- | ---------------- | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Elastic Load Balancing                   | US East (N. Virginia) | 0,00 USD          | 57,31 USD        | Application Load Balancer | Número de Application Load Balancers (1)                                                                                                                                                                                                                           |
| Amazon Route 53                          | US East (N. Virginia) | 0,00 USD          | 80,51 USD        | AWS Route 53              | Hosted Zones (1), Registros adicionais em Hosted Zones (10.000), Número de domínios armazenados (1)                                                                                                                                                                |
| Amazon CloudFront                        | US East (N. Virginia) | 0,00 USD          | 52,50 USD        | CloudFront CDN            | Transferência de dados para a internet (500 GB por mês), Transferência de dados para o origem (500 GB por mês), Número de requisições (HTTPS) (4.800 por mês)                                                                                                      |
| AWS Web Application Firewall (WAF)       | US East (N. Virginia) | 0,00 USD          | 22,00 USD        | WAF                       | Número de Web Access Control Lists (1 por mês), Número de Regras adicionadas por Web ACL (8 por mês), Número de Rule Groups por Web ACL (1 por mês), Número de Regras dentro de cada Rule Group (1 por mês), Número de Managed Rule Groups por Web ACL (1 por mês) |
| Amazon CloudWatch                        | US East (N. Virginia) | 0,00 USD          | 0,90 USD         | CloudWatch                | Número de Métricas (3), GetMetricData (3), GetMetricWidgetImage (1), Outras requisições de API (1)                                                                                                                                                                 |
| AWS Secrets Manager                      | US East (N. Virginia) | 0,00 USD          | 0,40 USD         | Secrets Manager           | Número de secrets (1), Duração média de cada secret (30 dias), Número de chamadas de API (1 por mês)                                                                                                                                                               |
| AWS Cost Explorer                        | US East (N. Virginia) | 0,00 USD          | 0,01 USD         | Budgets                   | Número de requisições API por mês (1), Número de UsageRecords por mês (1)                                                                                                                                                                                          |
| Amazon Simple Storage Service (S3)       | US East (N. Virginia) | 0,02 USD          | 1,28 USD         | S3                        | Armazenamento S3 Standard (50 GB por mês), Requisições PUT, COPY, POST, LIST (5), Requisições GET, SELECT (5), Dados retornados pelo S3 Select (50 GB por mês)                                                                                                     |
| Amazon Simple Notification Service (SNS) | US East (N. Virginia) | 0,00 USD          | 2,00 USD         | SNS                       | Requisições (5 milhões por mês), Notificações HTTP/HTTPS (milhões por mês)                                                                                                                                                                                         |
| Amazon Virtual Private Cloud (VPC)       | US East (N. Virginia) | 0,00 USD          | 738,55 USD       | VPC                       | Dias úteis por mês (22), Número de associações de sub-redes (5), Número de NAT Gateways (3)                                                                                                                                                                        |
| Amazon RDS for MySQL                     | US East (N. Virginia) | 0,00 USD          | 395,30 USD       | RDS                       | Armazenamento (100 GB), Tipo de instância (db.r5.large), Opção de implantação (Multi-AZ)                                                                                                                                                                           |

---

**Nota**: O AWS Pricing Calculator fornece apenas uma estimativa dos custos da AWS e não inclui impostos que possam ser aplicáveis. Seus custos reais dependem de uma variedade de fatores, incluindo o uso real dos serviços da AWS.
