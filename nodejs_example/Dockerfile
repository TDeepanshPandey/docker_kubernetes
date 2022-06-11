FROM node:14-alpine

# Set a working directory
WORKDIR usr/app
# Copy local file like package.json to container
COPY ./package.json ./
RUN npm install

COPY ./ ./

CMD ["npm", "start"]
