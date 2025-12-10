# Template CloudFormation - VPC com Alta Disponibilidade

Este template CloudFormation cria uma VPC completa na AWS com alta disponibilidade, incluindo:

- **VPC** com suporte a DNS
- **2 Subnets Públicas** em diferentes Availability Zones
- **2 Subnets Privadas** em diferentes Availability Zones
- **Internet Gateway** para acesso à internet
- **2 NAT Gateways** (um em cada AZ) para acesso à internet das subnets privadas
- **Tabelas de Rotas** configuradas automaticamente
- **Security Group** básico sem regras de entrada

## Parâmetros

O template aceita os seguintes parâmetros:

- **EnvironmentName**: Nome do ambiente (será usado como prefixo nos recursos)
- **VpcCIDR**: CIDR da VPC (padrão: 10.192.0.0/16)
- **PublicSubnet1CIDR**: CIDR da primeira subnet pública (padrão: 10.192.10.0/24)
- **PublicSubnet2CIDR**: CIDR da segunda subnet pública (padrão: 10.192.11.0/24)
- **PrivateSubnet1CIDR**: CIDR da primeira subnet privada (padrão: 10.192.20.0/24)
- **PrivateSubnet2CIDR**: CIDR da segunda subnet privada (padrão: 10.192.21.0/24)

## Como fazer o deploy

### Usando AWS CLI

```bash
aws cloudformation create-stack \
  --stack-name minha-vpc \
  --template-body file://vpc.yaml \
  --parameters file://parameters.json \
  --region us-east-2
```

### Usando Console AWS

1. Acesse o console do CloudFormation na AWS
2. Clique em "Create stack"
3. Selecione "Upload a template file"
4. Faça upload do arquivo `vpc.yaml`
5. Configure os parâmetros ou use o arquivo `parameters.json`
6. Revise e crie a stack

### Atualizar uma stack existente

```bash
aws cloudformation update-stack \
  --stack-name minha-vpc \
  --template-body file://vpc.yaml \
  --parameters file://parameters.json \
  --region us-east-2
```

## Outputs

Após o deploy, o template retorna:

- **VPC**: ID da VPC criada
- **PublicSubnets**: Lista de IDs das subnets públicas
- **PrivateSubnets**: Lista de IDs das subnets privadas
- **PublicSubnet1/2**: IDs individuais das subnets públicas
- **PrivateSubnet1/2**: IDs individuais das subnets privadas
- **InternetGateway**: ID do Internet Gateway
- **NatGateway1/2**: IDs dos NAT Gateways
- **VpcCIDR**: CIDR block da VPC
- **NoIngressSecurityGroup**: ID do Security Group criado

## Custos

⚠️ **Atenção**: Este template cria 2 NAT Gateways, que têm custo de aproximadamente $0.045/hora cada (cerca de $32/mês por gateway). Se você não precisar de alta disponibilidade, considere usar apenas um NAT Gateway.

## Personalização

Você pode modificar os valores padrão no arquivo `parameters.json` ou passar os parâmetros diretamente na linha de comando.

