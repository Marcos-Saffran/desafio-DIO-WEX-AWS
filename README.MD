# RELATÓRIO DE IMPLEMENTAÇÃO DE SERVIÇOS AWS

Data: 17 de julho de 2025

Empresa: Abstergo Industries 

Responsável: Marcos

## Introdução
Este relatório apresenta o processo de implementação de ferramentas na empresa Abstergo Industries, realizado por Marcos. O objetivo do projeto foi elencar 3 serviços AWS, com a finalidade de realizar diminuição de custos imediatos.

## Descrição do Projeto
O projeto de implementação de ferramentas foi dividido em 3 etapas, cada uma com seus objetivos especí­ficos. A seguir, serão descritas as etapas do projeto:

Etapa 1: 
- Amazon EC2 (Elastic Compute Cloud)
- Infraestrutura computacional escalável para sistemas de gestão de estoque e pedidos
- Implementação de servidores virtuais com capacidade de auto-scaling para gerenciar picos de demanda durante períodos de alta movimentação de produtos farmacêuticos. A redução de custos ocorre através da eliminação de servidores físicos ociosos e pagamento apenas pelos recursos utilizados, resultando em economia de até 60% nos custos de infraestrutura.

Etapa 2: 
- Amazon RDS (Relational Database Service)
- Gerenciamento de banco de dados para controle de estoque e relacionamento com parceiros
- Migração do banco de dados local para um serviço gerenciado que oferece backup automático, alta disponibilidade e recuperação de desastres. Elimina a necessidade de administrador de banco dedicado e reduz custos operacionais em aproximadamente 40%, além de garantir conformidade com regulamentações farmacêuticas através de criptografia nativa.

Etapa 3: 
- Amazon S3 (Simple Storage Service)
- Armazenamento seguro e escalável para documentos regulatórios e dados de rastreabilidade
- Implementação de storage em nuvem para armazenar documentos de conformidade, certificados de produtos e dados de rastreabilidade exigidos pela ANVISA. Reduz custos de armazenamento físico em até 70% e oferece diferentes classes de armazenamento (Standard, IA, Glacier) para otimizar custos baseado na frequência de acesso aos dados.



## Conclusão
A implementação de ferramentas na empresa *Abstergo Industries tem como esperado uma redução significativa de custos operacionais (estimada em 50-60% nos custos de TI), maior agilidade na distribuição de produtos farmacêuticos através de sistemas escaláveis, conformidade automática com regulamentações do setor, e melhoria na rastreabilidade de produtos*, o que aumentará a eficiência e a produtividade da empresa. Recomenda-se a continuidade da utilização das ferramentas implementadas e a busca por novas tecnologias que possam melhorar ainda mais os processos da empresa.

## Anexos

### Documentação Técnica Gerada
1. **[Manual de Configuração - Amazon EC2](./Manual_Configuracao_EC2.md)**
   - Configuração detalhada de instâncias EC2
   - Setup de Auto Scaling e Load Balancer
   - Estimativas de custos e links oficiais

2. **[Manual de Configuração - Amazon RDS](./Manual_Configuracao_RDS.md)**
   - Migração de banco de dados PostgreSQL
   - Configurações de segurança e conformidade ANVISA
   - Processo de migração com AWS DMS

3. **[Manual de Configuração - Amazon S3](./Manual_Configuracao_S3.md)**
   - Estrutura de buckets para documentos regulatórios
   - Políticas de lifecycle e classes de armazenamento
   - Configurações de segurança e compliance

4. **[Análise Comparativa de Custos](./Analise_Custos_Comparativa.md)**
   - Comparação detalhada: on-premises vs AWS
   - Economia projetada de 92,5% nos custos de TI
   - ROI e projeções de crescimento para 3 anos

5. **[Documentação de Migração de Dados](./Documentacao_Migracao_Dados.md)**
   - Estratégia completa de migração em 4 fases
   - Scripts e comandos técnicos
   - Planos de contingência e rollback

6. **[Certificados de Conformidade ANVISA](./Certificados_Conformidade_ANVISA.md)**
   - Certificações AWS reconhecidas pela ANVISA
   - Evidências de conformidade regulatória
   - Declaração de conformidade assinada

### Links Úteis da AWS
- [Documentação Oficial EC2](https://docs.aws.amazon.com/ec2/)
- [Documentação Oficial RDS](https://docs.aws.amazon.com/rds/)
- [Documentação Oficial S3](https://docs.aws.amazon.com/s3/)
- [AWS Compliance Center](https://aws.amazon.com/compliance/)
- [Calculadora de Preços AWS](https://calculator.aws/)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)

Assinatura do Responsável pelo Projeto:

Marcos

Assinatura do Responsável pela Empresa:

Diretor de TI - Abstergo Industries