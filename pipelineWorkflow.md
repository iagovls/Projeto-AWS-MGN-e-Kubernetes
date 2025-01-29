## Pipeline Terraform + Github actions + AWS

<div align="center">

![diagrama 1](/icons/gitAWS.drawio.png)

**Workflow pipeline**

</div>

- Os desenvolvedores criam toda a infra no Terraform, e essa infra é colocado no repositório github.
- Dentro do **github repository** costuma-se ter branchs distintas para cada ambiente (Dev, HML /QA, Production).
- Sempre que tivermos um código mergeado pra branch de develop, é feito o deploy desse código para a branch de homologação ou QA, na qual serão feitos os testes e se tudo estiver OK, se for homologado, é feita a implantação no ambiente de produção, para isso fazemos o merge da branch de **hml** para a **main** e com isso a pipeline do github actions é acionada e faz esse deploy da infraestrutura criada no Terraform(no github) para o ambiente de produção/**production**.

<div align="center">

![diagrama 2](/icons/workflow.png)

**Workflow pipeline**

</div>
