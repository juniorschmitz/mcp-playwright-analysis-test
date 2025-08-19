# 📋 Collection Postman - ServeRest API

## 🎯 Visão Geral

Esta collection contém uma suíte completa de testes automatizados para a API ServeRest, incluindo testes funcionais, de integração e de performance. A collection foi desenvolvida seguindo as melhores práticas de testes de API e inclui validações automatizadas para todos os cenários principais.

## 📁 Arquivos Incluídos

1. **ServeRest-Collection.json** - Collection principal com todos os testes
2. **ServeRest-Environment.json** - Environment com variáveis de configuração
3. **README.md** - Este guia de utilização

## 🏗️ Estrutura da Collection

### 1. 🔑 MÓDULO LOGIN
- **CT001** - Login com credenciais válidas
- **CT002** - Login com email inválido  
- **CT003** - Login com senha inválida

### 2. 👥 MÓDULO USUÁRIOS
- **CT006** - Listar todos os usuários
- **CT010** - Cadastrar usuário válido
- **CT012** - Cadastrar usuário com email duplicado
- **CT016** - Buscar usuário por ID válido
- **CT017** - Buscar usuário por ID inválido

### 3. 📦 MÓDULO PRODUTOS
- **CT024** - Listar todos os produtos
- **CT028** - Cadastrar produto como administrador
- **CT029** - Cadastrar produto sem autenticação
- **CT033** - Buscar produto por ID válido

### 4. 🛒 MÓDULO CARRINHOS
- **CT042** - Listar todos os carrinhos
- **CT045** - Cadastrar carrinho válido
- **CT050** - Cadastrar carrinho sem autenticação
- **CT051** - Buscar carrinho por ID válido
- **CT056** - Cancelar compra com carrinho existente

### 5. 🔄 TESTES DE INTEGRAÇÃO
- **CI001** - Fluxo completo de compra (7 etapas)

### 6. ⚡ TESTES DE PERFORMANCE
- **Performance** - Login múltiplo
- **Performance** - Listar produtos

## 🚀 Como Importar e Usar

### 1. Importar a Collection no Postman

1. Abra o Postman
2. Clique em **"Import"** no canto superior esquerdo
3. Selecione o arquivo `ServeRest-Collection.json`
4. Clique em **"Import"**

### 2. Importar o Environment

1. No Postman, clique no ícone de **"Environment"** (engrenagem) no canto superior direito
2. Clique em **"Import"**
3. Selecione o arquivo `ServeRest-Environment.json`
4. Clique em **"Import"**
5. Selecione o environment **"ServeRest Environment"** como ativo

### 3. Executar os Testes

#### ▶️ Executar Testes Individuais
1. Navegue até a collection "ServeRest API - Testes Automatizados"
2. Selecione qualquer teste individual
3. Clique em **"Send"**
4. Verifique os resultados na aba **"Test Results"**

#### ▶️ Executar Toda a Collection
1. Clique com botão direito na collection
2. Selecione **"Run collection"**
3. Configure as opções:
   - **Iterations**: 1 (para execução única)
   - **Delay**: 500ms (para evitar sobrecarga)
   - **Data File**: Nenhum
4. Clique em **"Run ServeRest API - Testes Automatizados"**

#### ▶️ Executar Apenas um Módulo
1. Clique com botão direito no módulo desejado (ex: "1. MÓDULO LOGIN")
2. Selecione **"Run folder"**
3. Configure e execute

## 🤖 Funcionalidades Automatizadas

### 🔐 Gestão Automática de Tokens
- Login automático nos testes que requerem autenticação
- Armazenamento e reutilização de tokens JWT
- Diferenciação entre tokens de usuário comum e administrador

### 📊 Gestão de Dados Dinâmicos
- Geração automática de timestamps únicos
- Criação de emails únicos para evitar conflitos
- Armazenamento de IDs criados para uso em testes subsequentes

