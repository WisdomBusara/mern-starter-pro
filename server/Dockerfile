FROM node:20-alpine AS deps
WORKDIR /app
COPY package*.json tsconfig.json ./
RUN npm ci || npm install
FROM node:20-alpine AS build
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY src ./src
RUN npm run build || true
FROM node:20-alpine
WORKDIR /app
ENV NODE_ENV=production
COPY --from=build /app/dist ./dist
COPY package*.json ./
RUN npm pkg set scripts.start="node dist/index.js"
EXPOSE 5000
CMD ["npm","start"]
