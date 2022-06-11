# Use an exsisting docker image
FROM alpine

# Download and install a dependency
RUN apk add --update redis

# What to do when it starts
CMD ["redis-server"]
