.PHONY: all push

USER="501stalpha1"
ifeq ($(shell uname -m), i686)
  PLATFORM="386"
else
  PLATFORM="amd64"
endif
FULL_VERSION="5.7.17"
MINOR_VERSION=$(shell echo $(FULL_VERSION) | cut -d '.' -f -2)
MAJOR_VERSION=$(shell echo $(FULL_VERSION) | cut -d '.' -f 1)

all: .last-docker-build

.last-docker-build: Dockerfile
	docker build -t "$(USER)/mysql:$(FULL_VERSION)-$(PLATFORM)" .
	docker tag "$(USER)/mysql:$(FULL_VERSION)-$(PLATFORM)" "$(USER)/mysql:$(MINOR_VERSION)-$(PLATFORM)"
	docker tag "$(USER)/mysql:$(FULL_VERSION)-$(PLATFORM)" "$(USER)/mysql:$(MAJOR_VERSION)-$(PLATFORM)"
	docker tag "$(USER)/mysql:$(FULL_VERSION)-$(PLATFORM)" "$(USER)/mysql:latest-$(PLATFORM)"
	@touch $@

push: .last-docker-push

.last-docker-push: .last-docker-build
	docker push "$(USER)/mysql:$(FULL_VERSION)-$(PLATFORM)"
	manifest-tool push from-spec ./manifest-full.yml
	docker push "$(USER)/mysql:$(MINOR_VERSION)-$(PLATFORM)"
	manifest-tool push from-spec ./manifest-minor.yml
	docker push "$(USER)/mysql:$(MAJOR_VERSION)-$(PLATFORM)"
	manifest-tool push from-spec ./manifest-major.yml
	docker push "$(USER)/mysql:latest-$(PLATFORM)"
	@touch $@
