# ------------------- Stage 1: Builder -------------------
FROM python:3.10-slim AS builder

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    POETRY_VERSION=1.7.0

WORKDIR /app

# Install system dependencies + poetry
RUN apt-get update && \
    apt-get install -y --no-install-recommends curl && \
    pip install --no-cache-dir "poetry==$POETRY_VERSION"

# Copy dependency files
COPY pyproject.toml poetry.lock ./

# Install production dependencies (using new poetry syntax)
RUN poetry config virtualenvs.create false && \
    poetry install --only main --no-interaction --no-ansi

# ------------------- Stage 2: Runtime -------------------
FROM python:3.10-alpine

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

WORKDIR /app

# Copy installed dependencies
COPY --from=builder /usr/local/lib/python3.10/site-packages /usr/local/lib/python3.10/site-packages
COPY --from=builder /usr/local/bin /usr/local/bin

# Copy application code
COPY . .

# Cleanup and permissions
RUN find /usr/local/lib/python3.10/site-packages -name "tests" -exec rm -rf {} + && \
    find /usr/local/lib/python3.10/site-packages -name "__pycache__" -exec rm -rf {} + && \
    adduser -D myuser && \
    chown -R myuser:myuser /app

USER myuser

EXPOSE 8000

CMD ["gunicorn", "--bind", "0.0.0.0:8000", "demo.wsgi:application"]