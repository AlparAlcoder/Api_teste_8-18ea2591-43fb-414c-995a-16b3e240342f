# Documentação API FastAPI

Este software é uma API simples construída com FastAPI e SQLAlchemy. Ela permite a criação, autenticação e recuperação de informações de usuários. A autenticação é feita utilizando JWT (JSON Web Tokens).

## Dependências

- fastapi
- sqlalchemy
- pydantic
- jose
- passlib

## Classes

### User

Um modelo SQLAlchemy que representa um usuário.

- id (Integer): O ID exclusivo do usuário.
- name (String): O nome do usuário.
- email (String): O e-mail do usuário.
- password (String): A senha do usuário.

### UserIn

Um modelo Pydantic que representa os dados de entrada para a criação de um novo usuário.

- name (str): O nome do usuário.
- email (str): O e-mail do usuário.
- password (str): A senha do usuário.

### TokenData

Um modelo Pydantic que representa os dados contidos em um token JWT.

- username (Optional[str]): O nome do usuário.

## Funções

### get_db

Retorna uma nova sessão SQLAlchemy.

### get_user(db: Session, user_id: int)

Recupera um usuário pelo seu ID.

- db (Session): A sessão SQLAlchemy atual.
- user_id (int): O ID do usuário a ser recuperado.

### authenticate_user(db: Session, username: str, password: str)

Autentica um usuário com base em um nome de usuário e senha.

- db (Session): A sessão SQLAlchemy atual.
- username (str): O nome de usuário a ser autenticado.
- password (str): A senha do usuário a ser autenticada.

### create_access_token(data: dict, expires_delta: Optional[timedelta] = None)

Cria um novo token JWT.

- data (dict): Os dados a serem inclusos no token.
- expires_delta (Optional[timedelta]): O tempo até o token expirar.

## Rotas

### POST /token

Autentica um usuário e retorna um token JWT.

- form_data (OAuth2PasswordRequestForm): Os dados do formulário contendo o nome do usuário e a senha.

### POST /users/

Cria um novo usuário.

- user (UserIn): Os dados do usuário a serem criados.

### GET /users/{user_id}

Recupera um usuário pelo seu ID.

- user_id (int): O ID do usuário a ser recuperado.

## Exemplos de uso

Para criar um novo usuário:

```bash
curl -X POST "http://localhost:8000/users/" -H  "accept: application/json" -H  "Content-Type: application/json" -d "{\"name\":\"user1\",\"email\":\"user1@example.com\",\"password\":\"password1\"}"
```

Para autenticar um usuário:

```bash
curl -X POST "http://localhost:8000/token" -H  "accept: application/json" -H  "Content-Type: application/x-www-form-urlencoded" -d "username=user1&password=password1"
```

Para recuperar um usuário:

```bash
curl -X GET "http://localhost:8000/users/1" -H  "accept: application/json"
```

## Notas importantes

- Certifique-se de substituir "YOUR_SECRET_KEY" por uma chave secreta real em um ambiente de produção.
- As senhas são armazenadas em texto simples para simplificar o exemplo. Em um ambiente de produção, certifique-se de armazenar senhas de maneira segura.
- A API não implementa a funcionalidade completa de CRUD. Isso também é feito para simplificar o exemplo. Em um ambiente de produção, você provavelmente desejará adicionar rotas para atualizar e excluir usuários.