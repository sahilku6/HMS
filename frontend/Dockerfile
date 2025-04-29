FROM node:18

WORKDIR /app

COPY package*.json ./

# More robust retry logic for npm install
RUN npm install || \
  (echo "Retry 1..." && sleep 5 && npm install) || \
  (echo "Retry 2..." && sleep 10 && npm install) || \
  (echo "Retry 3..." && sleep 20 && npm install)

COPY . .

EXPOSE 5173

CMD ["npm", "run", "dev"]
