# Manual de Configuração - Amazon S3

## Objetivo
Configuração do Amazon S3 para armazenamento seguro de documentos regulatórios e dados de rastreabilidade da Abstergo Industries.

## Estrutura de Buckets

### Bucket Principal: abstergo-documents-prod
- **Região**: us-east-1 (N. Virginia)
- **Versionamento**: Habilitado
- **Criptografia**: SSE-S3 (AES-256)

### Estrutura de Pastas
```
abstergo-documents-prod/
├── anvisa-certificates/
│   ├── 2025/
│   └── historico/
├── product-documentation/
│   ├── bulas/
│   ├── rotulos/
│   └── certificados-qualidade/
├── traceability-data/
│   ├── lotes/
│   ├── fornecedores/
│   └── distribuicao/
└── backup-systems/
    ├── daily/
    └── weekly/
```

## Classes de Armazenamento

### Standard (Acesso Frequente)
- Documentos ativos (últimos 30 dias)
- Certificados ANVISA vigentes
- **Custo**: $0.023/GB/mês

### Standard-IA (Acesso Infrequente)
- Documentos de 30 dias a 1 ano
- Histórico de lotes recentes
- **Custo**: $0.0125/GB/mês

### Glacier (Arquivo)
- Documentos históricos (>1 ano)
- Backups de longo prazo
- **Custo**: $0.004/GB/mês

## Configuração de Segurança

### Políticas de Bucket
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AbstergoAccess",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::ACCOUNT:role/AbstergoRole"
            },
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::abstergo-documents-prod/*"
        }
    ]
}
```

### Controle de Acesso
- **IAM Roles**: Separação por departamento
- **MFA**: Obrigatório para exclusões
- **Logging**: CloudTrail habilitado
- **Monitoring**: CloudWatch métricas

## Lifecycle Policies

### Regra Automática de Transição
```json
{
    "Rules": [
        {
            "Id": "AbstergoLifecycle",
            "Status": "Enabled",
            "Transitions": [
                {
                    "Days": 30,
                    "StorageClass": "STANDARD_IA"
                },
                {
                    "Days": 365,
                    "StorageClass": "GLACIER"
                }
            ]
        }
    ]
}
```

## Conformidade ANVISA

### Requisitos Atendidos
- Retenção mínima de 5 anos para documentos farmacêuticos
- Integridade de dados garantida via versionamento
- Audit trail completo via CloudTrail
- Criptografia em trânsito e em repouso

### Backup e Recuperação
- Cross-Region Replication para us-west-2
- Backup diário automático
- RTO (Recovery Time Objective): 4 horas
- RPO (Recovery Point Objective): 1 hora

## Estimativa de Custos

### Cenário Típico (500GB de dados)
- **Standard (100GB)**: $2.30/mês
- **Standard-IA (200GB)**: $2.50/mês
- **Glacier (200GB)**: $0.80/mês
- **Total mensal**: ~$5.60
- **Economia vs. storage físico**: 70%

## Comandos Úteis

### Upload via AWS CLI
```bash
# Upload de arquivo com metadados
aws s3 cp documento.pdf s3://abstergo-documents-prod/anvisa-certificates/ \
    --metadata "department=regulatory,type=certificate"

# Sincronização de diretório
aws s3 sync ./documentos/ s3://abstergo-documents-prod/product-documentation/
```

## Links de Referência
- [S3 User Guide](https://docs.aws.amazon.com/s3/)
- [S3 Storage Classes](https://aws.amazon.com/s3/storage-classes/)
- [S3 Security](https://docs.aws.amazon.com/s3/latest/userguide/security.html)
