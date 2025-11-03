# Ivory DevOps Test - Static Website

Este projeto foi desenvolvido como parte do teste técnico para estágio DevOps da IVORY, demonstrando conhecimentos em Git, CI/CD, SonarCloud e deploy em nuvem.

## 📋 Objetivo

Provisionar um ambiente para hospedagem de um site estático com pipeline completa de CI/CD, incluindo análise de qualidade de código no SonarCloud e deploy automatizado em ambiente de nuvem.

## 🏗️ Arquitetura do Projeto

### Branches
- **main**: Branch principal de produção
- **develop**: Branch de desenvolvimento
- **homolog**: Branch de homologação

### Tecnologias Utilizadas
- HTML5, CSS3, JavaScript
- Git para versionamento
- GitHub Actions para CI/CD
- SonarCloud para análise de qualidade
- AWS S3 + CloudFront para hospedagem

## 🚀 Pipeline CI/CD

A pipeline é acionada automaticamente a cada push para a branch `main` e executa:

1. **Build**: Preparação dos arquivos estáticos
2. **Quality Analysis**: Análise no SonarCloud
3. **Deploy**: Upload para AWS S3 e invalidação do CloudFront

## 📦 Estrutura do Projeto

```
├── css/           # Arquivos de estilo
├── img/           # Imagens e ícones
├── js/            # Scripts JavaScript
├── json/          # Arquivos de configuração JSON
├── index.html     # Página principal
├── contato.html   # Página de contato
├── servicos.html  # Página de serviços
├── sobre.html     # Página sobre
└── .github/       # Workflows do GitHub Actions
```

## 🛠️ Como Executar Localmente

1. Clone o repositório:
```bash
git clone https://github.com/[SEU-USUARIO]/ivory-devops-test.git
cd ivory-devops-test
```

2. Abra o arquivo `index.html` em um navegador ou utilize um servidor HTTP local:
```bash
python -m http.server 8000
# ou
npx serve .
```

## 📊 Análise de Qualidade

Este projeto está configurado com SonarCloud para análise contínua de qualidade do código. Os resultados podem ser visualizados em: [Link do SonarCloud será adicionado]

## 🌐 Site em Produção

O site está disponível em: [Link será adicionado após deploy]

## 👥 Autor

Desenvolvido como parte do processo seletivo para estágio DevOps na IVORY.

---

© 2025 IVORY - Teste Técnico DevOps
