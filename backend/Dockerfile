# Stage 1:
FROM node:18-alpine AS builder

WORKDIR /app

# Copy package files and install only production dependencies
COPY package*.json .
RUN npm ci --only=production

COPY . .

# Stage 2:
FROM node:18-alpine


WORKDIR /app

# Create non-root user for better security
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Set environment variable
ENV NODE_ENV=production

COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app .

# Set file permissions
RUN chown -R appuser:appgroup /app

# Switch to non-root user
USER appuser

EXPOSE 5001

CMD ["node", "src/index.js"]