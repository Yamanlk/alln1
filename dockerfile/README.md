# Dockerfile

## pnpm + prisma + nestjs

```dockerfile
FROM node:20-slim as base
RUN apt-get update -y && apt-get install -y openssl
ENV PNPM_HOME="/pnpm"
ENV PATH="$PNPM_HOME:$PATH"
RUN corepack enable
WORKDIR /app
COPY package.json pnpm-lock.yaml ./

FROM base AS builder
# don't use --dev here, prisma needs @prisma/client installed to be able to generate client
RUN --mount=type=cache,id=pnpm,target=/pnpm/store pnpm install --frozen-lockfile
COPY . .
# this step is required for type checking, if you don't care about type checking
# you can enable `skipDefaultLibCheck` or if your using swc you can just remove
# the following line
RUN pnpm prisma generate
RUN pnpm run build

FROM base
# install only production dependencies
RUN --mount=type=cache,id=pnpm,target=/pnpm/store pnpm install --prod --frozen-lockfile
COPY --from=builder /app/dist /app/dist
COPY prisma /app/prisma
RUN pnpx prisma generate
CMD [ "pnpm", "start:prod" ]
```
