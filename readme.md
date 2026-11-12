# Hub de Leitura | Automação de Testes de API com Cypress

[![Cypress](https://img.shields.io/badge/Cypress-15.9.0-17202C?logo=cypress&logoColor=white)](https://www.cypress.io/) [![JavaScript](https://img.shields.io/badge/JavaScript-ES6%2B-F7DF1E?logo=javascript&logoColor=black)](https://developer.mozilla.org/docs/Web/JavaScript)

Projeto de automação de testes de API para o sistema **Hub de Leitura**. A suíte foi desenvolvida em JavaScript com Cypress e valida as operações de gestão de usuários e do catálogo de livros, incluindo respostas de sucesso, regras de validação e estrutura dos dados retornados.

## Objetivo da automação

Verificar os principais endpoints da API do Hub de Leitura de forma automatizada, exercitando o fluxo de autenticação e as operações de consulta, criação, atualização e remoção de usuários e livros.

## Funcionalidades e fluxos testados

### Gestão de usuários

- Autenticação pelo endpoint `POST /login` e utilização do token retornado no cabeçalho `Authorization`.
- Listagem de usuários e validação de array e propriedades `id`, `name` e `email`.
- Consulta de usuário por ID (`GET /users/2`).
- Consulta com paginação e busca (`page`, `limit` e `search`).
- Criação de usuário com e-mail gerado dinamicamente.
- Validação de erro para e-mail em formato inválido.
- Atualização de um usuário por ID fixo e de um usuário criado durante o próprio cenário.
- Exclusão de usuário criado durante o cenário.

### Catálogo de livros

- Listagem de livros e validação de array e propriedades `id`, `title` e `author`.
- Consulta de livro por ID obtido dinamicamente a partir da listagem.
- Listagem de autores em `GET /books/authors`.
- Filtros por título, categoria e autor; além de `limit` e `offset`.
- Criação de livro a partir da fixture `livros.json`, com título, autor e ISBN dinâmicos.
- Validação de erro para envio de título vazio.
- Atualização e exclusão de livros criados no próprio teste.

## Tipos de testes realizados

- Testes funcionais de API.
- Cenários positivos e negativos.
- Validações de status HTTP, mensagens de resposta, tipos de dados e propriedades do payload.
- Fluxos com dados dinâmicos para reduzir colisões entre execuções e permitir operações subsequentes sobre os recursos criados.

## Métodos HTTP cobertos

| Método | Endpoints exercitados |
| --- | --- |
| `GET` | `/users`, `/users/2`, `/books`, `/books/{id}`, `/books/authors` |
| `POST` | `/login`, `/users`, `/books` |
| `PUT` | `/users/{id}`, `/books/{id}` |
| `DELETE` | `/users/{id}`, `/books/{id}` |

## Tecnologias e ferramentas

- JavaScript
- Node.js e npm
- [Cypress 15.9.0](https://www.cypress.io/)
- [cypress-plugin-api 2.11.2](https://www.npmjs.com/package/cypress-plugin-api)
- Visual Studio Code
- Git e GitHub

## Estrutura principal

```text
.
├── cypress/
│   ├── e2e/
│   │   ├── catalogoLivros.cy.js    # cenários do catálogo
│   │   └── usuarios.cy.js          # cenários de usuários
│   ├── fixtures/
│   │   ├── livros.json             # massas válida e inválida de livro
│   │   └── example.json            # fixture de exemplo do Cypress
│   ├── support/
│   │   ├── commands.js             # comandos customizados
│   │   └── e2e.js                  # carregamento de suporte e plugin
│   └── videos/                     # vídeos gerados pelo Cypress
├── cypress.config.js               # baseUrl, projectId e vídeo
├── package.json
└── README.md
```

## Pré-requisitos

- Node.js e npm instalados.
- Google Chrome instalado, pois os scripts do projeto o definem como navegador de execução.
- API do Hub de Leitura em execução e acessível em `http://localhost:3000/api/`.

## Instalação

No PowerShell, clone o repositório e entre na pasta do projeto:

```powershell
git clone https://github.com/richielmartillo/hub-de-leitura-api-cypress.git
Set-Location hub-de-leitura-api-cypress
```

Instale as dependências:

```powershell
npm install
```

## Configuração da API

A suíte usa a `baseUrl` definida em [`cypress.config.js`](cypress.config.js):

```text
http://localhost:3000/api/
```

Antes de executar os testes, inicie a API do Hub de Leitura de modo que ela esteja disponível nesse endereço. Este repositório contém somente a automação: não inclui o código-fonte da API, arquivo de contêiner ou script npm para iniciá-la; portanto, o comando de inicialização deve ser obtido no projeto da API.

Os testes autenticam-se com as credenciais cadastradas no código (`admin@biblioteca.com` / `admin123`). O token retornado por `POST /login` é obtido novamente em cada `beforeEach` e usado nas requisições protegidas. Não há variáveis de ambiente configuradas no projeto.

## Execução dos testes

### Modo interativo

Com a API em execução, abra o Cypress:

```powershell
npx cypress open --e2e --browser chrome
```

### Modo headless

Execute o script disponível no projeto:

```powershell
npm test
```

Ele corresponde a `npx cypress run --browser chrome`.

## Scripts npm disponíveis

| Script | Comando configurado | Finalidade |
| --- | --- | --- |
| `npm test` | `npx cypress run --browser chrome` | Executa a suíte no Chrome. |
| `npm run cy:report` | `npx cypress run --browser chrome --record --key ...` | Executa a suíte com a opção de gravação do Cypress configurada no `package.json`. |

> O projeto possui `projectId` e uma chave de gravação no script `cy:report`, mas este README não pressupõe a disponibilidade de um serviço externo, relatórios publicados ou resultados de execução.

## Comandos customizados e dados de teste

Os comandos em `cypress/support/commands.js` centralizam ações reutilizáveis:

- `cy.geraToken(email, senha)`: realiza login e retorna o token da resposta.
- `cy.cadastrarUsuario(nome, email, senha)`: cria um usuário e retorna seu ID.
- `cy.criarLivro(title, token, author)`: cria um livro e retorna seu ID.
- `cy.atualizarLivro(token, bookId, dadosDoLivro)`: envia uma atualização de livro.

As massas de livros ficam em `cypress/fixtures/livros.json`. Valores baseados em `Date.now()` são usados em e-mails, títulos, autores e ISBNs para os cenários de criação e para encadear operações de atualização ou exclusão com o ID retornado pela API.

## Boas práticas observadas

- Separação dos cenários por domínio e método HTTP.
- Reuso de autenticação e criação de usuário por comandos customizados.
- Uso de fixture para concentrar massas válidas e inválidas de livros.
- Asserções sobre código de status, mensagens, propriedades e tipos de dados.
- Uso de `failOnStatusCode: false` nos cenários que verificam respostas de erro esperadas.
- Criação de recursos no próprio teste antes de atualizá-los ou excluí-los.

## Possíveis melhorias futuras

As sugestões abaixo não fazem parte da implementação atual:

- Mover credenciais e chave de gravação para variáveis de ambiente.
- Criar comandos para os fluxos de livros já padronizados em `commands.js`.
- Adicionar cenários de autorização, recursos inexistentes e demais regras de validação da API.
- Implementar limpeza explícita para recursos de teste quando aplicável ao ambiente da API.
- Documentar ou versionar o projeto da API necessário para a execução local.


