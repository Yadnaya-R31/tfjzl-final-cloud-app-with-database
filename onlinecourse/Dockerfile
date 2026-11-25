FROM python:3.9.18-slim-bullseye

ENV PYTHONUNBUFFERED=1
ENV PYTHONDONTWRITEBYTECODE=1

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    gcc \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy project files
COPY . .

# Collect static files and run migrations
RUN python3 manage.py collectstatic --noinput

EXPOSE 8000

CMD ["sh", "-c", "python3 manage.py makemigrations && python3 manage.py migrate && gunicorn --bind 0.0.0.0:8000 --workers 3 myproject.wsgi"]
