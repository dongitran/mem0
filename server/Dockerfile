FROM python:3.12-slim

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Install server dependencies
COPY server/requirements.txt /app/requirements.txt
RUN pip install --no-cache-dir -r /app/requirements.txt

# Install mem0 package from repository
WORKDIR /app/packages
COPY pyproject.toml README.md ./
COPY mem0 ./mem0
RUN pip install --no-cache-dir --no-deps -e .

# Copy server code
WORKDIR /app
COPY server /app

EXPOSE 8000
ENV PYTHONUNBUFFERED=1

# Auto-run alembic migrations and start FastAPI server
CMD ["sh", "-c", "alembic upgrade head && uvicorn main:app --host 0.0.0.0 --port 8000"]
