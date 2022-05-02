# Terraform AWS LocalStack

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

Considerando que você tenha já o client do terraform `terraform` instalado e configurado corretamente, execute:

```sh
$ terraform init
$ terraform plan
$ terraform apply -auto-aprove
$ terraform destroy
```
