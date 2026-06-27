# Start with a base image
FROM golang:1.24-alpine AS builder


# Set the working directory inside the container
WORKDIR /app


# Copy the go.mod and go.sum files to the working directory
COPY go.mod go.sum ./

# Download all the dependencies
RUN go mod download

# Copy the source code to the working directory
COPY . .


# Build the application
RUN CGO_ENABLED=0 GOOS=linux go build -o main .

#######################################################
# Reduce the image size using multi-stage builds
# We will use a distroless image to run the application

FROM gcr.io/distroless/static-debian12

# Copy the binary from the previous stage
COPY --from=builder /app/main .

USER nonroot:nonroot

EXPOSE 5000

# Command to run the application
CMD ["./main"]
