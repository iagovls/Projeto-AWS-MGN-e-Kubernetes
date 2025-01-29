## Pipeline Terraform + Github actions + AWS

<div align="center">

![diagrama 1](/icons/gitAWS.drawio.png)

**Workflow pipeline GitHub**

</div>

- Os desenvolvedores criam toda a infra no Terraform, e essa infra é colocado no repositório github.
- Dentro do **github repository** costuma-se ter branchs distintas para cada ambiente (Dev, HML /QA, Production).
- Sempre que tivermos um código mergeado pra branch de develop, é feito o deploy desse código para a branch de homologação ou QA, na qual serão feitos os testes e se tudo estiver OK, se for homologado, é feita a implantação no ambiente de produção, para isso fazemos o merge da branch de **hml** para a **main** e com isso a pipeline do github actions é acionada e faz esse deploy da infraestrutura criada no Terraform(no github) para o ambiente de produção/**production**.

<div align="center">

![diagrama 2](/icons/workflow.png)

**Workflow pipeline GitHub AWS**

</div>

- Quando os desenvolvedores fizerem o merge e acionarem o github actions, o repositório será clonado.
- Para que o Terraform seja executado dentro da AWS, é necessário configurar as linhas de comando(CLIs) da AWS e Terraform.
- Após a configuração das linhas de comando, é necessário fazer uma conexão entre o GitHub Actions e a conta AWS, e para isso é feita uma configuração via Assume Role(forma segura de configurar a pipeline, para que a conexão com a nossa conta AWS não precise de credenciais fixas).
- Neste ponto, a pipeline adquire um Lock de Execução para não permitir problemas de execução paralelos.
- Executa-se o Terraform Plan & Apply.
- Por fim, armazena-se ou atualiza-se o StateFile dentro de um S3 Bucket.

Finaliza-se assim, o workflow do processo de Deply da Infra Terraform dentro da AWS.
