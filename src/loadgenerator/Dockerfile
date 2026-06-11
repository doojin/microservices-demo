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

USER appuser

ENV USERS=5
ENV SPAWN_RATE=1

CMD ["sh", "-c", "locust -f locustfile.py --users=$USERS --spawn-rate=$SPAWN_RATE --headless --host=http://frontend:8080"]