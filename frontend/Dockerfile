# Stage 1: Build with Node 20 (full image)
FROM node:20 as builder

WORKDIR /app

# Install Python and build tools (needed for some dependencies)
RUN apt-get update && apt-get install -y python3 make g++

# Copy package files first for better caching
COPY package.json yarn.lock* package-lock.json* pnpm-lock.yaml* ./

# Install dependencies (force if needed)
RUN npm install --force

# Copy all files
COPY . .

# Build the application
RUN npm run build

# Stage 2: Serve with Node 20 Alpine
FROM node:20-alpine as runner

WORKDIR /app

# Install serve globally
RUN npm install -g serve

# Copy built files from builder
COPY --from=builder /app/dist ./dist

EXPOSE 3000
CMD ["serve", "-s", "dist", "-l", "3000"]
