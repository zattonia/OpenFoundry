FROM node:20-alpine AS builder

WORKDIR /app

# Installazione di pnpm
RUN corepack enable && corepack prepare pnpm@latest --activate

# Copia dei file di configurazione
COPY package.json pnpm-lock.yaml pnpm-workspace.yaml ./
COPY apps/web/package.json ./apps/web/

# Installazione dipendenze
RUN pnpm install --frozen-lockfile

# Copia dei sorgenti
COPY . .

# Build dell'applicazione web
RUN pnpm --filter @open-foundry/web build

FROM node:20-alpine AS runner
WORKDIR /app

RUN corepack enable && corepack prepare pnpm@latest --activate

COPY --from=builder /app ./

EXPOSE 10000

ENV PORT=10000
ENV HOST=0.0.0.0

CMD ["sh", "-c", "cd apps/web && npx vite preview --host 0.0.0.0 --port $PORT --allowedHosts all"]
