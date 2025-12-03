FROM python:3.13-slim

# Definir diretório de trabalho
WORKDIR /app

# Instalar dependências do sistema necessárias para psycopg2 e Pillow
RUN apt-get update && apt-get install -y \
    build-essential \
    libpq-dev \
    gcc \
    libjpeg-dev \
    zlib1g-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Copiar requirements.txt primeiro para aproveitar cache
COPY requirements.txt .

# Atualizar pip e instalar dependências Python
RUN pip install --upgrade pip setuptools wheel \
    && pip install -r requirements.txt

# Copiar o restante do projeto
COPY . .

# Variáveis de ambiente (pode sobrescrever no Render)
ENV DEBUG=0
ENV SECRET_KEY=supersecretkey
ENV DATABASE_URL=postgres://user:password@host:port/dbname
ENV ALLOWED_HOSTS=*

# Expor porta
EXPOSE 8000

# Comando padrão: migrar banco, coletar static e rodar Gunicorn
CMD sh -c "python manage.py collectstatic --noinput && \
           python manage.py makemigrations && \
           python manage.py migrate && \
           gunicorn config.wsgi:application --bind 0.0.0.0:8000"
