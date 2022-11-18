# SMU 2022.2

Material de Referência: [AWS > Documentation > AWS CloudFormation > User Guide > Sample templates > South America (Sao Paulo) region > Amazon EC2](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/sample-templates-services-sa-east-1.html#w2ab1c33c50c13c13).

## SSH

Criar um par de chaves pública e privada (pode usar par de chaves próprio, legado):

```sh
ssh-keygen -t ed25519 -C <nome-da-chave> -f <arquivo>
```

## AWS IAM

Adicionar as seguintes permissões ao grupo de operadores:

- `AmazonEC2FullAccess`
- `AmazonVPCFullAccess`
- `AmazonSSMReadOnlyAccess`
- `AWSCloudFormationFullAccess`

## Gitpod

Criar as seguintes variáveis de ambiente:

- `AWS_ACCESS_KEY_ID`: ID da chave de acesso associada a um usuário IAM.
- `AWS_SECRET_ACCESS_KEY`: chave secreta associada ao ID (item anterior).
- `AWS_DEFAULT_REGION`: região definida como padrão ao criar os objetos na AWS.
- `SSH_PRIVATE_KEY_AWS`: conteúdo da chave privada. As quebras de linha devem ser convertidas para `|`, de forma que seja possível converter de volta (ver comando `tc` em `.gitpod.yml`).

Em todos os itens anteriores, o escopo da varivável pode ser restrito ao repositório de trabalho (`boidacarapreta/smu20222`, por exemplo) ou manter o valor padrão (`*/*`).

## Gitpod + AWS EC2

Importar a chave pública (se ainda não o fez) com:

```sh
aws ec2 import-key-pair --key-name <nome-da-chave> \ 
--public-key-material fileb://<arquivo>
```

## Gitpod + AWS CloudFormation

O arquivo `Makefile` automatiza o processo. Manualmente, o processo para criar é:

```sh
aws cloudformation create-stack \
--stack-name <nome-da-pilha> \
--template-body file://ec2+eip.json \
--parameters ParameterKey=KeyName,ParameterValue=<nome-da-chave>
````

onde `<nome-da-chave>` é o mesmo valor informado para criar a chave pública em [EC2](#ec2). Para destruir:

```sh
aws cloudformation delete-stack --stack-name <nome-da-pilha>
```
