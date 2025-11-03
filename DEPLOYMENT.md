# Guia de Configuração e Deploy

Este documento contém instruções detalhadas para configurar e realizar o deploy do projeto Ivory DevOps Test.

## 🔧 Configuração Inicial

### 1. Configuração do SonarCloud

1. Acesse [SonarCloud](https://sonarcloud.io)
2. Faça login com sua conta GitHub
3. Clique em "+" e selecione "Analyze new project"
4. Selecione o repositório `ivory-devops-test`
5. Configure as seguintes propriedades:
   - **Project Key**: `ivory-devops-test`
   - **Organization**: `ivory-it` (ou sua organização)
   - **Visibility**: Public (para evitar cobranças)

### 2. Configuração do AWS

#### 2.1 Criando Bucket S3
```bash
# Criar bucket S3 (substitua pelo nome único)
aws s3 mb s3://ivory-devops-test-site --region us-east-1

# Configurar bucket para site estático
aws s3 website s3://ivory-devops-test-site --index-document index.html --error-document index.html

# Configurar política do bucket para acesso público
aws s3api put-bucket-policy --bucket ivory-devops-test-site --policy file://bucket-policy.json
```

#### 2.2 Configuração do CloudFront
```bash
# Criar distribuição CloudFront via AWS CLI
aws cloudfront create-distribution --distribution-config file://cloudfront-config.json
```

### 3. Secrets do GitHub

Configure os seguintes secrets no repositório GitHub:

#### 3.1 SonarCloud
- `SONAR_TOKEN`: Token de autenticação do SonarCloud
  - Vá para SonarCloud → Account → Security → Generate Token

#### 3.2 AWS
- `AWS_ACCESS_KEY_ID`: Chave de acesso AWS
- `AWS_SECRET_ACCESS_KEY`: Chave secreta AWS
- `S3_BUCKET_NAME`: Nome do bucket S3 (ex: ivory-devops-test-site)
- `CLOUDFRONT_DISTRIBUTION_ID`: ID da distribuição CloudFront

## 🚀 Fluxo de Deploy

### Estratégia de Branching

1. **Feature Branch** → `develop`
2. **develop** → `homolog` (para homologação)
3. **homolog** → `main` (para produção)

### Pipeline de Deploy

O deploy automático é executado quando há push para a branch `main`:

1. **Build & Test**: Validação de código e análise no SonarCloud
2. **Security Scan**: Verificação de segurança básica
3. **Deploy**: Sincronização com S3 e invalidação do CloudFront

## 📁 Arquivos de Configuração AWS

### bucket-policy.json
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::ivory-devops-test-site/*"
    }
  ]
}
```

### cloudfront-config.json
```json
{
  "CallerReference": "ivory-devops-test-2024",
  "Origins": {
    "Quantity": 1,
    "Items": [
      {
        "Id": "S3-ivory-devops-test-site",
        "DomainName": "ivory-devops-test-site.s3.us-east-1.amazonaws.com",
        "S3OriginConfig": {
          "OriginAccessIdentity": ""
        }
      }
    ]
  },
  "DefaultCacheBehavior": {
    "TargetOriginId": "S3-ivory-devops-test-site",
    "ViewerProtocolPolicy": "redirect-to-https",
    "MinTTL": 0,
    "ForwardedValues": {
      "QueryString": false,
      "Cookies": {"Forward": "none"}
    }
  },
  "Comment": "Ivory DevOps Test CloudFront Distribution",
  "Enabled": true,
  "DefaultRootObject": "index.html"
}
```

## 🔍 Monitoramento

### SonarCloud Quality Gate
- O projeto está configurado para usar Quality Gate padrão
- Análise executada a cada push/PR
- Resultados disponíveis em: https://sonarcloud.io/project/overview?id=ivory-devops-test

### Logs de Deploy
- Logs disponíveis na aba "Actions" do GitHub
- Monitoramento de deploy via AWS CloudWatch
- Métricas do CloudFront disponíveis no console AWS

## 🛠️ Comandos Úteis

### Deploy Local para Teste
```bash
# Sincronizar arquivos com S3
aws s3 sync . s3://ivory-devops-test-site --exclude "*.git*" --exclude "*.md" --exclude "node_modules/*"

# Invalidar cache do CloudFront
aws cloudfront create-invalidation --distribution-id YOUR_DISTRIBUTION_ID --paths "/*"
```

### Verificação do Site
```bash
# Verificar status do bucket
aws s3api get-bucket-website --bucket ivory-devops-test-site

# Verificar distribuição CloudFront
aws cloudfront get-distribution --id YOUR_DISTRIBUTION_ID
```

## 📞 Suporte

Em caso de problemas com o deploy:

1. Verificar logs no GitHub Actions
2. Validar configurações do SonarCloud
3. Conferir policies e permissões AWS
4. Testar conectividade com os serviços

---

© 2025 IVORY - Documentação de Deploy
