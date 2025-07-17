# Manual de Configuração - Amazon RDS

## Objetivo
Este manual descreve a configuração do Amazon RDS para migração do banco de dados da Abstergo Industries.

## Especificações do Banco

### Configuração Recomendada
- **Engine**: PostgreSQL 14.9
- **Classe de Instância**: db.t3.medium
- **Armazenamento**: 500 GB SSD (gp2)
- **Multi-AZ**: Habilitado para alta disponibilidade

### Configurações de Segurança
- **Criptografia**: Habilitada (AES-256)
- **VPC Security Groups**: Acesso restrito apenas às instâncias EC2
- **Backup**: Retenção de 7 dias
- **Snapshot automático**: Diário às 03:00 UTC

## Processo de Migração

### 1. Preparação
```sql
-- Verificar compatibilidade do schema atual
SELECT version();
\dt -- Listar tabelas existentes
```

### 2. Criação da Instância RDS
```bash
aws rds create-db-instance \
    --db-instance-identifier abstergo-prod-db \
    --db-instance-class db.t3.medium \
    --engine postgres \
    --master-username admin \
    --master-user-password [SENHA_SEGURA] \
    --allocated-storage 500 \
    --vpc-security-group-ids sg-12345678
```

### 3. Migração de Dados
- Utilizar AWS Database Migration Service (DMS)
- Configurar endpoint de origem (banco local)
- Configurar endpoint de destino (RDS)
- Executar migração com validação

## Conformidade Regulatória

### ANVISA
- Audit logging habilitado
- Retenção de logs por 5 anos
- Criptografia em trânsito e em repouso
- Controle de acesso baseado em roles

### Validação
- Testes de integridade de dados
- Verificação de performance
- Testes de recuperação de backup

## Estimativa de Custos
- **Instância db.t3.medium**: $0.068/hora
- **Armazenamento**: $0.115/GB/mês
- **Backup**: Incluído até 500GB
- **Economia estimada**: 40% vs. administração local

## Links de Referência
- [RDS PostgreSQL](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_PostgreSQL.html)
- [AWS DMS](https://docs.aws.amazon.com/dms/)
