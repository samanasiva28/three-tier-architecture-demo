# Use a newer Go version (1.23 or later) to match your current Go version
FROM golang:1.23

# Set environment variable for Go modules
ENV GO111MODULE=on

# Set the working directory
WORKDIR /go/src/app

# Copy your Go files into the image
COPY *.go .

# Initialize Go modules and fetch dependencies
RUN go mod init dispatch && go mod tidy

# Build the binary and place it in the working directory
RUN go build -o dispatch

# Expose the port your app uses (optional but recommended)
EXPOSE 8080

# Run the compiled binary
CMD ["./dispatch"]

