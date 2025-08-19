# 📋 Documentação Completa - Casos de Teste API ServeRest

## 🎯 Objetivo

Este documento apresenta uma análise completa dos casos de teste para a API ServeRest (v2.29.7), baseada na documentação Swagger disponível em https://compassuol.serverest.dev/. A análise inclui testes funcionais, de segurança, integração e performance.

## 🏗️ Arquitetura da API

### Informações Gerais
- **URL Base**: `https://compassuol.serverest.dev`
- **Versão**: 2.29.7
- **Documentação**: OpenAPI 3.0.0
- **Autenticação**: JWT Bearer Token (duração 600 segundos)

### Módulos da API

1. **Login** - Autenticação de usuários
2. **Usuários** - Gestão de usuários do sistema
3. **Produtos** - Catálogo de produtos
4. **Carrinhos** - Carrinho de compras

## 🧪 Casos de Teste Detalhados

### Nomenclatura dos Casos de Teste
- **CT001-XXX**: Casos de teste funcionais
- **ST001-XXX**: Casos de teste de segurança
- **IT001-XXX**: Casos de teste de integração
- **PT001-XXX**: Casos de teste de performance

---

## 🔑 1. MÓDULO LOGIN

### Endpoint: POST /login

#### CT001-LOGIN - Login com Credenciais Válidas
**Objetivo**: Validar autenticação com usuário válido
**Dados de Entrada**:
```json
{
  "email": "fulano@qa.com",
  "password": "teste"
}
```
**Resultado Esperado**: 
- Status: 200 OK
- Response contém token JWT válido
- Token tem duração de 600 segundos

#### CT002-LOGIN - Login com Credenciais Inválidas
**Objetivo**: Validar tratamento de erro para credenciais incorretas
**Dados de Entrada**:
```json
{
  "email": "invalido@teste.com",
  "password": "senhaerrada"
}
```
**Resultado Esperado**:
- Status: 401 Unauthorized
- Mensagem: "Email e/ou senha inválidos"

#### ST001-LOGIN - Login com Dados Malformados
**Objetivo**: Validar sanitização de entrada
**Dados de Entrada**:
```json
{
  "email": "<script>alert('xss')</script>",
  "password": "'; DROP TABLE users; --"
}
```
**Resultado Esperado**:
- Status: 400 Bad Request
- Validação de formato de email

---

## 👥 2. MÓDULO USUÁRIOS

### GET /usuarios

#### CT003-USERS - Listar Todos os Usuários
**Objetivo**: Recuperar lista completa de usuários
**Resultado Esperado**:
- Status: 200 OK
- Array de usuários com campos obrigatórios
- Quantidade total de usuários

#### CT004-USERS - Listar Usuários com Paginação
**Objetivo**: Validar funcionalidade de paginação
**Query Parameters**: `?_limit=5&_page=1`
**Resultado Esperado**:
- Status: 200 OK
- Máximo 5 usuários retornados
- Headers de paginação

### GET /usuarios/{_id}

#### CT005-USERS - Buscar Usuário Existente por ID
**Objetivo**: Recuperar usuário específico
**Parâmetro**: ID válido de usuário
**Resultado Esperado**:
- Status: 200 OK
- Dados completos do usuário

#### CT006-USERS - Buscar Usuário Inexistente
**Objetivo**: Validar tratamento para ID não encontrado
**Parâmetro**: ID inexistente
**Resultado Esperado**:
- Status: 400 Bad Request
- Mensagem: "Usuário não encontrado"

### POST /usuarios

#### CT007-USERS - Cadastrar Usuário Válido
**Objetivo**: Criar novo usuário com dados válidos
**Dados de Entrada**:
```json
{
  "nome": "João Silva",
  "email": "joao.silva@teste.com",
  "password": "senha123",
  "administrador": "false"
}
```
**Resultado Esperado**:
- Status: 201 Created
- Usuário criado com ID único
- Mensagem de sucesso

