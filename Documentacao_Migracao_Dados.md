# Documentação de Migração de Dados

## Objetivo
Este documento descreve o processo detalhado para migração dos dados da Abstergo Industries para a infraestrutura AWS.

## Escopo da Migração

### Sistemas Envolvidos
1. **Sistema de Gestão de Estoque** (PostgreSQL 12)
2. **Sistema de Pedidos** (MySQL 8.0)
3. **Documentos Regulatórios** (File Server)
4. **Dados de Rastreabilidade** (Oracle 19c)

### Volumes de Dados
- **Banco de dados**: 2.5TB total
- **Arquivos**: 800GB de documentos
- **Backups históricos**: 1.2TB

## Estratégia de Migração

### Fase 1: Preparação (Semana 1-2)

#### Avaliação de Dados
```bash
# Análise de tamanho das tabelas
SELECT 
    schemaname,
    tablename,
    pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename)) as size
FROM pg_tables 
ORDER BY pg_total_relation_size(schemaname||'.'||tablename) DESC;
```

#### Ferramentas Necessárias
- AWS Database Migration Service (DMS)
- AWS DataSync (para arquivos)
- pgdump/pg_restore
- AWS CLI

### Fase 2: Migração de Teste (Semana 3)

#### Setup do Ambiente de Teste
1. Criar instâncias RDS de teste
2. Configurar VPC e Security Groups
3. Estabelecer conexão VPN

#### Comandos de Migração
```bash
# Backup completo do PostgreSQL
pg_dump -h localhost -U admin -d estoque > backup_estoque.sql

# Upload para S3
aws s3 cp backup_estoque.sql s3://abstergo-migration/backups/

# Restore no RDS
psql -h rds-endpoint.region.rds.amazonaws.com -U admin -d estoque < backup_estoque.sql
```

### Fase 3: Sincronização Inicial (Semana 4)

#### Configuração do DMS
```json
{
    "source-endpoint": {
        "endpoint-type": "source",
        "engine-name": "postgres",
        "server-name": "servidor-local.abstergo.com",
        "port": 5432,
        "database-name": "estoque"
    },
    "target-endpoint": {
        "endpoint-type": "target",
        "engine-name": "postgres",
        "server-name": "abstergo-prod.cluster-abc123.us-east-1.rds.amazonaws.com",
        "port": 5432,
        "database-name": "estoque"
    }
}
```

### Fase 4: Migração de Arquivos (Semana 4-5)

#### DataSync Configuration
```bash
# Criar tarefa DataSync
aws datasync create-task \
    --source-location-arn arn:aws:datasync:region:account:location/loc-source \
    --destination-location-arn arn:aws:datasync:region:account:location/loc-destination \
    --name "AbstergoDocumentMigration"
```

## Validação de Dados

### Checklist de Validação
- [ ] Contagem de registros por tabela
- [ ] Verificação de integridade referencial
- [ ] Validação de checksums de arquivos
- [ ] Teste de queries críticas
- [ ] Verificação de permissões

### Scripts de Validação
```sql
-- Validação de contagem
SELECT 
    'origem' as fonte, 
    COUNT(*) as total_registros 
FROM produtos
UNION ALL
SELECT 
    'destino' as fonte, 
    COUNT(*) as total_registros 
FROM produtos_migrados;

-- Verificação de integridade
SELECT COUNT(*) as orfaos
FROM pedidos p
LEFT JOIN clientes c ON p.cliente_id = c.id
WHERE c.id IS NULL;
```

## Cronograma Detalhado

### Semana 1: Preparação
- **Segunda**: Instalação e configuração de ferramentas
- **Terça**: Análise de dependências entre sistemas
- **Quarta**: Backup completo dos sistemas atuais
- **Quinta**: Testes de conectividade com AWS
- **Sexta**: Criação de ambientes de teste

### Semana 2: Setup AWS
- **Segunda**: Criação de instâncias RDS
- **Terça**: Configuração de S3 buckets
- **Quarta**: Setup de VPN e networking
- **Quinta**: Configuração de IAM roles
- **Sexta**: Testes de conectividade

### Semana 3: Migração de Teste
- **Segunda-Terça**: Migração do sistema de estoque
- **Quarta-Quinta**: Migração do sistema de pedidos
- **Sexta**: Validação e testes

### Semana 4: Produção
- **Segunda**: Sincronização inicial via DMS
- **Terça**: Migração de arquivos via DataSync
- **Quarta**: Validação completa
- **Quinta**: Go-live controlado
- **Sexta**: Monitoramento e ajustes

## Plano de Rollback

### Cenários de Rollback
1. **Falha na migração**: Retorno ao ambiente original
2. **Performance inadequada**: Otimização ou rollback
3. **Problemas de conectividade**: Ativação de contingência

### Procedimento de Rollback
```bash
# Parar aplicações
sudo systemctl stop abstergo-app

# Reconfigurar connection strings
sed -i 's/rds-endpoint/servidor-local/g' /etc/abstergo/config.ini

# Reiniciar aplicações
sudo systemctl start abstergo-app
```

## Monitoramento Pós-Migração

### Métricas Chave
- **Latência de queries**: < 50ms (target)
- **Disponibilidade**: > 99.9%
- **Throughput**: Manter níveis atuais
- **Uso de CPU/Memória**: < 70%

### Alertas Configurados
- Falha de conexão com RDS
- Latência > 100ms
- Uso de CPU > 80%
- Falha de backup automático

## Contingências

### Backup de Emergência
- Snapshot automático a cada 4 horas durante migração
- Backup completo diário mantido por 30 dias
- Point-in-time recovery configurado

### Suporte 24x7
- Equipe de plantão durante as 4 semanas
- Escalação para AWS Support (Business Plan)
- Contato direto com DBA especialista

## Compliance e Auditoria

### Registros Obrigatórios
- Log de todas as ações de migração
- Timestamp de início/fim de cada etapa
- Validação de integridade registrada
- Aprovações formais documentadas

### Certificação ANVISA
- Validação de criptografia em trânsito
- Verificação de backup e recovery
- Teste de audit trail
- Documentação de conformidade

---
*Documento atualizado em julho de 2025 - Versão 1.0*
