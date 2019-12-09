#build stage
FROM golang:alpine AS build

WORKDIR /src

COPY main*.go ./
COPY go.* ./

RUN CGO_ENABLED=0 go build -o /bin/helloapp

CMD ["rm"]


FROM scratch

COPY --from=build /bin/helloapp /bin/helloapp

EXPOSE 8080

CMD ["/bin/helloapp"]