#### CT008-USERS - Cadastrar Usuário com Email Duplicado
**Objetivo**: Validar unicidade de email
**Dados de Entrada**: Email já existente no sistema
**Resultado Esperado**:
- Status: 400 Bad Request
- Mensagem: "Este email já está sendo usado"

#### CT009-USERS - Cadastrar Usuário com Dados Incompletos
**Objetivo**: Validar campos obrigatórios
**Dados de Entrada**: JSON sem campo obrigatório
**Resultado Esperado**:
- Status: 400 Bad Request
- Detalhes dos campos faltantes

#### ST002-USERS - Validação de Formato de Email
**Objetivo**: Validar formato correto do email
**Dados de Entrada**:
```json
{
  "nome": "Teste",
  "email": "email-invalido",
  "password": "senha123",
  "administrador": "false"
}
```
**Resultado Esperado**:
- Status: 400 Bad Request
- Erro de validação de email

### PUT /usuarios/{_id}

#### CT010-USERS - Atualizar Usuário Existente
**Objetivo**: Modificar dados de usuário válido
**Parâmetro**: ID válido
**Headers**: `Authorization: Bearer {token}`
**Dados de Entrada**: Dados atualizados
**Resultado Esperado**:
- Status: 200 OK
- Dados atualizados
- Mensagem de sucesso

#### CT011-USERS - Atualizar Usuário Inexistente
**Objetivo**: Validar comportamento para ID inexistente
**Parâmetro**: ID inexistente
**Resultado Esperado**:
- Status: 201 Created (novo usuário criado)
- Ou 400 Bad Request dependendo da implementação

#### ST003-USERS - Atualização Sem Autorização
**Objetivo**: Validar controle de acesso
**Headers**: Token inválido ou ausente
**Resultado Esperado**:
- Status: 401 Unauthorized
- Mensagem de token requerido

### DELETE /usuarios/{_id}

#### CT012-USERS - Deletar Usuário Existente
**Objetivo**: Remover usuário do sistema
**Parâmetro**: ID válido
**Headers**: `Authorization: Bearer {token}`
**Resultado Esperado**:
- Status: 200 OK
- Usuário removido do sistema
- Mensagem de exclusão

#### CT013-USERS - Deletar Usuário com Carrinho
**Objetivo**: Validar regra de negócio
**Parâmetro**: ID de usuário com carrinho ativo
**Resultado Esperado**:
- Status: 400 Bad Request
- Mensagem: "Não é possível excluir usuário com carrinho cadastrado"

#### CT014-USERS - Deletar Usuário Inexistente
**Objetivo**: Validar comportamento para ID inexistente
**Parâmetro**: ID inexistente
**Resultado Esperado**:
- Status: 200 OK
- Mensagem informativa

---

## 📦 3. MÓDULO PRODUTOS

### GET /produtos

#### CT015-PRODUCTS - Listar Todos os Produtos
**Objetivo**: Recuperar catálogo completo
**Resultado Esperado**:
- Status: 200 OK
- Array de produtos com campos obrigatórios
- Quantidade total

#### CT016-PRODUCTS - Filtrar Produtos por Nome
**Objetivo**: Busca por nome do produto
**Query Parameters**: `?nome=Samsung`
**Resultado Esperado**:
- Status: 200 OK
- Produtos filtrados por nome

#### CT017-PRODUCTS - Filtrar Produtos por Preço
**Objetivo**: Busca por faixa de preço
**Query Parameters**: `?preco=470`
**Resultado Esperado**:
- Status: 200 OK
- Produtos com preço específico

### GET /produtos/{_id}

#### CT018-PRODUCTS - Buscar Produto Existente
**Objetivo**: Recuperar produto específico
**Parâmetro**: ID válido
**Resultado Esperado**:
- Status: 200 OK
- Dados completos do produto

#### CT019-PRODUCTS - Buscar Produto Inexistente
**Objetivo**: Validar tratamento para ID não encontrado
**Parâmetro**: ID inexistente
**Resultado Esperado**:
- Status: 400 Bad Request
- Mensagem: "Produto não encontrado"

