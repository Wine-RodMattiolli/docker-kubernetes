# Especificar a imagem base
FROM node:alpine

# Instalar a dependência
WORKDIR /usr/app
COPY ./ ./
RUN npm install

# Comandos Padrão
CMD ["npm", "start"]