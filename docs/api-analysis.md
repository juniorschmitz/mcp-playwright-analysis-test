# 🔍 Análise Completa da API ServeRest

## 📖 Informações Gerais

### Visão Geral
A **ServeRest** é uma API REST gratuita que simula uma loja virtual, desenvolvida para fins educacionais e de testes. Disponibilizada em https://compassuol.serverest.dev/, oferece endpoints completos para gerenciamento de usuários, produtos e carrinho de compras.

### Especificações Técnicas
- **Versão da API**: 2.29.7
- **Especificação**: OpenAPI 3.0.0
- **URL Base**: `https://compassuol.serverest.dev`
- **Formato de Dados**: JSON
- **Autenticação**: JWT Bearer Token
- **Duração do Token**: 600 segundos (10 minutos)

### Propósito
Esta API foi criada para ser utilizada em:
- Treinamentos de automação de testes
- Estudos de APIs REST
- Prática de integração de sistemas
- Desenvolvimento de habilidades em QA

---

## 🏗️ Arquitetura da API

### Modelo de Dados

#### Usuário (User)
```json
{
  "_id": "string",
  "nome": "string",
  "email": "string",
  "password": "string",
  "administrador": "string" // "true" ou "false"
}
```

#### Produto (Product)
```json
{
  "_id": "string",
  "nome": "string",
  "preco": "number",
  "descricao": "string",
  "quantidade": "number"
}
```

#### Carrinho (Cart)
```json
{
  "_id": "string",
  "produtos": [
    {
      "idProduto": "string",
      "quantidade": "number",
      "precoUnitario": "number"
    }
  ],
  "precoTotal": "number",
  "quantidadeTotal": "number",
  "idUsuario": "string"
}
```

