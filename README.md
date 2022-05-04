# Terraform AWS LocalStack

Esse repositório apresenta um exemplo prático do `LocalStack` com integração com o `Terraform` que pode ser acessado nesse link: [HotDog](https://dev.to/mrwormhole/localstack-with-terraform-and-docker-for-running-aws-locally-3a6d)

Após fazer o download do repositório executar os comandos abaixo, conforme necessidade.

## GoLang

Para preparar o ambiente para o build da aplicação, será necessário a versão `1.16` do `GoLang`, caso você tenha o `asdf` instalado, basta executar os comandos abaixo. Senão será necessário realizar a instalação pelo site oficial do [Go](https://go.dev/dl/)

```sh
$ asdf plugin-add golang https://github.com/kennyp/asdf-golang.git
$ asdf install golang 1.16
$ asdf local golang 1.16
```

Para realizar o build da aplicação executar o arquivo sh que consta na raiz do projeto.

```sh
$ ./zip-it.sh
```

## LocalStack

Para subir a infraestrutura da AWS localmente, execute:

```sh
$ docker network create localstack-tutorial
$ docker-compose up -d --build
```

Para deletar a infraestrutura, execute:
```sh
$ docker-compose down
$ docker network delete localstack-tutorial
```

## AWS

Considerando que você tenha já o `aws-cli` instalado e configurado corretamente, execute:

```sh
$ aws configure --profile localstack
```

adicione as credenciais conforme necessário:
```
para access_key : mock_access_key
para secret_key : mock_secret_key
para region : us-east-1
para output : json
```

Para verificar a criação dos recursos do sistema, após provisionar a infraestrutura pelo terraform, segue os passos:

```sh
$ aws --endpoint-url=http://localhost:4566 lambda list-functions --profile localstack
$ aws --endpoint-url=http://localhost:4566 dynamodb list-tables --profile localstack
$ aws --endpoint-url=http://localhost:4566 kinesis list-streams --profile localstack
```

Para executar e utilizar os recursos do sistema, execute o exemplo abaixo:
```sh
$ aws lambda invoke --function-name dogCatcher --endpoint-url=http://localhost:4566 --cli-binary-format raw-in-base64-out --payload '{"quantity": 2}' output.txt --profile localstack
$ aws dynamodb scan --endpoint-url http://localhost:4566 --table-name dogs --profile localstack
```

## Terraform

Considerando que você tenha já o client do terraform `terraform` instalado e configurado corretamente, execute:

```sh
$ terraform init
$ terraform plan
$ terraform apply -auto-approve
$ terraform destroy
```