### ✅ Validações Automatizadas
- **Status Codes**: Verificação de códigos HTTP esperados
- **Response Structure**: Validação da estrutura JSON das respostas
- **Business Rules**: Verificação de regras de negócio específicas
- **Response Time**: Validação de performance
- **Data Integrity**: Verificação de integridade dos dados

### 🔧 Pré-condições Automáticas
- Setup automático de dados necessários
- Login automático quando necessário
- Limpeza de dados entre testes

## 🔧 Variáveis de Environment

| Variável | Descrição | Tipo |
|----------|-----------|------|
| `baseUrl` | URL base da API ServeRest | Default |
| `token` | Token JWT para autenticação | Secret |
| `userId` | ID do usuário criado durante os testes | Default |
| `productId` | ID do produto criado durante os testes | Default |
| `cartId` | ID do carrinho criado durante os testes | Default |
| `integrationUserId` | ID do usuário para testes de integração | Default |
| `integrationToken` | Token para testes de integração | Secret |
| `integrationCartId` | ID do carrinho para testes de integração | Default |
| `timestamp` | Timestamp único para cada execução | Default |

## 🧪 Tipos de Testes Incluídos

### Testes Funcionais
- ✅ Cenários de sucesso (happy path)
- ✅ Cenários de erro (sad path)
- ✅ Validação de campos obrigatórios
- ✅ Validação de regras de negócio
- ✅ Testes de boundary

### Testes de Segurança
- ✅ Autenticação com credenciais válidas/inválidas
- ✅ Autorização (usuário comum vs administrador)
- ✅ Validação de tokens JWT
- ✅ Testes com tokens expirados/inválidos

### Testes de Integração
- ✅ Fluxo completo de e-commerce
- ✅ Interdependências entre módulos
- ✅ Consistência de dados entre operações

### Testes de Performance
- ✅ Tempo de resposta individual
- ✅ Testes de carga básica
- ✅ Validação de timeouts

## 📋 Cenários de Teste Principais

### 1. Gestão de Usuários
- Cadastro, consulta, edição e exclusão
- Validação de emails únicos
- Controle de usuários com carrinho

### 2. Gestão de Produtos  
- CRUD completo para administradores
- Controle de acesso por permissão
- Validação de produtos em carrinho

### 3. Gestão de Carrinhos
- Criação e gestão de carrinho por usuário
- Controle de estoque automático
- Fluxos de compra e cancelamento

### 4. Autenticação e Autorização
- Login com diferentes tipos de usuário
- Gestão de tokens JWT
- Controle de permissões por endpoint

## 📊 Interpretando os Resultados

### Execução Individual
- **🟢 Verde**: Teste passou
- **🔴 Vermelho**: Teste falhou
- **📋 Detalhes**: Clique em "Test Results" para ver detalhes

### Execução em Lote
- **📈 Summary**: Visão geral de sucessos/falhas
- **⏱️ Timeline**: Tempo de execução de cada teste
- **❌ Failed Tests**: Lista de testes que falharam com detalhes

### Métricas Importantes
- **📊 Pass Rate**: Porcentagem de testes que passaram
- **⚡ Average Response Time**: Tempo médio de resposta
- **🕐 Total Runtime**: Tempo total de execução

## 🔧 Troubleshooting

### ⚠️ Problemas Comuns

1. **🔑 Token Expirado**
   - **Sintoma**: Erro 401 em testes autenticados
   - **Solução**: Execute novamente o teste de login

2. **📧 Email Duplicado**
   - **Sintoma**: Erro 400 "Este email já está sendo usado"
   - **Solução**: A collection usa timestamps únicos, mas em execução muito rápida pode ocorrer

3. **🛒 Produto em Carrinho**
   - **Sintoma**: Erro ao tentar excluir produto
   - **Solução**: Cancele carrinho primeiro ou use outro produto

4. **⏱️ Rate Limiting**
   - **Sintoma**: Muitos erros 429 ou timeouts
   - **Solução**: Aumente o delay entre requests

### 💡 Dicas de Execução

