# Start with a stable, well-supported base image: Ubuntu 22.04 LTS
FROM ubuntu:22.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive
ENV PYTHONUNBUFFERED=1

# Install system dependencies and Python 3.12
RUN apt-get update &&\
    apt-get install -y --no-install-recommends\
    software-properties-common\
    gnupg\
    && add-apt-repository ppa:deadsnakes/ppa &&\
    apt-get update &&\
    apt-get install -y --no-install-recommends\
    python3.12\
    python3.12-venv\
    python3-pip\
    build-essential\
    sed\
    && rm -rf /var/lib/apt/lists/*

# Create a standard Python virtual environment
RUN python3.12 -m venv /opt/venv

# Set the working directory
WORKDIR /app

# Copy the new requirements file
COPY python/requirements.txt ./

# Install all dependencies from the requirements.txt file. This is the most reliable method.
RUN /opt/venv/bin/pip install -r requirements.txt

# Copy the application source code
COPY python/src ./src

# Apply the sed patch to disable Supabase URL validation
RUN sed -i 's/.*validate_supabase_url.*/#&/' src/server/config/config.py

# Install Playwright's browser dependencies
RUN /opt/venv/bin/python -m playwright install-deps
RUN /opt/venv/bin/python -m playwright install

# Expose the port
EXPOSE 8000

# Define the command to run the application.
CMD /opt/venv/bin/python -m uvicorn src.server.main:app --host 0.0.0.0 --port 8000
