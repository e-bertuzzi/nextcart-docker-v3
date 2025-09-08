# Stage 1: Build
#FROM node:20-alpine AS builder
#WORKDIR /app

#COPY package*.json tsconfig*.json nx.json ./
#RUN npm install

#COPY . .
#RUN npx nx build website

# Stage 2: Runtime con nginx
#FROM nginx:alpine

# Website buildato in apps/dist/website
#COPY --from=builder /app/apps/dist/website /usr/share/nginx/html

#EXPOSE 80
#CMD ["nginx", "-g", "daemon off;"]

# Stage 1: Build
# Stage 1: Build
FROM node:20-alpine AS builder
WORKDIR /app

# Copia file essenziali per installare le dipendenze
COPY package*.json tsconfig*.json nx.json ./
RUN npm install

# Copia tutto il codice sorgente
COPY . .

# Build dell'app React con NX + Vite
RUN npx nx build website-user

# Stage 2: Runtime con Nginx
FROM nginx:alpine

RUN apk add --no-cache bash

# Copia la build generata da NX/Vite
COPY --from=builder /app/dist/apps/website-user /usr/share/nginx/html

# Copia la configurazione Nginx personalizzata
COPY apps/website-user/default.conf /etc/nginx/conf.d/default.conf

# Copia lo script wait-for-it
COPY apps/website-user/wait-for-it.sh /wait-for-it.sh
RUN chmod +x /wait-for-it.sh

EXPOSE 80

# Avvia Nginx solo quando l'API è pronta
CMD ["/wait-for-it.sh", "api:3000", "--", "nginx", "-g", "daemon off;"]

