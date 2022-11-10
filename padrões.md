# Nomeando sua imagem
```
hugovallada/api-conversao:v1

namespace/repositorio:versao
```

# Preferência a imagens oficiais
```
Imagens oficiais não possuem namespace.
Imagens oficiais são seguras.
```

# Sempre especificar as tags nas imagens
 ```
 FROM node -> ERRADO. Sempre está usando a latest -> Quebra a idempotencia.

 FRON node:14.17.5 -> CERTO. Sempre vai estar com a mesma versçao do NODE.
 ```

 # Um processo por container
 ```
 Não colocar mais de uma aplicação por container.
 Se precisar subir várias aplicações, subir múltiplos containers e comunicar entre eles.
 ```

 # Aproveitamento das camadas de imagem
 ```
 Utilizar as camadas para ganhar desempenho na construção da imagem
 ```

 # Utilizar o .dockerignore
 ```
 Ignore os dados desnecessários
 ```

 # COPY vs ADD
 ```
 Se não for descompactar ou pegar o arquivo via url, UTILIZAR O COPY
 ```

 # ENTRYPOINT vs CMD
 ```
 O ENTRYPOINT é imutável.
 O CMD pode ser sobrescrito
 Pode usar os 2 combinados, usando o CMD para passar parâmetros para o ENTRYPOINT.
 Usar o ENTRYPOINT quando o start do container deve ser imutável
 ```

 # Usar argumentos na construção da imagem
 ```
 Pode passar argumentos que serão substituidos durante o build da imagem

 ARG TAG=latest
 FROM ubuntu:${TAG}

 docker image build ... --build-arg TAG="20.04"
 ```

 # Multistage Build
 ```
Realiza o build em um estágio e depois gera uma nova imagem que será usada para o container

FROM golang:1.10 as build
WORKDIR /app
COPY main.go .
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main

FROM scratch as final
WORKDIR /app
COPY --from=build /app/main .
CMD ["./main"]
 ```