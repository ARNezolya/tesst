# syntax=docker/dockerfile:1

FROM node:20-alpine AS deps
WORKDIR /app
COPY package*.json ./ || true
RUN if [ -f package.json ]; then npm ci --omit=dev; fi

FROM node:20-alpine AS build
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules || true
COPY . .
# Если есть сборка (Next.js / TypeScript) — выполни:
RUN npm run build || true

FROM node:20-alpine AS runtime
WORKDIR /app
ENV NODE_ENV=production
COPY --from=build /app ./
EXPOSE 3000
# Если у тебя есть npm start — он запустится; иначе замени на свою команду
CMD ["npm","start"]
