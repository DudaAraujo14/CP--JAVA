# 🧙‍♂️ MTG Deck Builder API

# 1. Introdução
Esta documentação descreve a API RESTful para um sistema de criação e gerenciamento de decks de Magic: The Gathering (MTG). O objetivo desta API é permitir que usuários criem, visualizem, editem e gerenciem seus decks de MTG de forma eficiente. Inspirado em plataformas como Moxfield e Archidekt, este sistema visa fornecer uma experiência completa para os entusiastas de MTG organizarem suas coleções de decks.

## 2. Rotas da API
A tabela abaixo apresenta as principais rotas disponíveis na API, incluindo os métodos HTTP, descrições e códigos de status esperados
# API de Gerenciamento de Decks e Cartas

Esta API permite gerenciar decks de cartas, cartas individuais e usuários, além de listar formatos de jogo. Abaixo estão os endpoints disponíveis e seus respectivos códigos de status de resposta:

## Endpoints de Decks (/decks)

| Método | Rota        | Descrição                  | Códigos de Status |
|--------|-------------|-----------------------------|-------------------|
| GET    | /decks      | Listar todos os decks       | 200, 500          |
| GET    | /decks/{id} | Obter detalhes de um deck específico | 200, 404, 500     |
| POST   | /decks      | Criar um novo deck          | 201, 400, 500     |
| PUT    | /decks/{id} | Atualizar um deck existente | 200, 404, 500     |
| DELETE | /decks/{id} | Excluir um deck             | 204, 404, 500     |

## Endpoints de Cartas em um Deck (/decks/{id}/cards)

| Método | Rota                 | Descrição                      | Códigos de Status |
|--------|----------------------|---------------------------------|-------------------|
| GET    | /decks/{id}/cards    | Listar cartas de um deck       | 200, 500          |
| POST   | /decks/{id}/cards    | Adicionar cartas a um deck      | 201, 400, 500     |
| PUT    | /decks/{id}/cards/{cardId} | Atualizar uma carta em um deck | 200, 400, 500     |
| DELETE | /decks/{id}/cards/{cardId} | Remover uma carta de um deck   | 204, 404, 500     |

## Endpoints de Cartas (/cards)

| Método | Rota        | Descrição                   | Códigos de Status |
|--------|-------------|------------------------------|-------------------|
| GET    | /cards      | Listar todas as cartas disponíveis | 200, 500          |
| GET    | /cards/{id} | Obter detalhes de uma carta específica | 200, 404, 500     |
| POST   | /cards      | Adicionar uma nova carta     | 201, 400, 500     |
| PUT    | /cards/{id} | Atualizar informações de uma carta | 200, 400, 500     |
| DELETE | /cards/{id} | Excluir uma carta            | 204, 404, 500     |

## Endpoints de Usuários (/users)

| Método | Rota        | Descrição                   | Códigos de Status |
|--------|-------------|------------------------------|-------------------|
| GET    | /users      | Listar todos os usuários      | 200, 500          |
| GET    | /users/{id} | Obter detalhes de um usuário específico | 200, 404, 500     |
| POST   | /users      | Criar um novo usuário        | 201, 400, 500     |
| PUT    | /users/{id} | Atualizar informações de um usuário | 200, 400, 500     |
| DELETE | /users/{id} | Excluir um usuário           | 204, 404, 500     |

## Endpoints de Formatos de Jogo (/formats)

| Método | Rota           | Descrição                     | Códigos de Status |
|--------|----------------|--------------------------------|-------------------|
| GET    | /formats       | Listar todos os formatos de jogo | 200, 500          |
| GET    | /formats/{id}  | Obter detalhes de um formato específico | 200, 404, 500     |

**Observação sobre Códigos de Status:**

* **200 OK:** A requisição foi bem-sucedida.
* **201 Created:** Um novo recurso foi criado com sucesso.
* **204 No Content:** A requisição foi bem-sucedida, mas não há conteúdo para retornar.
* **400 Bad Request:** A requisição era inválida ou malformada.
* **404 Not Found:** O recurso solicitado não foi encontrado.
* **500 Internal Server Error:** Ocorreu um erro no servidor ao processar a requisição.

# 3. DTOs e Modelos de Dados
Descrições detalhadas dos Data Transfer Objects (DTOs) utilizados nas requisições e respostas da API.

## DeckDTO (Resposta)

```json
{
  "id": 123,
  "nome": "Meu Deck Aggro",
  "formato": "Standard",
  "descricao": "Um deck agressivo focado em criaturas rápidas.",
  "cartas": [
    {
      "id": 456,
      "nome": "Lobo Veloz",
      "tipo": "Criatura — Lobo",
      "cor": "Verde",
      "custoMana": "{G}",
      "quantidade": 4
    },
    {
      "id": 789,
      "nome": "Raio",
      "tipo": "Mágica Instantânea",
      "cor": "Vermelho",
      "custoMana": "{R}",
      "quantidade": 4
    }
  ],
  "dataCriacao": "2024-07-26T10:00:00Z",
  "dataAtualizacao": "2024-07-26T10:30:00Z",
  "criadorId": 1,
  "publico": false
}
```

