
FROM python:3.13-slim

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    postgresql-client \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements
COPY requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Set shared Playwright browser location
ENV PLAYWRIGHT_BROWSERS_PATH=/ms-playwright

# Install Chromium and required Linux libraries
RUN python -m playwright install --with-deps chromium \
    && chmod -R a+rX /ms-playwright

# Copy application code
COPY . .

# Create non-root user
RUN useradd -m -u 1000 appuser \
    && chown -R appuser:appuser /app

USER appuser

EXPOSE 8000

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
