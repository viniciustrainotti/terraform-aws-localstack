# Terraform AWS LocalStack

Esse repositório apresenta um exemplo prático de uma API em Go, que tem sua infraestrutura implantada no `LocalStack` com a utilização do `Terraform`. A referência deste projeto pode ser acessado neste link: [HotDog](https://dev.to/mrwormhole/localstack-with-terraform-and-docker-for-running-aws-locally-3a6d)

Após fazer o download do repositório executar os comandos abaixo, conforme necessidade.

## Requisitos

São necessárias as seguintes ferramentas/SKDs para executar este projeto:

- [Terraform](https://www.terraform.io/downloads)

- [AWS-CLI](https://aws.amazon.com/pt/cli/)
- [GoLang >1.16](https://go.dev/dl/)
Caso já possua o gerenciador `asdf` instalado, basta executar os comandos abaixo:
```sh
$ asdf plugin-add golang https://github.com/kennyp/asdf-golang.git
$ asdf install golang 1.16
$ asdf local golang 1.16
```

### Preparando o ambiente

Primeiro, vamos realizar o build da aplicação executando o arquivo sh que consta na raiz do projeto.

```sh
$ ./zip-it.sh
```

## LocalStack

Para subir a infraestrutura da AWS localmente através do LocalStack, execute:

```sh
$ docker network create localstack-tutorial
$ docker-compose up -d --build
```


Para deletar a infraestrutura, execute:
```sh
$ docker-compose down
$ docker network delete localstack-tutorial
```

## Terraform

Através do `terraform`, vamos criar a infraestrutura que será utilizada pela aplicação em si executando os seguintes comandos:

```sh
$ terraform init
$ terraform plan
$ terraform apply -auto-approve
```

Ao terminar os testes, o comando abaixo irá excluir a infraestrutura criada pelo `terraform`.
```sh
$ terraform destroy
```

## AWS-CLI

Para configurar um perfil para o LocalStack no seu AWS-CLI, rode o seguinte comando:

```sh
$ aws configure --profile localstack
```

E adicione as credenciais de teste conforme necessário:
```
para access_key : mock_access_key
para secret_key : mock_secret_key
para region : us-east-1
para output : json
```

Para verificar a criação dos recursos do sistema, após provisionar a infraestrutura pelo Terraform, execute os seguintes comandos:

```sh
$ aws --endpoint-url=http://localhost:4566 lambda list-functions --profile localstack
$ aws --endpoint-url=http://localhost:4566 dynamodb list-tables --profile localstack
$ aws --endpoint-url=http://localhost:4566 kinesis list-streams --profile localstack
```

Para executar e utilizar os recursos do sistema, execute os comandos abaixo:
```sh
$ aws lambda invoke --function-name dogCatcher --endpoint-url=http://localhost:4566 --cli-binary-format raw-in-base64-out --payload '{"quantity": 2}' output.txt --profile localstack
$ aws dynamodb scan --endpoint-url http://localhost:4566 --table-name dogs --profile localstack
```

