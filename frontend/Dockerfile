# Use a stable Node.js image
FROM node:18

# Set working directory in container
WORKDIR /app

# Copy package.json and package-lock.json first to take advantage of Docker cache
COPY package*.json ./

# Install dependencies (adding retry logic for unstable networks)
RUN npm install --prefer-offline --no-audit --progress=false || \
    (echo "Retrying npm install..." && sleep 5 && npm install --prefer-offline --no-audit --progress=false)

# Copy the rest of the application
COPY . .

# Expose the app port (adjust if needed)
EXPOSE 5173

# Default command to run the app in development mode
CMD ["npm", "run", "dev"]
