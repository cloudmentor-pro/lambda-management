ID = media-info

# If you can override, exec `make LAMBDA_ENV=xxx`
LAMBDA_ENV = dev
TEMPLATE_FILE = template_$(LAMBDA_ENV).yaml
BUCKET_NAME = cmp-s3-lambda-$(LAMBDA_ENV)
STACK_NAME = cmp-cfn-lambda-$(ID)

all:
	make clean
	make build
	make package
	make deploy

.PHONY: build
build: ## build SAM template
	CGO_ENABLED=0 sam build --template-file $(TEMPLATE_FILE)

package: ## make a zip file and upload it to S3
	sam package --s3-bucket $(BUCKET_NAME) --s3-prefix $(ID) --output-template-file _$(TEMPLATE_FILE) --force-upload

.PHONY: deploy
deploy: ## execute CloudFormation
	sam deploy -t _$(TEMPLATE_FILE) --stack-name $(STACK_NAME) --no-confirm-changeset --no-fail-on-empty-changeset

clean: ## cleanup temporary dirs
	rm -rf .aws-sam _$(TEMPLATE_FILE)

watch: ## sync in realtime to cloud w/out updating cloudformation
	sam sync --stack-name $(STACK_NAME))  -t $(TEMPLATE_FILE) --watch


.PHONY: help
help: ## Display this help screen
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-20s\033[0m %s\n", $$1, $$2}'
