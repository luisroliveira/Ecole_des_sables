# Use uma imagem oficial do Python
FROM python:3.13-slim

# Set working directory
WORKDIR /app

# Copie os arquivos de requirements primeiro (para otimizar cache)
COPY requirements.txt .

# Atualize pip e instale dependências
RUN pip install --upgrade pip setuptools wheel \
    && pip install -r requirements.txt

# Copie todo o restante do projeto
COPY . .

# Defina variáveis de ambiente padrão (podem ser sobrescritas no Render)
ENV DEBUG=0
ENV SECRET_KEY=supersecretkey
ENV DATABASE_URL=postgres://user:password@host:port/dbname
ENV ALLOWED_HOSTS=*

# Exponha a porta que o Gunicorn usará
EXPOSE 8000

# Comando para coletar static files, migrar banco e rodar Gunicorn
CMD sh -c "python manage.py collectstatic --noinput && \
           python manage.py makemigrations && \
           python manage.py migrate && \
           gunicorn config.wsgi:application --bind 0.0.0.0:8000"
