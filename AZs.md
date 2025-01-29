## Descrição componentes em cada zona.

Expliacação dos componentes das zonas:

- US-EAST-A.
- US-EAST-1B.
- US-EAST-1C.

---

### US-EAST-1A (Ambiente de Dev e HML/ QA)

<div align="center">

![alt text](/icons/us-east-1a.png)

**us-east-1a**

</div>

- A zona de disponibilidade us-east-1a, é utilizada como ambiente para os Devs e HML(Homologação) /QA.
- Nessa zona têm-se três subnetes:

1. **Public subnet**: Conectada a um Nat Gateway, permitindo assim que as subnetes privadas tenham acesso à internet.

> [!important]
> A utilização de subnetes privadas para isolar as aplicações, adiciona uma camada de segurança, pois os componentes dentro da mesma, não podem ser acessados externamente a não ser que seja permitido através da configuração dos SG e Internet Gateway.

2. **Private Subnet 1**: Nela se encontram os dois ambientes, Dev e HML /QA, os quais não são expostos para clientes e são usados apenas para as etapas de desenvolvimento e testes.
   2.1 Dentro de cada um dos ambientes, há dois grupos de segurança(security groups-sg):

- **sg-1**: Permite acesso para a aplicação, possui um Pod com a aplicação React, o Frontend.
- **sg-2**: Permite acesso apenas para a aplicação Frontend, para que consiga ter acesso as APIs, na arquitetura original, tinha-se uma instância para todas as APIs, entretanto, quando entramos nos conceitos de Kubernetes e microserviços, cada Pod der ter apenas uma função, então como haviam 3 APIs, foram criados 3 Pods, um para cada API.

3. **Private Subnet 2**: Têm-se um banco RDS read/write(leitura e gravação) utilizado pelos dois ambientes para suas respectivas atividades.

---

### US-EAST-1B e US-EAST-1C (Ambientes de Produção/Production)

<div align="center">

![alt text](/icons/bc.png)

**us-east-1b e us-east-1c**

</div>

Esses dois ambientes são os ambientes que os clientes acessam.

- Essas duas AZs são praticamente identicas, a razão/motivo de utilizar duas zonas iguais, é o obetivo de ter uma alta disponibilidade.
- Um ponto a mais, é que os bancos RDS são multi-az, ou seja, eles vão ter um efeito Failover, então, caso um dos bancos venha a cair, o outro tomará o seu lugar, garantindo assim uma alta disponibilidade e resistência à falhas para a Infra.
- Nesse caso, esses dois ambientes serão disponibilizados para os clientes.

Com isso conseguimos ver de forma simplificada e detalhada cada uma das AZs da Infra.
