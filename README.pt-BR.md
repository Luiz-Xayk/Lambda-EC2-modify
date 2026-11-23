🇺🇸 [English](README.md) | 🇧🇷 [Português](README.pt-BR.md)

# Lambda-EC2-modify

AWS Lambda + EventBridge para alterar o tipo de uma instância EC2 de forma agendada.

A função Lambda para a instância, aguarda até que ela esteja parada, altera o tipo da instância e a inicia novamente. A região, o ID e o novo tipo da instância são fornecidos pela regra do EventBridge como um input JSON constante.

## Arquivos do Repositório

- `EC2 policy.txt`: política IAM (JSON) para a role da Lambda.
- `Lambda Function.txt`: código da função Lambda (Node.js).
- `Constant eventbridge.txt`: input JSON constante enviado pela regra do EventBridge.

## Passo 1: Criar a Policy

- Siga o caminho: IAM console > Policies > Create policy > selecione a aba **JSON** > apague todo o conteúdo e cole o conteúdo do `EC2 policy.txt` deste repositório.
- Dê um nome para a policy e clique em **Create policy**.

## Passo 2: Criar a Role

- Siga o caminho: IAM console > Create role > selecione **AWS service** > **Lambda** > vincule a policy criada no Passo 1.
- Dê um nome para a role e clique em **Create role**.

## Passo 3: Criar a Lambda Function

- Siga o caminho: Lambda console > Create function > **Author from scratch** > em **Function name**, nomeie como `modify-instance` ou algo semelhante > em **Runtime**, selecione **Node.js 16.x** > expanda **Change default execution role**, selecione **Use an existing role**, escolha a role criada no Passo 2 e clique em **Create function**.
- Configure o código da função: abra a função Lambda, apague o conteúdo da aba `index.js` e cole o conteúdo do `Lambda Function.txt` deste repositório. Clique em **Deploy**.
- Configure a função: vá para **Configuration** > **General configuration** > **Edit** > altere o timeout para 10 minutos.

## Passo 4: Criar a Regra do EventBridge

- Siga o caminho: EventBridge console > Rules > Create rule.
- Dê um nome para a rule e, em **Rule type**, selecione **Schedule**.
- Clique em **Continue to create rule**, informe o agendamento como expressão cron e clique em **Next**.
- Adicione o target: selecione **AWS service**, pesquise **Lambda function** e selecione a função criada no Passo 3.
- Expanda **Additional settings**, selecione **Constant (JSON text)** e cole o conteúdo do `Constant eventbridge.txt` deste repositório (preenchido com os dados da sua instância).
- Em **Retry attempts**, coloque 40 e clique em **Next** duas vezes.
- Clique em **Create rule**.

Após esses passos, o Lambda + EventBridge estará configurado e pronto para uso. Para testar a função Lambda, abra-a e vá em **Test** > **Configure test event** e, em **Event JSON**, cole o conteúdo da sua constant do EventBridge.
