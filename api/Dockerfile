# ---- Stage 1 : build ----
FROM node:22-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci --ignore-scripts
COPY . .
RUN npx prisma generate
RUN npm run build

# ---- Stage 2 : runtime ----
FROM node:22-alpine AS runtime
WORKDIR /app
ENV NODE_ENV=production

COPY package*.json ./
RUN npm ci --omit=dev --ignore-scripts

COPY --from=build /app/dist ./dist
COPY --from=build /app/prisma ./prisma


RUN addgroup -S nodejs && adduser -S nestjs -G nodejs
RUN chown -R nestjs:nodejs /app
USER nestjs

EXPOSE 3000
CMD ["node", "dist/main"]