1. **🚀 Primeira Execução**
   - Execute toda a collection uma vez para setup inicial
   - Verifique se todas as variáveis foram populadas

2. **🔨 Execução de Desenvolvimento**
   - Execute módulos individuais durante desenvolvimento
   - Use delay de 1000ms para debugging

3. **🏭 Execução de CI/CD**
   - Use delay mínimo (100-500ms)
   - Configure retry para testes flaky
   - Monitore métricas de performance

## 📈 Relatórios e Métricas

### Newman (CLI)
Para executar via linha de comando:

```bash
# Instalar Newman
npm install -g newman

# Executar collection
newman run ServeRest-Collection.json -e ServeRest-Environment.json

# Executar com relatório HTML
newman run ServeRest-Collection.json -e ServeRest-Environment.json -r html --reporter-html-export report.html

# Executar com múltiplos relatórios
newman run ServeRest-Collection.json -e ServeRest-Environment.json -r html,json,junit --reporter-html-export report.html
```

### 🔄 Integração com CI/CD
A collection pode ser integrada em pipelines de CI/CD usando Newman para:
- Testes automatizados em pull requests
- Validação em ambientes de staging
- Monitoramento contínuo em produção
- Geração de relatórios automáticos

## 🛠️ Manutenção

### 🔄 Atualizações da API
Quando a API ServeRest for atualizada:
1. Revise a documentação Swagger
2. Atualize endpoints que mudaram
3. Adicione novos testes para novas funcionalidades
4. Atualize validações conforme necessário

### 📈 Evolução dos Testes
Para adicionar novos testes:
1. Siga o padrão de nomenclatura (CTxxx)
2. Inclua validações adequadas
3. Atualize a documentação
4. Teste a integração com testes existentes

## 📝 Configurações Avançadas

### 🎛️ Configurações de Execução

```javascript
// Pre-request Script para configurações globais
pm.environment.set("timestamp", Date.now());
pm.environment.set("randomEmail", `test${Date.now()}@qa.com`);

// Test Script para validações avançadas
pm.test("Response time is less than 2000ms", () => {
    pm.expect(pm.response.responseTime).to.be.below(2000);
});

pm.test("Response has valid JSON structure", () => {
    pm.response.to.have.jsonBody();
});
```

### 📊 Métricas Personalizadas

```javascript
// Capturar métricas personalizadas
const responseTime = pm.response.responseTime;
const statusCode = pm.response.code;

console.log(`Endpoint: ${pm.request.url.path.join('/')}`);
console.log(`Status: ${statusCode}`);
console.log(`Response Time: ${responseTime}ms`);
```

## 🤝 Contribuição

Para contribuir com melhorias:
1. Mantenha o padrão de nomenclatura
2. Inclua validações abrangentes
3. Documente novos cenários
4. Teste thoroughly antes de commit

### 📋 Checklist para Novos Testes
- [ ] Nomenclatura seguindo padrão CTxxx
- [ ] Validações de status code
- [ ] Validações de estrutura de resposta
- [ ] Validações de regras de negócio
- [ ] Tratamento de pré-requisitos
- [ ] Documentação atualizada

## 📚 Recursos Adicionais

### 🔗 Links Úteis
- [Documentação da API ServeRest](https://compassuol.serverest.dev/)
- [Postman Learning Center](https://learning.postman.com/)
- [Newman Documentation](https://github.com/postmanlabs/newman)
- [Chai Assertion Library](https://www.chaijs.com/)

### 📖 Documentação Relacionada
- [Análise Completa da API](../docs/api-analysis.md)
- [Casos de Teste Detalhados](../docs/test-cases.md)
- [Guia de Configuração CI/CD](../.github/workflows/api-tests.yml)

---

**📝 Criado por**: Jacques Schmitz Junior  
**📅 Data**: 19 de Agosto de 2025  
**🔖 Versão da API**: ServeRest 2.29.7  
**🏷️ Versão da Collection**: 1.0.0

---

*Para dúvidas ou sugestões, abra uma issue no repositório do projeto.*