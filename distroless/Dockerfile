# Without Edge Function Support (Distroless)
# --- Builder Stage ---

FROM node:20-alpine AS builder

WORKDIR /app

COPY package.json pnpm-lock.yaml ./
COPY . .

RUN npm install -g pnpm && pnpm install --frozen-lockfile && pnpm build && pnpm prune --prod

# --- Runner Stage ---
FROM gcr.io/distroless/nodejs20-debian12

WORKDIR /app

COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next/static ./.next/static

ENV NODE_ENV=production
ENV PORT=3000
EXPOSE 3000

CMD ["server.js"]