### POST /produtos

#### CT020-PRODUCTS - Cadastrar Produto Válido
**Objetivo**: Criar novo produto
**Headers**: `Authorization: Bearer {admin_token}`
**Dados de Entrada**:
```json
{
  "nome": "Notebook Dell",
  "preco": 2500,
  "descricao": "Notebook para uso profissional",
  "quantidade": 10
}
```
**Resultado Esperado**:
- Status: 201 Created
- Produto criado com ID único

#### CT021-PRODUCTS - Cadastrar Produto com Nome Duplicado
**Objetivo**: Validar unicidade de nome
**Dados de Entrada**: Nome já existente
**Resultado Esperado**:
- Status: 400 Bad Request
- Mensagem: "Já existe produto com esse nome"

#### ST004-PRODUCTS - Cadastro Sem Permissão de Admin
**Objetivo**: Validar controle de acesso administrativo
**Headers**: Token de usuário comum
**Resultado Esperado**:
- Status: 403 Forbidden
- Mensagem: "Rota exclusiva para administradores"

#### CT022-PRODUCTS - Cadastrar com Dados Inválidos
**Objetivo**: Validar tipos de dados
**Dados de Entrada**:
```json
{
  "nome": "",
  "preco": -100,
  "descricao": "Teste",
  "quantidade": "texto"
}
```
**Resultado Esperado**:
- Status: 400 Bad Request
- Erros de validação detalhados

### PUT /produtos/{_id}

#### CT023-PRODUCTS - Atualizar Produto Existente
**Objetivo**: Modificar dados de produto
**Headers**: `Authorization: Bearer {admin_token}`
**Parâmetro**: ID válido
**Resultado Esperado**:
- Status: 200 OK
- Produto atualizado

#### CT024-PRODUCTS - Atualizar Produto Inexistente
**Objetivo**: Comportamento para ID inexistente
**Parâmetro**: ID inexistente
**Resultado Esperado**:
- Status: 201 Created
- Novo produto criado

### DELETE /produtos/{_id}

#### CT025-PRODUCTS - Deletar Produto Existente
**Objetivo**: Remover produto do catálogo
**Headers**: `Authorization: Bearer {admin_token}`
**Parâmetro**: ID válido
**Resultado Esperado**:
- Status: 200 OK
- Produto removido

#### CT026-PRODUCTS - Deletar Produto em Carrinho
**Objetivo**: Validar regra de negócio
**Parâmetro**: ID de produto em carrinho ativo
**Resultado Esperado**:
- Status: 400 Bad Request
- Mensagem: "Não é possível excluir produto que faz parte de carrinho"

---

## 🛒 4. MÓDULO CARRINHOS

### GET /carrinhos

#### CT027-CARTS - Listar Todos os Carrinhos
**Objetivo**: Recuperar todos os carrinhos ativos
**Resultado Esperado**:
- Status: 200 OK
- Array de carrinhos com produtos
- Quantidade total

#### CT028-CARTS - Filtrar Carrinho por Usuário
**Objetivo**: Buscar carrinho de usuário específico
**Query Parameters**: `?idUsuario={user_id}`
**Resultado Esperado**:
- Status: 200 OK
- Carrinho do usuário específico

### GET /carrinhos/{_id}

#### CT029-CARTS - Buscar Carrinho Existente
**Objetivo**: Recuperar carrinho específico
**Parâmetro**: ID válido
**Resultado Esperado**:
- Status: 200 OK
- Dados completos do carrinho

#### CT030-CARTS - Buscar Carrinho Inexistente
**Objetivo**: Validar tratamento para ID não encontrado
**Parâmetro**: ID inexistente
**Resultado Esperado**:
- Status: 400 Bad Request
- Mensagem: "Carrinho não encontrado"

### POST /carrinhos

