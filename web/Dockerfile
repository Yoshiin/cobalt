# --- Stage 1: Build ---
FROM node:23-alpine AS builder

RUN corepack enable && corepack prepare pnpm@latest --activate

RUN apk add --no-cache python3 make g++ git

WORKDIR /app
COPY . .

RUN git init

ARG WEB_DEFAULT_API
ENV VITE_API_URL=${WEB_DEFAULT_API}

RUN pnpm install
RUN cd web && pnpm run build

# --- Stage 2: Runtime ---
FROM node:23-alpine AS runner
WORKDIR /app
RUN npm install -g serve
COPY --from=builder /app/web/build ./build
EXPOSE 3000
CMD ["serve", "-s", "build", "-l", "3000"]
