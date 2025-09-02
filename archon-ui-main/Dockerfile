# Simple Vite dev server setup
FROM node:18-alpine

WORKDIR /app

# Install system dependencies needed for some npm packages
RUN apk add --no-cache python3 make g++ git curl

# Copy package files
COPY package*.json ./
FROM node:18-alpine
WORKDIR /app
RUN apk add --no-cache python3 make g++ git curl
COPY package*.json ./
RUN npm ci
RUN mkdir -p /app/coverage && chmod 777 /app/coverage
COPY . .
# Automatically correct the API URL environment variable in the source code
RUN sed -i 's/import.meta.env.ARCHON_SERVER_PORT/import.meta.env.VITE_API_URL/g' src/config/api.ts
EXPOSE 3737
CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0"]
