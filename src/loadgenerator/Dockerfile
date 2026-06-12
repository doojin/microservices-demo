FROM python:3.11-slim

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

RUN useradd appuser \
  --no-create-home \
  --uid 1001 \
  --shell /usr/sbin/nologin \
  && chown -R appuser:appuser /app

ENV LOCUST_SPAWN_RATE=1
ENV LOCUST_USERS=5
ENV LOCUST_HOST=http://frontend:8080

USER appuser

CMD ["locust", "-f", "locustfile.py", "--headless"]