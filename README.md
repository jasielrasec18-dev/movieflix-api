# MovieFlix API

API REST para gerenciar um catálogo de filmes: listagem, cadastro, atualização, remoção e busca por gênero. Desenvolvida com Node.js, Express, TypeScript e Prisma ORM sobre PostgreSQL, com documentação interativa no Swagger.

## Funcionalidades

- Listagem de filmes em ordem alfabética, já com o gênero e o idioma de cada um.
- Cadastro de filmes com bloqueio de títulos duplicados (a comparação ignora maiúsculas e minúsculas).
- Atualização parcial de um filme pelo ID.
- Remoção de filmes pelo ID.
- Filtro de filmes pelo nome do gênero.
- Respostas de erro com mensagens em português e status HTTP adequados (`404`, `409` e `500`).
- Documentação OpenAPI 3 servida pelo Swagger UI.

## Tecnologias

| Tecnologia | Uso |
| --- | --- |
| Node.js e TypeScript | Execução e tipagem da aplicação |
| Express 5 | Servidor HTTP e rotas |
| Prisma 4 | Acesso ao banco de dados |
| PostgreSQL | Banco de dados |
| Swagger UI Express | Documentação interativa da API |
| tsx | Execução em modo de desenvolvimento com recarregamento |
| ESLint e Prettier | Padronização do código |

## Modelo de dados

O schema em `prisma/schema.prisma` possui três tabelas:

| Tabela | Campos |
| --- | --- |
| `movies` | `id`, `title`, `release_date`, `genre_id`, `language_id`, `oscar_count` |
| `genres` | `id`, `name` |
| `languages` | `id`, `name` |

Cada filme pode estar ligado a um gênero e a um idioma.

## Como rodar localmente

### Pré-requisitos

- Node.js e npm
- Um banco PostgreSQL acessível

### 1. Instale as dependências

```bash
git clone https://github.com/jasielrasec18-dev/movieflix-api.git
cd movieflix-api
npm install
```

### 2. Configure o banco de dados

Crie um arquivo `.env` na raiz com a conexão do PostgreSQL:

```dotenv
DATABASE_URL="postgresql://usuario:senha@localhost:5432/movieflix?schema=public"
```

O repositório não inclui migrations. Se o banco ainda não tiver as tabelas, crie-as a partir do schema e gere o Prisma Client:

```bash
npx prisma db push
npx prisma generate
```

Cadastre ao menos um gênero e um idioma (por exemplo, pelo `npx prisma studio`) antes de criar filmes, já que o cadastro recebe `genre_id` e `language_id`.

### 3. Inicie a API

```bash
npm run dev
```

- API: [http://localhost:3000](http://localhost:3000)
- Documentação: [http://localhost:3000/docs](http://localhost:3000/docs)

Para compilar e executar o build:

```bash
npm run build
npm start
```

## Scripts

| Comando | Descrição |
| --- | --- |
| `npm run dev` | Inicia o servidor com recarregamento automático |
| `npm run build` | Compila o TypeScript para a pasta `dist/` |
| `npm start` | Executa o código compilado |
| `npm run format` | Formata os arquivos com o Prettier |

## Rotas

| Método | Rota | Descrição | Respostas |
| --- | --- | --- | --- |
| `GET` | `/movies` | Lista todos os filmes | `200` |
| `POST` | `/movies` | Cadastra um filme | `201`, `409`, `500` |
| `PUT` | `/movies/:id` | Atualiza um filme | `200`, `404`, `500` |
| `DELETE` | `/movies/:id` | Remove um filme | `200`, `404`, `500` |
| `GET` | `/movies/:genreName` | Lista os filmes de um gênero | `200`, `500` |
| `GET` | `/docs` | Documentação Swagger | `200` |

### Exemplo de cadastro

`POST /movies` com `Content-Type: application/json`:

```json
{
  "title": "Cidade de Deus",
  "genre_id": 1,
  "language_id": 1,
  "oscar_count": 0,
  "release_date": "2002-08-30"
}
```

Se já existir um filme com o mesmo título, a API responde `409` com a mensagem "Já existe um filme cadastrado com esse título".

### Exemplo de atualização

`PUT /movies/1` aceita apenas os campos que devem mudar:

```json
{
  "oscar_count": 1
}
```

### Exemplo de filtro por gênero

```http
GET /movies/drama
```

A busca pelo nome do gênero não diferencia maiúsculas de minúsculas.

## Estrutura do projeto

```text
prisma/
  schema.prisma     # Modelos e relacionamentos do banco
src/
  server.ts         # Servidor Express, rotas e integração com o Prisma
swagger.json        # Especificação OpenAPI usada pelo Swagger UI
prisma.config.ts    # Configuração do Prisma
tsconfig.json       # Configuração do TypeScript
```

## Autor

Desenvolvido por **Jasiel Rasec** · [GitHub](https://github.com/jasielrasec18-dev)
