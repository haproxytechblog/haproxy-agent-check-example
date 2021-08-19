FROM golang:alpine AS build
WORKDIR /src
COPY . .
RUN GOOS=linux GOARCH=amd64 go build -v -o ./agent .

FROM nginx:alpine
EXPOSE 80/tcp
EXPOSE 3000/tcp
RUN apk add stress-ng
COPY --from=build /src/agent /
COPY ./init.sh /
RUN chmod +x /init.sh
CMD ["/init.sh"]