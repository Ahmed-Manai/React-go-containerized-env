##### download golang alpine (minimal) iamge && defining this stage as build
# baster image contain needed files (build tools) for static linking 
FROM golang:1.19-buster as build 
# setting working directory
WORKDIR /app
# add a nonroot user with specific user id 1001
RUN useradd -u 1001 nonroot
# copy only the file contain the reference to our dependencies (don't invalidate the cache)
COPY go.mod go.sum ./
# download all dependencies with a cache mount
RUN --mount=type=cache,target=/go/pkg/mod \
	--mount=type=cache,target=/root/.cache/go-build \ 
	go mod download
# copy file from current dir to the root dir of the image 
COPY . .
# build in the code inside the image, making the binaries ready to use
# add extra configuratio (static linking need blaster image) to be able to pass the binaries (define as external and static) and define a network 
RUN go build \
	-ldflags="-linkmode external -extldflags -static"\
	-tags netgo \
	-o api-golang

##### defining second stage with empty image (scratch) is a release stage
FROM scratch 
# define environment variable for gin (API framework)
ENV GIN_MODE release
# Copy the password file for my user in the build stage 
COPY --from=build /etc/passwd /etc/passwd
# Copy files from first stage image to second stage 
COPY --from=build /app/api-golang api-golang
# define the nonroot user
USER nonroot
# port documentation for other users
EXPOSE 8080
# run cmd when the container base on this image starts
CMD ["/api-golang"]