#### Token de Autenticação
```json
{
  "authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

### Relacionamentos
- **Usuário ↔ Carrinho**: 1:1 (um usuário pode ter apenas um carrinho)
- **Produto ↔ Carrinho**: N:M (produtos podem estar em múltiplos carrinhos)
- **Usuário ↔ Produto**: Administradores podem gerenciar produtos

---

## 🔗 Endpoints Detalhados

### 1. 🔑 Autenticação

#### POST /login
**Descrição**: Realiza autenticação de usuário e retorna token JWT

**Request Body**:
```json
{
  "email": "string",
  "password": "string"
}
```

**Responses**:
- **200 OK**: Login realizado com sucesso
  ```json
  {
    "message": "Login realizado com sucesso",
    "authorization": "Bearer {token}"
  }
  ```
- **401 Unauthorized**: Credenciais inválidas
  ```json
  {
    "message": "Email e/ou senha inválidos"
  }
  ```

**Validações**:
- Email deve ter formato válido
- Senha é obrigatória
- Usuário deve existir no sistema

**Regras de Negócio**:
- Token JWT expira em 600 segundos
- Token deve ser incluído no header `Authorization` para endpoints protegidos

---

### 2. 👥 Usuários

#### GET /usuarios
**Descrição**: Lista todos os usuários cadastrados

**Query Parameters**:
- `_id`: Filtrar por ID específico
- `nome`: Filtrar por nome
- `email`: Filtrar por email
- `administrador`: Filtrar por tipo (true/false)

**Response (200 OK)**:
```json
{
  "usuarios": [
    {
      "_id": "BeeJh5lz3k6kSIzA",
      "nome": "Fulano da Silva",
      "email": "fulano@qa.com",
      "password": "teste",
      "administrador": "true"
    }
  ],
  "quantidade": 1
}
```

#### GET /usuarios/{_id}
**Descrição**: Busca usuário específico por ID

**Path Parameters**:
- `_id`: ID do usuário

**Responses**:
- **200 OK**: Usuário encontrado
- **400 Bad Request**: Usuário não encontrado

#### POST /usuarios
**Descrição**: Cadastra novo usuário

**Request Body**:
```json
{
  "nome": "string",
  "email": "string",
  "password": "string",
  "administrador": "string"
}
```

**Validações**:
- Nome é obrigatório
- Email deve ser único e ter formato válido
- Password é obrigatória
- Administrador deve ser "true" ou "false"

**Responses**:
- **201 Created**: Usuário criado
- **400 Bad Request**: Dados inválidos ou email já existe

#### PUT /usuarios/{_id}
**Descrição**: Edita usuário existente ou cria novo se ID não existir

**Autenticação**: Requerida

**Request Body**: Mesmo formato do POST

**Responses**:
- **200 OK**: Usuário editado
- **201 Created**: Novo usuário criado
- **401 Unauthorized**: Token inválido

#### DELETE /usuarios/{_id}
**Descrição**: Remove usuário do sistema

**Autenticação**: Requerida

**Regras de Negócio**:
- Não é possível excluir usuário que possui carrinho

**Responses**:
- **200 OK**: Usuário excluído
- **400 Bad Request**: Usuário tem carrinho cadastrado
- **401 Unauthorized**: Token inválido

---

### 3. 📦 Produtos

#### GET /produtos
**Descrição**: Lista todos os produtos disponíveis

**Query Parameters**:
- `_id`: Filtrar por ID
- `nome`: Filtrar por nome
- `preco`: Filtrar por preço
- `descricao`: Filtrar por descrição

**Response (200 OK)**:
```json
{
  "produtos": [
    {
      "_id": "BeeJh5lz3k6kSIzA",
      "nome": "Logitech MX Vertical",
      "preco": 470,
      "descricao": "Mouse",
      "quantidade": 382
    }
  ],
  "quantidade": 1
}
```

#### GET /produtos/{_id}
**Descrição**: Busca produto específico por ID

**Responses**:
- **200 OK**: Produto encontrado
- **400 Bad Request**: Produto não encontrado

#### POST /produtos
**Descrição**: Cadastra novo produto

**Autenticação**: Requerida (apenas administradores)

**Request Body**:
```json
{
  "nome": "string",
  "preco": "number",
  "descricao": "string",
  "quantidade": "number"
}
```

**Validações**:
- Nome deve ser único
- Preço deve ser maior que 0
- Quantidade deve ser maior ou igual a 0

**Responses**:
- **201 Created**: Produto cadastrado
- **400 Bad Request**: Dados inválidos ou nome já existe
- **401 Unauthorized**: Token inválido
- **403 Forbidden**: Usuário não é administrador

#### PUT /produtos/{_id}
**Descrição**: Edita produto existente ou cria novo

**Autenticação**: Requerida (apenas administradores)

**Responses**:
- **200 OK**: Produto editado
- **201 Created**: Novo produto criado
- **401 Unauthorized**: Token inválido
- **403 Forbidden**: Usuário não é administrador

#### DELETE /produtos/{_id}
**Descrição**: Remove produto do sistema

**Autenticação**: Requerida (apenas administradores)

**Regras de Negócio**:
- Não é possível excluir produto que está em carrinho

**Responses**:
- **200 OK**: Produto excluído
- **400 Bad Request**: Produto faz parte de carrinho
- **401 Unauthorized**: Token inválido
- **403 Forbidden**: Usuário não é administrador

---

### 4. 🛒 Carrinhos

#### GET /carrinhos
**Descrição**: Lista todos os carrinhos ativos

**Query Parameters**:
- `_id`: Filtrar por ID
- `idUsuario`: Filtrar por usuário
- `precoTotal`: Filtrar por preço total

**Response (200 OK)**:
```json
{
  "carrinhos": [
    {
      "_id": "qbMqntef4iTOwWfg",
      "produtos": [
        {
          "idProduto": "BeeJh5lz3k6kSIzA",
          "quantidade": 2,
          "precoUnitario": 470
        }
      ],
      "precoTotal": 940,
      "quantidadeTotal": 2,
      "idUsuario": "0uxuPY0cbmQhpEz1"
    }
  ],
  "quantidade": 1
}
```

#### GET /carrinhos/{_id}
**Descrição**: Busca carrinho específico por ID

**Responses**:
- **200 OK**: Carrinho encontrado
- **400 Bad Request**: Carrinho não encontrado

#### POST /carrinhos
**Descrição**: Cadastra novo carrinho para usuário autenticado

**Autenticação**: Requerida

**Request Body**:
```json
{
  "produtos": [
    {
      "idProduto": "string",
      "quantidade": "number"
    }
  ]
}
```

**Regras de Negócio**:
- Usuário pode ter apenas um carrinho
- Produto deve existir
- Quantidade solicitada deve estar disponível em estoque
- Estoque é reduzido automaticamente

**Responses**:
- **201 Created**: Carrinho cadastrado
- **400 Bad Request**: Regras de negócio violadas
- **401 Unauthorized**: Token inválido

#### PUT /carrinhos/{_id}
**Descrição**: Edita carrinho existente

**Autenticação**: Requerida

**Comportamento**:
- Se carrinho não existir, cria novo
- Atualiza produtos e quantidades
- Recalcula preços automaticamente

#### DELETE /carrinhos/concluir-compra
**Descrição**: Finaliza compra e remove carrinho

**Autenticação**: Requerida

**Comportamento**:
- Remove carrinho do usuário autenticado
- Mantém produtos com estoque reduzido
- Simula conclusão da compra

**Responses**:
- **200 OK**: Compra concluída
- **401 Unauthorized**: Token inválido

#### DELETE /carrinhos/cancelar-compra
**Descrição**: Cancela compra e restaura estoque

**Autenticação**: Requerida

**Comportamento**:
- Remove carrinho do usuário autenticado
- Restaura quantidade dos produtos ao estoque
- Simula cancelamento da compra

**Responses**:
- **200 OK**: Compra cancelada
- **401 Unauthorized**: Token inválido

---

## 🔒 Sistema de Autenticação

### JWT Token
A API utiliza JSON Web Tokens (JWT) para autenticação:

**Características**:
- **Algoritmo**: HS256
- **Duração**: 600 segundos (10 minutos)
- **Header**: `Authorization: Bearer {token}`

**Estrutura do Token**:
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
eyJlbWFpbCI6ImZ1bGFub0BxYS5jb20iLCJwYXNzd29yZCI6InRlc3RlIiwiaWF0IjoxNjc5MzQyNjUwLCJleHAiOjE2NzkzNDMyNTB9.
rSsXDB_bSdkJvnF0vI5qP4ZgNzKzV8mHzU9Y7nGxUxA
```

