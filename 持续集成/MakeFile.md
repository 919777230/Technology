
## 代码
```base
.PHONY: docker-dev
version := $(shell xmllint --xpath '/*[local-name()="project"]/*[local-name()="version"]/text()' pom.xml)
current_dir := $(shell basename "$(CURDIR)")

package:
	mvn -Pdev clean package -Dmaven.test.skip=true

install:
	mvn -Pdev clean install -Dmaven.test.skip=true

deploy:
	mvn -Pdev clean deploy -Dmaven.test.skip=true

install-client:
	mvn -Pdev clean install -f client_pom.xml -Dmaven.test.skip=true

deploy-client:
	mvn -Pdev clean deploy -f client_pom.xml -Dmaven.test.skip=true

run:
	mvn spring-boot:run -Dmaven.test.skip=true

tag:
	$(eval commit := $(shell git rev-parse --short=8 HEAD))
	$(eval tag := $(shell mvn help:evaluate -Dexpression=project.version | grep -v "^\[" | grep -v "Download" | awk '{print $$0"-$(commit)"}'))
	git tag -a $(tag) -m "Branch: `git branch | grep \* | awk '{print $$2}'`"
	git push origin refs/tags/$(tag)

release:
	rm -f release.properties pom.xml.releaseBackup
	rm -f */release.properties */pom.xml.releaseBackup
	mvn release:prepare -B
	mvn release:perform
	rm -f release.properties pom.xml.releaseBackup
	rm -f */release.properties */pom.xml.releaseBackup

docker-dev:
	mvn -Pdev clean package -Dmaven.test.skip=true
	docker build --build-arg spring_profiles_active=dev -t $(current_dir):latest .
	docker tag $(current_dir):latest 172.16.1.10:5000/$(current_dir):latest
	docker push 172.16.1.10:5000/$(current_dir):latest

docker-prod:
	mvn -Pprod clean package -Dmaven.test.skip=true
	docker build --build-arg spring_profiles_active=prod -t $(current_dir):$(version) .
	docker tag $(current_dir):$(version) 172.16.1.10:5000/$(current_dir):$(version)
	docker push 172.16.1.10:5000/$(current_dir):$(version)
```
