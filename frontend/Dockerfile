FROM node:20.16.0-alpine3.19 AS builder
WORKDIR /app

#ENV
ENV BACKEND_URL=###MONKEYTYPE_BACKENDURL### RECAPTCHA_SITE_KEY=###RECAPTCHA_SITE_KEY###

#COPY
COPY ["package.json", "package-lock.json", "./"]

# Copy the entire frontend directory
COPY . .

COPY firebase-config-live.ts src/ts/constants/firebase-config.ts
COPY firebase-config-live.ts src/ts/constants/firebase-config-live.ts

#build
RUN npm i --frozen-lockfile && \
    npm run build

# COPY to target
FROM nginx:mainline-alpine

COPY --from=builder /app/dist /usr/share/nginx/html
COPY updateConfig.sh /docker-entrypoint.d/updateConfig.sh
RUN chmod +x /docker-entrypoint.d/updateConfig.sh