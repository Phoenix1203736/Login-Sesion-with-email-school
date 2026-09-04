# --- ETAPA 1: Compilación (Builder) ---
    FROM node:24-alpine AS builder
    WORKDIR /app
    RUN corepack enable
    COPY package.json pnpm-lock.yaml ./
    RUN pnpm install --frozen-lockfile
    COPY . .
    
    # Generar los archivos estáticos de producción (crea la carpeta /dist)
    RUN pnpm build
    
# --- ETAPA 2: Servidor de Producción para Vue ---
    FROM node:24-alpine AS runner
    WORKDIR /app
    ENV NODE_ENV=production
    
    # Instalar una herramienta ligera para servir archivos estáticos (ej. 'serve')
    RUN npm install -g serve
    
    # Copiar la carpeta compilada (dist) desde el builder
    COPY --from=builder /app/dist ./dist
    
    # Exponer el puerto que decidas (puede ser el 5173 o el 3000)
    EXPOSE 5173
    
    # Servir la carpeta de producción en el puerto indicado
    CMD ["serve", "-s", "dist", "-l", "5173"]
        