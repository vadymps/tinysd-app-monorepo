FROM node:20-alpine AS builder
WORKDIR /app

# Copy package files
COPY package*.json ./
COPY nx.json ./
COPY tsconfig.base.json ./

# Install dependencies
RUN npm ci

# Copy source code
COPY . .

# Build the API
RUN npx nx build api --prod

FROM node:20-alpine AS runner
WORKDIR /app

# Copy built application
COPY --from=builder /app/dist/apps/api ./dist
COPY --from=builder /app/package*.json ./

# Install only production dependencies
RUN npm ci --only=production && npm cache clean --force

EXPOSE 3000
CMD ["node", "dist/main.js"]
