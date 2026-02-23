#  Build Angular App
  FROM node:18 AS build

  WORKDIR /app
  
  COPY package*.json ./
  RUN npm install
  
  COPY . .
  RUN npm run build
  
  
  # Runtime  
  FROM node:18-alpine

  WORKDIR /app
  RUN npm install -g serve
  
  COPY --from=build /app/dist /app/dist
  EXPOSE 8081

  CMD ["serve", "-s", "dist/angular-15-crud", "-l", "8081"]