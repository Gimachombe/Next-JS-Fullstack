# ------------------------------
# 1️⃣ Base image
# ------------------------------
FROM node:18-alpine AS base
WORKDIR /app
COPY package*.json ./

# ------------------------------
# 2️⃣ Install dependencies
# ------------------------------
FROM base AS deps
RUN npm install

# ------------------------------
# 3️⃣ Build the app
# ------------------------------
FROM base AS builder
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build

# ------------------------------
# 4️⃣ Production image
# ------------------------------
FROM node:18-alpine AS runner
WORKDIR /app

ENV NODE_ENV=production
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package*.json ./

# Expose the port Next.js runs on
EXPOSE 3000

# Run the Next.js app
CMD ["npm", "start"]
