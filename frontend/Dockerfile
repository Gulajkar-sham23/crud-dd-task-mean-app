FROM node:20-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build || npx ng build --configuration production

RUN mkdir -p /tmp/nginxhtml && \
    if [ -d "dist" ]; then \
      SUBDIR=$(ls -1 dist | head -n1); \
      cp -r "dist/$SUBDIR/"* /tmp/nginxhtml/; \
    fi

FROM nginx:1.27-alpine
COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build /tmp/nginxhtml /usr/share/nginx/html
EXPOSE 80

