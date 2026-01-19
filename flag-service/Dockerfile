FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

# O serviço roda na 8002 (conforme README)
EXPOSE 8002

# Gunicorn conforme documentação do projeto
CMD ["gunicorn", "--bind", "0.0.0.0:8002", "app:app"]