## Atributos do DeckDTO (Resposta)

* `id` (integer, obrigatório): Identificador único do deck.
* `nome` (string, obrigatório): Nome do deck.
* `formato` (string, obrigatório): Formato do deck (ex: Standard, Modern, Commander).
* `descricao` (string, opcional): Descrição detalhada do deck.
* `cartas` (array, obrigatório): Lista de cartas no deck. Cada item é um objeto com as seguintes propriedades:
    * `id` (integer, obrigatório): Identificador único da carta.
    * `nome` (string, obrigatório): Nome da carta.
    * `tipo` (string, obrigatório): Tipo da carta.
    * `cor` (string, obrigatório): Cor da carta.
    * `custoMana` (string, obrigatório): Custo de mana da carta.
    * `quantidade` (integer, obrigatório): Quantidade desta carta no deck.
* `dataCriacao` (string, obrigatório, formato ISO 8601): Data e hora de criação do deck.
* `dataAtualizacao` (string, obrigatório, formato ISO 8601): Data e hora da última atualização do deck.
* `criadorId` (integer, obrigatório): Identificador do usuário que criou o deck.
* `publico` (boolean, obrigatório): Indica se o deck é público (visível para outros usuários).

# CriarDeckDTO (Requisição)
```json
{
  "nome": "Novo Deck Controle",
  "formato": "Modern",
  "descricao": "Um deck de controle com muitas remoções e counters.",
  "cartas": [
    {
      "cardId": 101,
      "quantidade": 4
    },
    {
      "cardId": 102,
      "quantidade": 3
    }
  ]
}
```
## Atributos do CriarDeckDTO (Requisição)

* `nome` (string, obrigatório): Nome do novo deck.
* `formato` (string, obrigatório): Formato do novo deck.
* `descricao` (string, opcional): Descrição do novo deck.
* `cartas` (array, opcional): Lista de cartas a serem adicionadas ao deck na criação. Cada item é um objeto com `cardId` (integer, obrigatório) e `quantidade` (integer, obrigatório).

# AtualizarDeckDTO (Requisição)
```json
{
  "nome": "Deck Aggro Atualizado",
  "descricao": "Versão melhorada do deck aggro inicial."
}
```
## Atributos:

* `nome` (string, opcional): Novo nome do deck.
* `descricao` (string, opcional): Nova descrição do deck.
* `formato` (string, opcional): Novo formato do deck.

# CartaNoDeckDTO (Resposta - dentro de DeckDTO)
```json
{
  "id": 456,
  "nome": "Lobo Veloz",
  "tipo": "Criatura — Lobo",
  "cor": "Verde",
  "custoMana": "{G}",
  "quantidade": 4
}
```
## Atributos do CartaNoDeckDTO (Resposta - dentro de DeckDTO)

* `id` (integer, obrigatório): Identificador único da carta.
* `nome` (string, obrigatório): Nome da carta.
* `tipo` (string, obrigatório): Tipo da carta.
* `cor` (string, obrigatório): Cor da carta.
* `custoMana` (string, obrigatório): Custo de mana da carta.
* `quantidade` (integer, obrigatório): Quantidade desta carta no deck.

# AdicionarCartaAoDeckDTO (Requisição)
```json
{
  "cardId": 103,
  "quantidade": 2
}
```
```json
[
  {
    "cardId": 104,
    "quantidade": 1
  },
  {
    "cardId": 105,
    "quantidade": 3
  }
]
```

## Atributos do AdicionarCartaAoDeckDTO (Requisição)

* `cardId` (integer, obrigatório): Identificador único da carta a ser adicionada.
* `quantidade` (integer, obrigatório): Quantidade da carta a ser adicionada.

# UsuarioDTO (Resposta)
```json
{
  "id": 1,
  "nome": "João Silva",
  "email": "joao.silva@email.com",
  "dataRegistro": "2024-07-20T08:00:00Z"
}
```

## Atributos do UsuarioDTO (Resposta)

* `id` (integer, obrigatório): Identificador único do usuário.
* `nome` (string, obrigatório): Nome do usuário.
* `email` (string, obrigatório): Endereço de email do usuário.
* `dataRegistro` (string, obrigatório, formato ISO 8601): Data e hora de registro do usuário.

# RegistrarUsuarioDTO (Requisição)
```json
{
  "nome": "Maria Souza",
  "email": "maria.souza@email.com",
  "senha": "senha123"
}
```
## Atributos do RegistrarUsuarioDTO (Requisição)

* `nome` (string, obrigatório): Nome do novo usuário.
* `email` (string, obrigatório): Endereço de email do novo usuário.
* `senha` (string, obrigatório): Senha do novo usuário.

