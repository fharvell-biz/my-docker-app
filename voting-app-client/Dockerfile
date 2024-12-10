FROM node:20-alpine
RUN apk add --no-cache curl
WORKDIR /app
COPy . .
RUN npm install
RUN IP=$(curl ifconfig.me) && echo "REACT_APP_BACKEND_API_URL=$IP" > .env
EXPOSE 3001
CMD ["npm", "start"]
