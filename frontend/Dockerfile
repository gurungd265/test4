# Stage 1: Build the application
FROM node:18 as builder  # Not using Alpine for builder stage

WORKDIR /app

# Copy package files first for better caching
COPY package.json yarn.lock* package-lock.json* pnpm-lock.yaml* ./

# Install dependencies
RUN npm install

# Copy all files
COPY . .

# Build the application
RUN npm run build

# Stage 2: Serve the application (can use Alpine here)
FROM node:18-alpine as runner

WORKDIR /app

# Install serve to run the production build
RUN npm install -g serve

# Copy the built files from the builder stage
COPY --from=builder /app/dist ./dist

# Expose the port the app will run on
EXPOSE 3000

# Start the server
CMD ["serve", "-s", "dist", "-l", "3000"]
