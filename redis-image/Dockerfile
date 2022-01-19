# Usando uma imagem docker existente como base
FROM alpine

# Fazendo Download e instalando a dependência
RUN apk add --update redis
RUN apk add --update gcc

# Preparar a imagem para o que fazer quando o container começar a rodar
CMD ["redis-server"]