# LoginUsuarioDTO (Requisição)
```json
{
  "email": "joao.silva@email.com",
  "senha": "senha123"
}
```

## Atributos do LoginUsuarioDTO (Requisição)

* `email` (string, obrigatório): Endereço de email do usuário.
* `senha` (string, obrigatório): Senha do usuário.

# FormatoDTO (Resposta)
```json
{
  "id": 1,
  "nome": "Standard",
  "descricao": "Formato com as coleções mais recentes.",
  "regras": "Lista de regras específicas do formato."
}
```

## Atributos do FormatoDTO (Resposta)

* `id` (integer, obrigatório): Identificador único do formato.
* `nome` (string, obrigatório): Nome do formato.
* `descricao` (string, opcional): Descrição do formato.
* `regras` (string, opcional): Detalhes das regras do formato.

# CartaDTO (Resposta)
```json
{
  "id": 456,
  "nome": "Lobo Veloz",
  "tipo": "Criatura — Lobo",
  "cor": "Verde",
  "custoMana": "{G}",
  "texto": "Aranha com alcance.",
  "poder": "2",
  "resistencia": "2",
  "raridade": "Comum",
  "set": "XLN"
}
```

## Atributos do CartaDTO (Resposta)

* `id` (integer, obrigatório): Identificador único da carta.
* `nome` (string, obrigatório): Nome da carta.
* `tipo` (string, obrigatório): Tipo da carta.
* `cor` (string, obrigatório): Cor da carta.
* `custoMana` (string, obrigatório): Custo de mana da carta.
* `texto` (string, opcional): Texto da carta (habilidades, etc.).
* `poder` (string, opcional): Poder da criatura (se aplicável).
* `resistencia` (string, opcional): Resistência da criatura (se aplicável).
* `raridade` (string, obrigatório): Raridade da carta.
* `set` (string, obrigatório): Código do set da carta.

  
# CorDTO (Resposta)
```json
{
  "id": 1,
  "nome": "Branco",
  "codigo": "W"
}
```

## Atributos do CorDTO (Resposta)

* `id` (integer, obrigatório): Identificador único da cor.
* `nome` (string, obrigatório): Nome da cor.
* `codigo` (string, obrigatório): Código da cor (ex: W, U, B, R, G).

# CardSetDTO (Resposta)
```json
{
  "id": 1,
  "codigo": "XLN",
  "nome": "Ixalan",
  "dataLancamento": "2017-09-29"
}
```

## Atributos do CardSetDTO (Resposta)

* `id` (integer, obrigatório): Identificador único do set.
* `codigo` (string, obrigatório): Código do set.
* `nome` (string, obrigatório): Nome do set.
* `dataLancamento` (string, obrigatório, formato YYYY-MM-DD): Data de lançamento do set.

# 4. Especificação OpenAPI
## Especificação OpenAPI para API de Construção de Decks de MTG

```yaml
openapi: 3.0.0
info:
  title: API de Construção de Decks de MTG
  version: v1
  description: API para criar e gerenciar decks de Magic: The Gathering.
servers:
  - url: http://localhost:8080/api # Substitua pela URL real da API
    description: Servidor de desenvolvimento

components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT

  schemas:
    Deck:
      type: object
      properties:
        id:
          type: integer
          description: Identificador único do deck.
          readOnly: true
        nome:
          type: string
          description: Nome do deck.
        formato:
          type: string
          description: Formato do deck.
        descricao:
          type: string
          nullable: true
          description: Descrição detalhada do deck.
        cartas:
          type: array
          items:
            $ref: '#/components/schemas/CartaNoDeck'
          description: Lista de cartas no deck.
        dataCriacao:
          type: string
          format: date-time
          readOnly: true
          description: Data e hora de criação do deck.
        dataAtualizacao:
          type: string
          format: date-time
          readOnly: true
          description: Data e hora da última atualização do deck.
        criadorId:
          type: integer
          readOnly: true
          description: Identificador do usuário que criou o deck.
        publico:
          type: boolean
          description: Indica se o deck é público.
      required:
        - id
        - nome
        - formato
        - cartas
        - dataCriacao
        - dataAtualizacao
        - criadorId
        - publico

    CriarDeck:
      type: object
      properties:
        nome:
          type: string
          description: Nome do novo deck.
        formato:
          type: string
          description: Formato do novo deck.
        descricao:
          type: string
          nullable: true
          description: Descrição do novo deck.
        cartas:
          type: array
          items:
            type: object
            properties:
              cardId:
                type: integer
                description: Identificador único da carta.
              quantidade:
                type: integer
                description: Quantidade da carta.
            required:
              - cardId
              - quantidade
      required:
        - nome

## 📑 Arquivo OpenAPI

A especificação completa da API está disponível no arquivo `openapi.yaml`.

Você pode validar a especificação usando o [Swagger Editor](https://editor.swagger.io).

