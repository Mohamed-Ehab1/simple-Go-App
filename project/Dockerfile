# Starting from a minimal Alpine base image
FROM alpine:3.14


# create a directory and set it as workdir
WORKDIR /app

#add envionmen variables

ENV MYSQL_HOST="172.17.0.2"
ENV MYSQL_USER="root"
ENV MYSQL_PASS="Bob123@mySql"
ENV MYSQL_PORT="3306"

# copy app project to container
COPY go.mod go.sum ./
RUN apk --no-cache add go && go mod download

# Copy the application files
COPY db.go main.go ./

# Build the Go application
RUN CGO_ENABLED=0 GOOS=linux go build -o myapp

# Set the command to run the executable
CMD ["./myapp"]

# Expose port
EXPOSE 9090
