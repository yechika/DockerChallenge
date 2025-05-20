# -------- Stage 1: Build --------
FROM alpine:latest AS builder

RUN apk add --no-cache build-base 
WORKDIR /app
COPY main.cpp .
RUN g++ -o hello -static main.cpp  

# -------- Stage 2: Runtime --------
FROM scratch  
COPY --from=builder /app/hello /
CMD ["/hello"]