# Docker compose com Django e banco de dados

## Informações gerais

- Assunto: Docker, conteinizar aplicativos
- Disciplina: *sistemas operacionais*
- **Tarefa**:
  1. Criar um projeto django com uma aplicação web
    - alternativa, criar uma _branch_ no repositório do projeto integrador para configurar o acesso ao repositório de dados
  2. Criar um `Dockerfile` para o projeto django
  3. Criar a imagem e testar o conteiner para testar
  4. OPCIONAL, porque dependendo de como pergunta ao assistente de IA; criar um `Dockerfile` para o repositório de banco de dados
  5. Criar um `docker-compose.yml` e configurar para 2 serviços: `webapp` e `db`
  6. Configurar o arquivo django de acesso ao repositório de dados para usar o serviço docker `db`
  7. Testar o `docker-compose.yml`
  8. Relatar minimamente o que foi feito.
- **Entrega**: copia desse aquivo markdown preenchido, no seu repositório _fork_ de https://github.com/sistemas-operacionais/2024.2


## Relatório

### Aluno

- nome: [Gustavo Henrique da Cruz Maciel]
- matrícula: [20232014040003]

### Relato

Foi necessário realizar a configuração de um ambiente Docker para o projeto Django, além de configurar a comunicação entre o serviço web. Passo a passo:

1. **Criação do projeto Django**: O projeto foi iniciado e uma aplicação web foi criada dentro do Django, com a estrutura básica configurada.

2. **Dockerfile**: criei um `Dockerfile` para o projeto Django, que contém as instruções necessárias para configurar o contêiner com o ambiente Python e instalar as dependências do projeto.

3. **Testes de contêiner**: A imagem foi construída com sucesso e os testes iniciais do contêiner foram realizados para garantir que o Django estivesse funcionando corretamente.

4. **Configuração do banco de dados**: Acabei pulando a criação `Dockerfile`.

5. **docker-compose.yml**: O arquivo foi configurado para permitir a execução de dois serviços, o `webapp` (Django) e o `db` (banco de dados), e a comunicação entre eles foi testada.

6. **Testes de docker-compose**: O `docker-compose` foi testado mas tive dúvida em relação a bugs, com o Django não conseguindo acessar o banco de dados.

### Arquivos docker e de configuração do django

**Dockerfile para Django**:
```
FROM python:3.10

WORKDIR /app

COPY . .

RUN pip install --no-cache-dir -r requirements.txt

EXPOSE 8000

CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```



**Docker-compose.yml**:
```
version: "3.8"

services:
  webapp:
    build: .
    ports:
      - "8000:8000"
    depends_on:
      - db
    environment:
      - DATABASE_URL=postgres://user:password@db:5432/mydatabase

  db:
    image: postgres:13
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: mydatabase
    volumes:
      - db_data:/var/lib/postgresql/data

volumes:
  db_data:
```



**Settings.py**:
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'mydatabase',
        'USER': 'user',
        'PASSWORD': 'password',
        'HOST': 'db',  
        'PORT': '5432',
    }
}
```