#### CT031-CARTS - Cadastrar Carrinho Válido
**Objetivo**: Criar novo carrinho
**Headers**: `Authorization: Bearer {token}`
**Dados de Entrada**:
```json
{
  "produtos": [
    {
      "idProduto": "BeeJh5lz3k6kSIzA",
      "quantidade": 2
    }
  ]
}
```
**Resultado Esperado**:
- Status: 201 Created
- Carrinho criado para usuário autenticado

#### CT032-CARTS - Cadastrar Segundo Carrinho
**Objetivo**: Validar regra de um carrinho por usuário
**Headers**: Token de usuário com carrinho existente
**Resultado Esperado**:
- Status: 400 Bad Request
- Mensagem: "Não é permitido ter mais de 1 carrinho"

#### CT033-CARTS - Carrinho com Produto Inexistente
**Objetivo**: Validar produtos no carrinho
**Dados de Entrada**: ID de produto inexistente
**Resultado Esperado**:
- Status: 400 Bad Request
- Mensagem: "Produto não encontrado"

#### CT034-CARTS - Carrinho com Quantidade Insuficiente
**Objetivo**: Validar estoque disponível
**Dados de Entrada**: Quantidade maior que estoque
**Resultado Esperado**:
- Status: 400 Bad Request
- Mensagem: "Produto não possui quantidade suficiente"

#### ST005-CARTS - Carrinho Sem Autenticação
**Objetivo**: Validar necessidade de login
**Headers**: Sem token de autorização
**Resultado Esperado**:
- Status: 401 Unauthorized
- Mensagem: "Token de acesso ausente, inválido, expirado ou usuário do token não existe mais"

### PUT /carrinhos/{_id}

#### CT035-CARTS - Atualizar Carrinho Existente
**Objetivo**: Modificar produtos do carrinho
**Headers**: `Authorization: Bearer {token}`
**Parâmetro**: ID do carrinho
**Resultado Esperado**:
- Status: 200 OK
- Carrinho atualizado

### DELETE /carrinhos/concluir-compra

#### CT036-CARTS - Concluir Compra
**Objetivo**: Finalizar compra e limpar carrinho
**Headers**: `Authorization: Bearer {token}`
**Resultado Esperado**:
- Status: 200 OK
- Mensagem: "Registro excluído com sucesso"
- Carrinho removido do sistema

#### CT037-CARTS - Concluir Compra Sem Carrinho
**Objetivo**: Validar existência de carrinho
**Headers**: Token de usuário sem carrinho
**Resultado Esperado**:
- Status: 200 OK
- Mensagem: "Não foi encontrado carrinho para esse usuário"

### DELETE /carrinhos/cancelar-compra

#### CT038-CARTS - Cancelar Compra
**Objetivo**: Cancelar compra e devolver estoque
**Headers**: `Authorization: Bearer {token}`
**Resultado Esperado**:
- Status: 200 OK
- Estoque dos produtos restaurado
- Carrinho removido

---

## 🔄 5. CASOS DE TESTE DE INTEGRAÇÃO

### IT001-E2E - Fluxo Completo de Compra
**Objetivo**: Validar jornada completa do usuário
**Passos**:
1. Cadastrar novo usuário
2. Fazer login
3. Listar produtos disponíveis
4. Adicionar produtos ao carrinho
5. Concluir compra
**Resultado Esperado**: Fluxo executado sem erros

### IT002-ADMIN - Fluxo Administrativo
**Objetivo**: Validar operações de administrador
**Passos**:
1. Login como admin
2. Cadastrar novo produto
3. Atualizar produto existente
4. Verificar produtos em carrinhos
5. Deletar produto não utilizado
**Resultado Esperado**: Operações executadas com sucesso

### IT003-STOCK - Controle de Estoque
**Objetivo**: Validar gestão de estoque
**Passos**:
1. Verificar quantidade inicial do produto
2. Adicionar produto ao carrinho
3. Verificar redução do estoque
4. Cancelar compra
5. Verificar restauração do estoque
**Resultado Esperado**: Estoque gerenciado corretamente

