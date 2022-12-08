# Terraform AWS LocalStack

Esse repositório apresenta o exemplo básico do docs do `LocalStack` com integração com o `Terraform` que pode ser acessado nesse link: [Integration Terraform](https://docs.localstack.cloud/integrations/terraform/)

Após fazer o download do repositório executar os comandos abaixo, conforme necessidade.

## LocalStack

Para subir a infraestrutura da AWS localmente, execute:

```sh
$ docker-compose up -d --build
```

Para deletar a infraestrutura, execute:
```sh
$ docker-compose down
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

Para consultar um recurso, segue o exemplo:

```sh
$ aws --endpoint-url=http://localhost:4566 s3 ls --profile localstack
```

## Terraform

Considerando que você tenha já o client do `terraform` instalado e configurado corretamente, execute:

```sh
$ terraform init
$ terraform plan
$ terraform apply -auto-approve
$ terraform destroy
```

## Example

Para copiar e visualizar uma imagem no bucket, segue os comandos:

URL: http://localhost:4566/my-bucket-localstack/pug.png

```sh
$ aws --endpoint-url=http://localhost:4566 s3 ls --profile localstack
$ aws --endpoint-url=http://localhost:4566 s3 cp imgs/pug.png s3://my-bucket-localstack/ --profile localstack
$ aws --endpoint-url=http://localhost:4566 s3 ls s3://my-bucket-localstack --recursive --human-readable --summarize --profile localstack
$ aws --endpoint-url=http://localhost:4566 s3 rm s3://my-bucket-localstack/pug.png --profile localstack
```

