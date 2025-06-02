# Build stage
FROM node:22.16.0-alpine3.22 AS builder

WORKDIR /app
RUN npm install -g pnpm

COPY package.json ./
COPY pnpm-lock.yaml ./

RUN pnpm install

COPY . .

RUN pnpm build

# Production stage
FROM node:22.16.0-alpine3.22

WORKDIR /app
RUN npm install -g pnpm
COPY --from=builder /app/next.config.mjs ./next.config.mjs
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package.json ./package.json
COPY --from=builder /app/pnpm-lock.yaml ./pnpm-lock.yaml

CMD ["pnpm", "dev"]