# FROM node:14-alpine3.12
# COPY . .
# EXPOSE 4000
# CMD ["node","index.js"]
FROM node:14-alpine3.12

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
COPY package*.json ./
RUN npm install

# Bundle app source
COPY . .

# Expose the port the app runs on
EXPOSE 4000

# Command to run the app
CMD ["node", "index.js"]