### Níveis de Acesso

#### Usuário Comum
- Pode gerenciar próprio perfil
- Pode visualizar produtos
- Pode gerenciar próprio carrinho
- **Não pode** criar/editar/deletar produtos

#### Administrador
- Todas as permissões de usuário comum
- Pode criar, editar e deletar produtos
- Acesso completo à gestão do catálogo

### Endpoints Protegidos
- `PUT /usuarios/{_id}` - Requer autenticação
- `DELETE /usuarios/{_id}` - Requer autenticação
- `POST /produtos` - Requer admin
- `PUT /produtos/{_id}` - Requer admin
- `DELETE /produtos/{_id}` - Requer admin
- `POST /carrinhos` - Requer autenticação
- `PUT /carrinhos/{_id}` - Requer autenticação
- `DELETE /carrinhos/*` - Requer autenticação

---

## 📊 Regras de Negócio

### Usuários
1. **Email único**: Cada email pode estar associado a apenas um usuário
2. **Administrador único**: O sistema permite múltiplos administradores
3. **Carrinho obrigatório**: Usuários com carrinho não podem ser excluídos

### Produtos
1. **Nome único**: Cada produto deve ter nome exclusivo
2. **Estoque controlado**: Quantidade não pode ser negativa
3. **Carrinho protege**: Produtos em carrinho não podem ser excluídos
4. **Admin exclusivo**: Apenas administradores gerenciam produtos

