# Lambda-EC2-modify
Lambda + eventbridge for instance modify

# Passo 1: Criar policy
- Siga o path: IAM console > policy > create policy > select Json tab > apague todo o conteúdo e cole o conteúdo do "EC2 policy.txt" que está nesse repo.
- De um nome para a policy e clique em "create policy".

# Passo 2: Criar role
- Siga o path: IAM console > create role > selecione AWS service > lambda > vincule a policy criada no passo 1.
- De um nome para a role e clique em "create role".

# Passo 3: Criar Lambda Function
- Siga o path: Lambda console > create function > "author from scratch" > em "function name" nomeie como "modify-instance" ou algo semelhante > em "runtime" usaremos o NodeJS 16.x > expanda a seta onde está escrito "change default execution role" e selecione "use an existing role", selecione a role criada no passo 2 e clique em "create function".
- Configurar o código da lambda function. Abra a função lambda e na aba "index.js" apague o conteúdo e cole o conteúdo do "Lambda function.txt" que está nesse repo. Clique em "Deploy".
- Configurar o lambda. Vá para a aba configuration > general configuration > edit > mude o timeout para 10 minutos.

# Passo 4: Criar EventBridge
- Siga o path: EventBridge console > rules > create rule.
- Em create rule, de um nome para a rule e em "rule type" selecione "Schedule".
- Clique em "continue to create rule", passe as configurações de tempo no Cron expression e clique em next.
- Adicione o target. Selecione "AWS service", pesquise "Lambda Function" e selecione a function criada no passo 3.
- Expanda a seta "additional settings", selecione "Constant(JSON text)" e cole o conteúdo do "Constant eventbridge.txt" que está nesse repo.
- Em "retry attempts", coloque 40 e clique em Next e novamente em Next.
- Clique em create rule.

Após esses passos, o lambda+eventbridge já estará configurado e funcional para uso. Para testar o lambda, abra a lambda function criada e vá em "Test" > "configure test event" e em "event JSON" cole o conteúdo da sua constant do eventbridge.
