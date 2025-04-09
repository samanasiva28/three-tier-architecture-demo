# Use a newer Go version (1.23 or later) to match your current Go version
FROM golang:1.23

WORKDIR /go/src/app

COPY *.go .

# Initialize Go modules and fetch dependencies
RUN go mod init dispatch && go get

# Install the application
RUN go install

# Command to run the application
CMD ["./dispatch"]
