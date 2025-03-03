FROM node:20.16.0-alpine3.19 AS builder
WORKDIR /app

#ENV
ENV BACKEND_URL=###MONKEYTYPE_BACKENDURL### RECAPTCHA_SITE_KEY=###RECAPTCHA_SITE_KEY###

#COPY
# Copy only package.json first
COPY package.json ./

# Install git and npm dependencies with legacy-peer-deps flag
RUN apk add --no-cache git && \
    npm install --legacy-peer-deps

# Copy the entire frontend directory
COPY . .

COPY firebase-config-live.ts src/ts/constants/firebase-config.ts
COPY firebase-config-live.ts src/ts/constants/firebase-config-live.ts

#build
RUN npm run build

# COPY to target
FROM nginx:mainline-alpine

COPY --from=builder /app/dist /usr/share/nginx/html
COPY updateConfig.sh /docker-entrypoint.d/updateConfig.sh
RUN chmod +x /docker-entrypoint.d/updateConfig.sh