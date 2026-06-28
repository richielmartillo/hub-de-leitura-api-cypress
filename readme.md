# Hub de Leitura - API Teste Task

Projeto de automação de testes de API desenvolvido para validar funcionalidades do sistema **Hub de Leitura**.

Este projeto foi criado a partir dos testes manuais realizados anteriormente no **Postman**, adaptando os principais cenários para automação com **Cypress**.

O objetivo deste projeto é testar rotas relacionadas a **usuários** e **catálogo de livros**, aplicando boas práticas de organização, validação de respostas e estruturação dos testes automatizados.

---

## Tecnologias utilizadas

- **JavaScript**
- **Node.js**
- **Cypress**
- **Cypress Plugin API**
- **Postman**
- **Git & GitHub**

---

## Uso do Postman no projeto

O **Postman** foi utilizado no módulo anterior para realizar os testes manuais da API, validar requisições, analisar respostas e compreender o comportamento dos endpoints.

Com base nesses testes manuais, os cenários foram adaptados e automatizados neste projeto utilizando **Cypress**.

---

## Estrutura do projeto

```bash
cypress/
├── e2e/
│   ├── catalogoLivros.cy.js
│   └── usuarios.cy.js
├── fixtures/
├── support/
└── videos/
