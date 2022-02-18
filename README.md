### 1º passo: criar o arquivo de políticas de segurança
> `policies.json`

### 2º passo: criar o role de segurança na AWS
```bash
aws iam create-role \
	--role-name lambda-example \
	--assume-role-policy-document file://policies.json \
	| tee logs/role.log
```

### 3º passo: criar um arquivo com o conteúdo e zipá-lo
```bash
zip function.zip index.js
```

### 4º passo: criar a lambda
```bash
aws lambda create-function \
	--function-name hello-cli \
	--zip-file fileb://function.zip \
	--handler index.handler \
	--runtime nodejs12.x \
	--role <role_arn> \
	| tee logs/lambda-create.log
```

### 5º passo: invoke lambda
```bash
aws lambda invoke \
	--function-name hello-cli \
	--log-type Tail \
	logs/lambda-exec.log
```

### Atualizar, zipar
```bash
zip function.zip index.js
```

### Atualizar lambda
```bash
aws lambda update-function-code \
	--zip-file fileb://function.zip \
	--function-name hello-cli \
	--publish \
	| tee logs/lambda-update.log
```

### Invokar e ver resultado
```bash
aws lambda invoke \
	--function-name hello-cli \
	--log-type Tail \
	logs/lambda-exec.log
```

### Deletando
```bash
aws lambda delete-function \
	--function-name hello-cli

aws iam delete-role \
	--role-name lambda-example
```
