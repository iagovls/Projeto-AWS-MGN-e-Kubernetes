## AWS WAF, CloudFront CDN e o Route 53

<div align="center">

![diagrama](/icons/albIgw.png)

**Diagrama de exemplo, representando a interação dos componentes de rede**

</div>

- Quando o Usuário acesso o DNS(Domain Name System), ele está fazendo uma requisição DNS no endereço https://example.com, o tráfego é enviado para o Route 53.
- O Route 53, é um serviço DNS que traduz nomes de domínio em endereço IP, permitindo que os usuário acessem a aplicação sem precisar lembrar de IPs, sem o Route 53 os usuários tem que acessar a aplicação pelo endereço IP.
- O Route 53 tem uma lista com o DNS - IP, e ele verifica se o nome de domínio faz match com o IP da lista, se sim, ele faz esse redirecionamento.
- O Route 53 responde com o IP do CloudFront, o CloudFront é um CDN(Content Delivery Network)

<div align="center">

![diagrama](/icons/cdn.png)

**Diagrama de exemplo, representando o uso de um CDN**

</div>

- O CloudFront é um CDN que acelera a entrega do seu site ou API distribuindo o conteúdo em servidores ao redor do mundo(os servidores edge). Ele melhora a velocidade, segurança e confiabilidade do trafégo.

- O CloudFront vai receber a requisição e passar para o WAF.
- O WAF(WEB Application Firewall) protege contra ataques como SQL Injection, Cross-Site Scripting (XSS), ataques de força bruta e bots.
- O WAF vai encaminhas a requisição para o Internet Gateway(IGW), e o internet gateway permite que a aplicação se comunique com a internet e receba trafégo. Ele vai redirecionar o trafégo de entrada para o ALB.
- O ALB vai distribuir a carga entre os pods no EKS cluster, garantindo a escalabilidade e alta disponibilidade.

E com isso vimos o papel do Route 53, CloudFront, WAF, IGW e ALB na Infra.
