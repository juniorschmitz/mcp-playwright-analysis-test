# 🚀 ServeRest API Analysis & Testing Suite

[![API Tests](https://github.com/juniorschmitz/mcp-playwright-analysis-test/actions/workflows/api-tests.yml/badge.svg)](https://github.com/juniorschmitz/mcp-playwright-analysis-test/actions/workflows/api-tests.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## 📋 Visão Geral

Este projeto contém uma **análise completa e automatizada da API ServeRest**, incluindo documentação detalhada, collection Postman com testes automatizados e pipeline de CI/CD. O objetivo é fornecer uma suíte robusta de testes para validação contínua da qualidade da API.

### 🎯 Objetivos do Projeto

- **Análise Completa**: Documentação detalhada de todos os endpoints da API ServeRest
- **Testes Automatizados**: Collection Postman com 58+ casos de teste automatizados
- **CI/CD Integration**: Pipeline GitHub Actions para execução automática dos testes
- **Qualidade de Software**: Validação contínua de funcionalidades, segurança e performance

## 🏗️ Arquitetura do Projeto

```
mcp-playwright-analysis-test/
├── docs/                                    # Documentação
│   ├── api-analysis.md                     # Análise detalhada da API
│   └── test-cases.md                       # Casos de teste documentados
├── postman/                                # Collection e Environment Postman
│   ├── collection/
│   │   └── ServeRest-Collection.json       # Collection principal
│   ├── environment/
│   │   └── ServeRest-Environment.json      # Variáveis de ambiente
│   └── README.md                           # Guia de uso da collection
├── .github/
│   └── workflows/
│       └── api-tests.yml                   # Pipeline CI/CD
├── reports/                                # Relatórios de execução (gerado)
└── README.md                               # Este arquivo
```

## 🧪 Abordagem de Testes

### Metodologia Aplicada

1. **Análise de Swagger**: Estudo detalhado da documentação OpenAPI
2. **Identificação de Cenários**: Mapeamento de casos de sucesso e falha
3. **Automação de Testes**: Implementação em Postman com validações robustas
4. **Integração Contínua**: Pipeline automatizado para execução regular

### Tipos de Testes Implementados

#### 🔧 Testes Funcionais
- **Happy Path**: Cenários de sucesso para todos os endpoints
- **Sad Path**: Validação de tratamento de erros
- **Boundary Testing**: Testes de limites e valores extremos
- **Business Rules**: Validação de regras de negócio específicas

#### 🔒 Testes de Segurança
- **Autenticação**: Validação de login com credenciais válidas/inválidas
- **Autorização**: Controle de acesso (usuário comum vs administrador)
- **Token Management**: Gestão de JWT, expiração e invalidação
- **Input Validation**: Sanitização e validação de entradas

#### 🔄 Testes de Integração
- **End-to-End Flows**: Fluxo completo de compra online
- **Data Consistency**: Integridade entre operações relacionadas
- **State Management**: Gestão de estado entre requisições

#### ⚡ Testes de Performance
- **Response Time**: Validação de tempo de resposta
- **Load Testing**: Testes básicos de carga
- **Timeout Validation**: Comportamento com timeouts

## 📊 Cobertura de Testes

### API ServeRest - Módulos Testados

| Módulo | Endpoints | Casos de Teste | Cobertura |
|--------|-----------|----------------|-----------|
| **Login** | 1 | 3 | 100% |
| **Usuários** | 5 | 15 | 100% |
| **Produtos** | 5 | 20 | 100% |
| **Carrinhos** | 5 | 20 | 100% |
| **Integração** | - | 4 | - |
| **Total** | **16** | **62** | **100%** |

### Validações Automatizadas

- ✅ **Status Codes**: HTTP 200, 201, 400, 401, 403
- ✅ **Response Structure**: Validação de schema JSON
- ✅ **Business Logic**: Regras específicas da aplicação
- ✅ **Data Integrity**: Consistência de dados entre operações
- ✅ **Performance**: Tempo de resposta < 2 segundos
- ✅ **Security**: Controle de acesso e autenticação

## 🚀 Como Usar

### Pré-requisitos

- [Node.js](https://nodejs.org/) v14+ (para Newman CLI)
- [Postman](https://www.postman.com/) (para execução manual)
- Git

### 1. Clone o Repositório

```bash
git clone https://github.com/juniorschmitz/mcp-playwright-analysis-test.git
cd mcp-playwright-analysis-test
```

### 2. Instalar Dependências

```bash
npm install
```

### 3. Executar Testes Localmente

#### Via Postman (GUI)
1. Abra o Postman
2. Importe `postman/collection/ServeRest-Collection.json`
3. Importe `postman/environment/ServeRest-Environment.json`
4. Execute a collection

#### Via Newman (CLI)
```bash
# Executar todos os testes
npm run test

# Executar apenas testes funcionais
npm run test:functional

# Executar com relatório HTML
npm run test:report
```

### 4. Visualizar Relatórios

Os relatórios são gerados em `reports/` após a execução:
- `newman-report.html` - Relatório detalhado
- `newman-report.json` - Dados em JSON
- `newman-report.xml` - Formato JUnit

## 🔄 CI/CD Pipeline

### GitHub Actions Workflow

O pipeline automatizado executa:

1. **Setup**: Instalação do Node.js e Newman
2. **Test Execution**: Execução da collection Postman
3. **Report Generation**: Geração de relatórios HTML/XML
4. **Artifact Upload**: Upload dos resultados
5. **Notification**: Status da execução

### Triggers

- ✅ **Push** para `main` branch
- ✅ **Pull Request** para `main`
- ✅ **Schedule**: Execução diária às 08:00 UTC
- ✅ **Manual**: Trigger manual via GitHub UI

### Visualização de Resultados

- **GitHub Actions Tab**: Status de execução
- **Artifacts**: Download dos relatórios
- **Badge**: Status no README principal

## 📁 Estrutura dos Arquivos

### Documentação (`docs/`)

- **`api-analysis.md`**: Análise detalhada da API ServeRest
- **`test-cases.md`**: Documentação completa dos casos de teste

### Collection Postman (`postman/`)

- **`collection/`**: Collection principal com todos os testes
- **`environment/`**: Variáveis de ambiente para diferentes ambientes
- **`README.md`**: Guia detalhado de uso da collection

### Pipeline (`..github/workflows/`)

- **`api-tests.yml`**: Workflow de CI/CD para execução automática

## 🛠️ Tecnologias Utilizadas

- **API Testing**: Postman + Newman
- **CI/CD**: GitHub Actions
- **Documentation**: Markdown
- **Reporting**: HTML, XML, JSON
- **Version Control**: Git + GitHub

## 📈 Métricas e KPIs

### Métricas de Qualidade

- **Pass Rate**: Meta > 95%
- **Response Time**: < 2000ms (P95)
- **Coverage**: 100% dos endpoints
- **Reliability**: Zero falsos positivos

### Monitoramento Contínuo

- Execução automática diária
- Alertas para falhas
- Histórico de execuções
- Tendências de performance

## 🤝 Contribuição

### Como Contribuir

1. Fork o projeto
2. Crie uma branch para sua feature (`git checkout -b feature/nova-feature`)
3. Commit suas mudanças (`git commit -m 'Adiciona nova feature'`)
4. Push para a branch (`git push origin feature/nova-feature`)
5. Abra um Pull Request

### Padrões de Código

- **Nomenclatura**: Seguir padrão CTxxx para casos de teste
- **Documentação**: Atualizar docs para novas funcionalidades
- **Validações**: Incluir validações robustas
- **Performance**: Manter testes eficientes

## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.

## 👨‍💻 Autor

**Jacques Schmitz Junior**
- GitHub: [@juniorschmitz](https://github.com/juniorschmitz)
- Email: juniorschmitz9@gmail.com
- LinkedIn: [Jacques Schmitz](https://www.linkedin.com/in/jacques-schmitz/)

## 🙏 Agradecimentos

- **ServeRest API**: Excelente API para estudos de testes
- **Paulo Gonçalves**: Criador da ServeRest API
- **Compass UOL**: Ambiente de aprendizado e inovação

## 📚 Recursos Adicionais

- [ServeRest API Documentation](https://compassuol.serverest.dev/)
- [Postman Learning Center](https://learning.postman.com/)
- [Newman Documentation](https://github.com/postmanlabs/newman)
- [GitHub Actions Documentation](https://docs.github.com/actions)

---

**⭐ Se este projeto foi útil para você, não esqueça de dar uma estrela!**

---

*Última atualização: 19 de Agosto de 2025*