FROM node:16-alpine AS builder

# Set a working directory
WORKDIR '/app'

# Copy local file like package.json to container
COPY package.json .
RUN npm install

COPY . .

RUN npm run build

FROM nginx
COPY --from=builder /app/build /usr/share/nginx/html 
