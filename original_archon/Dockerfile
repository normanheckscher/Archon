# Start with a stable, well-supported base image: Ubuntu 22.04 LTS
FROM ubuntu:22.04

# Set environment variables to prevent interactive prompts during installation
ENV DEBIAN_FRONTEND=noninteractive

# Install dependencies for adding PPAs (including gnupg for key management),
# then add the deadsnakes PPA for newer Python versions,
# then install Python 3.12, pip, and other necessary tools in a single layer.
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    software-properties-common \
    gnupg \
    && add-apt-repository ppa:deadsnakes/ppa && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
    python3.12 \
    python3-pip \
    build-essential \
    sed \
    && rm -rf /var/lib/apt/lists/*

# Set the working directory
WORKDIR /app

# Copy requirements files
COPY python/requirements.server.txt ./
COPY python/requirements.mcp.txt ./
COPY python/requirements.agents.txt ./

# Install Python dependencies using pip3
RUN pip3 install --no-cache-dir -r requirements.server.txt -r requirements.mcp.txt -r requirements.agents.txt

# Copy the application source code
COPY ./python/src ./src

# Apply the sed patch
RUN sed -i '/validate_supabase_url(supabase_url)/s/^/# /' src/server/config/config.py

# Install Playwright browser dependencies
RUN playwright install-deps
RUN playwright install

# Set environment variable and expose port
ENV PYTHONUNBUFFERED=1
EXPOSE 8000

# Define the command to run the application
CMD ["uvicorn", "src.server.main:app", "--host", "0.0.0.0", "--port", "8000"]
