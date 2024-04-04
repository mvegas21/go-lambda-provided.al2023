# go-on-provided.al2023

To run Go functions on 
###  Lambda OS-only Runtime | Amazon Linux 2023 | provided.al2023

Compiling:
First create an boostrap file with this command:

```
GOARCH=amd64 GOOS=linux go build -o bootstrap main.go
```

then zip the file

```
zip bootstrap.zip bootstrap
```
(zip archive.zip file-name)

the file must be in the root directory

all the proccess can be automate in CI/CD

Example:
```
stages:
  - code
  - test

    stage: code
    image: golang:1.21.1
  before_script:
    - go get github.com/aws/aws-lambda-go/lambda
  script:
    - GOARCH=amd64 GOOS=linux go build -tags lambda.norpc -o bootstrap main.go
  artifacts:
    paths:
      - bootstrap

  stage: test
  image: alpine
  dependencies:
    - build  
  before_script:
    - apk add --update zip curl
  script:
    - zip bootstrap.zip bootstrap
  artifacts:
    paths:
      - bootstrap.zip
    when: on_success 
```


I hope this helps.

Ref: https://aws.amazon.com/cn/blogs/compute/migrating-aws-lambda-functions-from-the-go1-x-runtime-to-the-custom-runtime-on-amazon-linux-2/