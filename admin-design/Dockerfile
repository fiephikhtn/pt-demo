FROM node:18-alpine AS deps
RUN apk add --no-cache libc6-compat

WORKDIR /app
COPY package*.json ./
RUN npm install

# Rebuild the source code only when needed
FROM node:18-alpine AS builder

WORKDIR /app

COPY --from=deps /app/node_modules ./node_modules

COPY . .

RUN npm run build

# Production image, copy all the files and run next
FROM node:18-alpine AS runner
# install simple http server for serving static content
RUN npm install -g http-server

WORKDIR /app

ENV NODE_ENV production

RUN addgroup --system --gid 1001 admingroup
RUN adduser --system --uid 1001 adminuser

COPY --from=builder /app/public ./public
COPY --from=builder /app/package.json ./package.json

# Automatically leverage output traces to reduce image size
# https://nextjs.org/docs/advanced-features/output-file-tracing
COPY --from=builder --chown=adminuser:admingroup /app/dist ./

USER adminuser

EXPOSE 8081

ENV PORT 8081

# CMD : instead entire param in CMD when run there are params input.
# ENTRYPOINT : when run there are params , It will append param input into.
CMD [ "http-server", "app", "-p 8081" ]