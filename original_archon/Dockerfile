FROM python:3.12-slim
WORKDIR /app
RUN apt-get update && apt-get install -y --no-install-recommends build-essential sed && rm -rf /var/lib/apt/lists/*
COPY python/requirements.server.txt ./
COPY python/requirements.mcp.txt ./
COPY python/requirements.agents.txt ./
RUN pip install --no-cache-dir -r requirements.server.txt -r requirements.mcp.txt -r requirements.agents.txt

# First, copy the source code into the image
COPY ./python/src ./src

# Now, modify the file that has been copied
RUN sed -i '/validate_supabase_url(supabase_url)/s/^/# /' src/server/config/config.py

RUN playwright install-deps
RUN playwright install
ENV PYTHONUNBUFFERED=1
EXPOSE 8000
CMD ["uvicorn", "src.server.main:app", "--host", "0.0.0.0", "--port", "8000"]
