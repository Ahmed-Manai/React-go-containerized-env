.PHONY: build
build:
	echo "FROM scratch" > Dockerfile
	docker build --tag my-scratch-image .
	-rm Dockerfile


# have to authenticate to dockerhub first and create repo first 
.PHONY: push-dockerhub
push-dockerhub:e
	docker tag my-scratch-image ahmed2docker4iot/my-scratch-image #default to latest
	docker push ahmed2docker4iot/my-scratch-image #push image with latest tag

	docker tag my-scratch-image ahmed2docker4iot/my-scratch-image:abc-123 
	docker push ahmed2docker4iot/my-scratch-image:abc-123 #push image with specific tag