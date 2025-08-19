# Simple Vite dev server setup
FROM node:18-alpine

WORKDIR /app

# Install system dependencies needed for some npm packages
RUN apk add --no-cache python3 make g++ git curl

# Copy package files
COPY package*.json ./

# Install dependencies including dev dependencies for testing
RUN npm ci

# Create coverage directory with proper permissions
RUN mkdir -p /app/coverage && chmod 777 /app/coverage

# Copy source code
COPY . .

# Automatically correct the API URL environment variable in the source code
RUN sed -i 's/import.meta.env.ARCHON_SERVER_PORT/import.meta.env.VITE_API_URL/g' src/config/api.ts

# Expose Vite's default port
EXPOSE 5173

# Start Vite dev server with host binding for Docker
CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0"]
