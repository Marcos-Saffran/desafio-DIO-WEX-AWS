# Manual de Configuração - Amazon EC2

## Objetivo
Este manual descreve os passos para configuração do Amazon EC2 para o sistema de gestão de estoque e pedidos da Abstergo Industries.

## Pré-requisitos
- Conta AWS ativa
- Permissões IAM adequadas
- Conhecimento básico de administração de sistemas

## Configuração do EC2

### 1. Criação da Instância
```bash
# Exemplo de comando AWS CLI
aws ec2 run-instances \
    --image-id ami-0c02fb55956c7d316 \
    --instance-type t3.medium \
    --key-name abstergo-key \
    --security-group-ids sg-12345678 \
    --subnet-id subnet-12345678
```

### 2. Configuração de Auto Scaling
- **Grupo de Auto Scaling**: abstergo-asg
- **Capacidade mínima**: 2 instâncias
- **Capacidade máxima**: 10 instâncias
- **Capacidade desejada**: 3 instâncias

### 3. Configuração de Load Balancer
- **Tipo**: Application Load Balancer
- **Portas**: 80 (HTTP) e 443 (HTTPS)
- **Health Check**: /health endpoint

### 4. Estimativa de Custos
- **Instâncias t3.medium**: $0.0416/hora
- **Economia estimada**: 60% comparado à infraestrutura física
- **Custo mensal projetado**: $600-1200 (variável conforme demanda)

## Segurança
- Utilizar Security Groups restritivos
- Implementar WAF (Web Application Firewall)
- Configurar CloudWatch para monitoramento

## Links Úteis
- [Documentação EC2](https://docs.aws.amazon.com/ec2/)
- [Guia de Auto Scaling](https://docs.aws.amazon.com/autoscaling/)