### IT004-AUTH - Ciclo de Autenticação
**Objetivo**: Validar gestão de tokens
**Passos**:
1. Login com credenciais válidas
2. Usar token para operações autenticadas
3. Aguardar expiração do token (600s)
4. Tentar operação com token expirado
5. Renovar token
**Resultado Esperado**: Controle de autenticação funcionando

---

## ⚡ 6. CASOS DE TESTE DE PERFORMANCE

### PT001-RESPONSE - Tempo de Resposta
**Objetivo**: Validar performance dos endpoints
**Métricas**:
- GET /usuarios: < 500ms
- POST /login: < 1000ms
- GET /produtos: < 800ms
- POST /carrinhos: < 1200ms
**Resultado Esperado**: Todos os endpoints dentro do SLA

### PT002-LOAD - Teste de Carga Básico
**Objetivo**: Validar comportamento com múltiplas requisições
**Cenário**: 10 usuários simultâneos por 1 minuto
**Métricas**:
- Taxa de erro < 1%
- Tempo de resposta médio < 2000ms
- Throughput > 50 req/min
**Resultado Esperado**: Sistema estável sob carga

### PT003-STRESS - Teste de Stress
**Objetivo**: Identificar limite do sistema
**Cenário**: Incremento gradual de usuários até erro
**Métricas**:
- Ponto de falha
- Tempo de recuperação
- Degradação graceful
**Resultado Esperado**: Sistema se degrada de forma controlada

---

## 📊 Resumo de Cobertura

### Por Módulo
| Módulo | Casos Funcionais | Casos Segurança | Casos Integração | Total |
|--------|------------------|------------------|------------------|-------|
| Login | 2 | 1 | - | 3 |
| Usuários | 10 | 2 | - | 12 |
| Produtos | 12 | 1 | - | 13 |
| Carrinhos | 12 | 1 | - | 13 |
| Integração | - | - | 4 | 4 |
| Performance | - | - | 3 | 3 |
| **Total** | **36** | **5** | **7** | **48** |

### Por Tipo de Teste
- **Testes Funcionais**: 75% (36/48)
- **Testes de Segurança**: 10% (5/48)
- **Testes de Integração**: 15% (7/48)

### Cobertura de Endpoints
- **Total de Endpoints**: 16
- **Endpoints Testados**: 16 (100%)
- **Cenários por Endpoint**: Média de 3 cenários

---

## ✅ Critérios de Aceitação

### Funcionalidade
- [ ] Todos os endpoints retornam status codes corretos
- [ ] Estrutura de response está conforme documentação
- [ ] Regras de negócio são validadas corretamente
- [ ] Tratamento de erros é adequado

### Segurança
- [ ] Autenticação é obrigatória onde necessária
- [ ] Autorização é validada por nível de usuário
- [ ] Inputs são sanitizados adequadamente
- [ ] Tokens têm tempo de vida controlado

### Performance
- [ ] Endpoints respondem dentro do SLA definido
- [ ] Sistema suporta carga mínima especificada
- [ ] Não há vazamentos de memória ou recursos

### Usabilidade
- [ ] Mensagens de erro são claras e úteis
- [ ] Documentação da API está atualizada
- [ ] Exemplos de uso estão corretos

---

## 🔧 Ferramentas de Teste

### Automação
- **Postman/Newman**: Execução de collection
- **GitHub Actions**: CI/CD pipeline
- **HTML Reporter**: Relatórios detalhados

### Monitoramento
- **Response Time**: Validação automática
- **Status Codes**: Verificação contínua
- **Data Validation**: Schema validation

### Relatórios
- **HTML**: Relatórios visuais
- **JSON**: Dados estruturados
- **XML**: Integração com ferramentas CI/CD

---

*Documento gerado como parte da análise automatizada da API ServeRest v2.29.7*

*Última atualização: 19 de Agosto de 2025*