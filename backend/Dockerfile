FROM python:3.10-slim

# Set working directory
WORKDIR /appuser

# Install system dependencies
RUN apt-get update && apt-get install -y \
    bash \
    dos2unix \
    && rm -rf /var/lib/apt/lists/*

# Install Poetry
RUN pip install poetry

# Copy dependency files
COPY pyproject.toml poetry.lock /appuser/

# Install dependencies (without dev dependencies)
RUN poetry install --no-root --only main

# Copy the rest of the code
COPY . /appuser

# Ensure prestart.sh uses Unix line endings & is executable
RUN dos2unix ./prestart.sh && chmod +x ./prestart.sh

# Environment variables
ENV PYTHONPATH=/appuser

# Expose FastAPI port
EXPOSE 8000

CMD ["bash", "-c", "poetry run bash ./prestart.sh && poetry run uvicorn app.main:app --host 0.0.0.0 --port 8000"]