### Carrinhos
1. **Um por usuário**: Cada usuário pode ter apenas um carrinho ativo
2. **Estoque automático**: Produtos são reservados automaticamente
3. **Validação de estoque**: Não permite adicionar mais que disponível
4. **Cancelamento restaura**: Cancelar compra devolve produtos ao estoque

### Autenticação
1. **Token expira**: JWT tem duração limitada de 600 segundos
2. **Bearer obrigatório**: Token deve ser enviado no header Authorization
3. **Validação contínua**: Cada requisição valida o token

---

## ⚠️ Limitações e Considerações

### Limitações Técnicas
- **Persistência**: Dados são armazenados em memória (reset periódico)
- **Rate Limiting**: Não implementado
- **CORS**: Configurado para permitir todas as origens
- **HTTPS**: Disponível apenas em HTTPS

### Considerações de Teste
- **Dados de teste**: Alguns usuários e produtos estão pré-cadastrados
- **Reset automático**: Base de dados é reiniciada periodicamente
- **Concorrência**: Múltiplos usuários podem afetar dados compartilhados

### Comportamentos Específicos
- **IDs únicos**: Sistema gera IDs automáticos para novos registros
- **Validation Messages**: Mensagens de erro em português
- **Case Sensitive**: Emails são case-sensitive
- **Numeric Types**: Preços e quantidades são tratados como números

---

## 🧪 Estratégia de Testes

### Testes Funcionais
1. **CRUD Completo**: Testar todas as operações para cada endpoint
2. **Validações**: Verificar todas as validações de entrada
3. **Regras de Negócio**: Validar cumprimento das regras estabelecidas
4. **Estados**: Testar diferentes estados da aplicação

### Testes de Segurança
1. **Autenticação**: Validar controle de acesso
2. **Autorização**: Verificar permissões por nível de usuário
3. **Input Validation**: Testar sanitização de entradas
4. **Token Management**: Verificar ciclo de vida dos tokens

### Testes de Integração
1. **Fluxos E2E**: Jornadas completas de usuário
2. **Consistência**: Integridade entre operações relacionadas
3. **Estado Compartilhado**: Impacto de operações concorrentes

### Testes de Performance
1. **Response Time**: Tempo de resposta de cada endpoint
2. **Throughput**: Capacidade de processamento
3. **Load Testing**: Comportamento sob carga
4. **Stress Testing**: Identificação de limites

---

## 📈 Métricas de Qualidade

### Disponibilidade
- **Uptime**: > 99%
- **Response Time**: < 2 segundos (P95)
- **Error Rate**: < 1%

### Funcionalidade
- **Coverage**: 100% dos endpoints
- **Business Rules**: 100% das regras implementadas
- **Validation**: 100% das validações testadas

### Segurança
- **Authentication**: 100% dos endpoints protegidos validados
- **Authorization**: 100% dos níveis de acesso testados
- **Input Validation**: 100% das entradas sanitizadas

---

## 🔧 Ferramentas Recomendadas

### Automação de Testes
- **Postman/Newman**: Collections automatizadas
- **REST Assured**: Testes em Java
- **Cypress**: Testes E2E com UI
- **K6**: Testes de performance

### Monitoramento
- **Datadog**: APM e métricas
- **New Relic**: Performance monitoring
- **Pingdom**: Uptime monitoring

### CI/CD
- **GitHub Actions**: Pipeline automatizado
- **Jenkins**: Integração contínua
- **Docker**: Containerização de testes

---

## 📚 Recursos Adicionais

### Documentação
- **Swagger UI**: https://compassuol.serverest.dev/
- **Repository**: https://github.com/ServeRest/ServeRest
- **NPM Package**: https://www.npmjs.com/package/serverest

### Comunidade
- **Issues**: Reportar problemas no GitHub
- **Discussions**: Dúvidas e melhorias
- **Contributing**: Contribuições são bem-vindas

### Exemplos
- **Postman Collections**: Exemplos na documentação
- **Code Samples**: Diferentes linguagens disponíveis
- **Tutorials**: Guias passo a passo

---

*Análise realizada em 19 de Agosto de 2025 - ServeRest API v2.29.7*