## Instruções sobre "README-CANDIDATO" (Timebox 30min):
Preencha este arquivo com informações claras e concisas, separadas pelas seguintes seções:

#### Seção 1: Instruções para rodar

- Quais variáveis de ambiente são necessárias?

# .env (opcional para desenvolvimento)
DEBUG=True
SECRET_KEY=sua-chave-secreta-aqui
ALLOWED_HOSTS=localhost,127.0.0.1

Observação: O projeto foi configurado para rodar com valores padrão, então as variáveis de ambiente são opcionais para desenvolvimento local. Em produção, recomendo configurar SECRET_KEY e DEBUG=False.

- Como instalar dependências?

Opção A: Com Docker (recomendado):

# Todas as dependências estão no Dockerfile
docker-compose build

Opção B: Manualmente (sem Docker):

# Crie e ative ambiente virtual
python -m venv venv

# Linux/Mac:
source venv/bin/activate

# Windows:
venv\Scripts\activate

# Instale dependências
pip install -r requirements.txt

- Como rodar o projeto?

Com Docker Compose (método principal):

# 1. Suba os containers
docker-compose up -d

# 2. Execute as migrações (se não rodar automaticamente)
docker-compose exec web python manage.py migrate

# 3. Crie um superusuário (opcional, para acessar o admin)
docker-compose exec web python manage.py createsuperuser

# 4. Acesse a aplicação:
#    - API: http://localhost:8000/api/
#    - Admin: http://localhost:8000/admin/
#    - Swagger: http://localhost:8000/api/docs/

# 5. Para parar:
docker-compose down

Sem Docker:

# 1. Execute migrações
python manage.py migrate

# 2. Execute o servidor
python manage.py runserver

# 3. Acesse: http://localhost:8000


#### Seção 2: Decisões de design

- Qual foi a maior dificuldade que você encontrou e como superou?

A maior dificuldade foi implementar o bônus de upload de imagens para o S3 AWS dentro do timebox de 4 horas. Embora tenha conseguido adicionar o campo ImageField ao modelo Product, a configuração completa com AWS S3 apresentou vários desafios.
Aprendizado principal: A integração com serviços externos como AWS requer não apenas código, mas também documentação clara e configuração de ambiente robusta. Decidi focar em fazer uma implementação que funcione localmente enquanto mantém a estrutura pronta para produção.

- O que você não teve tempo de fazer (dentro do timebox) e como você faria se tivesse mais tempo?

Com mais tempo, tentaria inclementar umas Melhorias na API, como:

Paginação otimizada com cursor-based pagination para grandes conjuntos de dados;
Filtros avançados usando django-filter para busca por intervalo de preço, categoria, etc;
Cache com Redis para endpoints frequentemente acessados;
Rate limiting para prevenir abuso da API.

#### Seção 3: Link para Deploy (Bônus)

- Cole aqui o link do projeto hospedado ou instrua como rodar via Docker.

https://github.com/coder-marllon/simple-resource-api.git


Como rodar via Docker :

Método 1: Com Docker Compose

# 1. Clone o repositório
git clone https://github.com/coder-marllon/simple-resource-api.git
cd simple-resource-api

# 2. Construa e execute os containers
docker-compose up --build

# 3. Ou para rodar em segundo plano:
docker-compose up -d --build

# 4. Acesse a aplicação em:
#    - API e Swagger: http://localhost:8000/api/docs/
#    - Admin Django: http://localhost:8000/admin/

Apenas Docker (sem docker-compose)

# 1. Construa a imagem
docker build -t simple-resource-api .

# 2. Execute o container
docker run -p 8000:8000 \
  -v $(pwd)/db.sqlite3:/app/db.sqlite3 \
  -v $(pwd)/media:/app/media \
  -e DEBUG=1 \
  simple-resource-api

# 3. Execute migrações (em outro terminal)
docker exec -it <container_id> python manage.py migrate

#### Seção final: Recomendações
- Escreva aqui dicas, melhorias e recomendações sobre este desafio.

Algumas melhorias que pensei foram :

Adicionar paginação nas listagens da API;

Implementar filtros por categoria, preço e nome.

Porém, achei o desafio muito bom da maneira que está. Desde já quero agradecer a oportunidade.