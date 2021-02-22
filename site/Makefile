REGISTRY := rg.fr-par.scw.cloud
NS := lujeni
PRJ ?= site
IMG := $(REGISTRY)/$(NS)/$(PRJ)
ENV ?= production
TAG ?= $(shell git describe --tags)

.SILENT: login

help:
	@awk 'BEGIN {FS = ":.*?## "} /^[a-zA-Z0-9_-]+:.*?## / {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST) | sort

local:
	docker run -it -p 8080:8080 $(IMG):latest-$(ENV) 

login:
	docker login $(REGISTRY)/$(NS) -u nologin -p $(SCW_SECRET_KEY)

build:
	DOCKER_BUILDKIT=1 docker build --build-arg MODE=$(ENV) --ssh default -t $(IMG):$(TAG)-$(ENV) .
	DOCKER_BUILDKIT=1 docker build --build-arg MODE=$(ENV) --ssh default -t $(IMG):latest-$(ENV) .

push: build
	docker push $(IMG):$(TAG)-$(ENV)
	docker push $(IMG):latest-$(ENV)

deploy: push
	kubectl apply -f kubernetes.yaml

undeploy:
	kubectl delete -f kubernetes.yaml
