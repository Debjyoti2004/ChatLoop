# Stage 1: Build
FROM node:18-alpine AS builder

WORKDIR /app

COPY package*.json .

RUN npm ci

COPY . .

ENV NODE_ENV=production

RUN npm run build

# Stage 2: Serve
FROM nginx:stable-alpine

RUN rm -rf /usr/share/nginx/html/*

COPY --from=builder /app/dist /usr/share/nginx/html

COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]