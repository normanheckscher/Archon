FROM python:3.12-slim
WORKDIR /app
RUN apt-get update && apt-get install -y --no-install-recommends build-essential && rm -rf /var/lib/apt/lists/*
COPY python/requirements.server.txt ./requirements.server.txt
COPY python/requirements.mcp.txt ./requirements.mcp.txt
COPY python/requirements.agents.txt ./requirements.agents.txt
RUN pip install --no-cache-dir -r requirements.server.txt -r requirements.mcp.txt -r requirements.agents.txt
RUN sed -i '/validate_supabase_url(supabase_url)/s/^/# /' src/server/config/config.py
RUN playwright install
COPY ./python/src ./src
ENV PYTHONUNBUFFERED=1
EXPOSE 8000
CMD ["uvicorn", "src.server.main:app", "--host", "0.0.0.0", "--port", "8000"]
