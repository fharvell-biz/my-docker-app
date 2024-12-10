FROM node:20-alpine
RUN apk add --no-cache curl
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install
COPY . .
RUN IP=$(curl ifconfig.me) && echo "CORS_ORIGIN=$IP" > .env
EXPOSE 5000
CMD ["npm", "start"]
