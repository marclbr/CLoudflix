Infraestrutura por trás do FrontEnd da Netflix
Uma breve descrição sobre o que esse projeto faz, mostra com base de estudos o princípio de funcionamento da infraestrutura da maior rede de Stream a NETFLIX.
Em um bootcamp no qual tive o prazer de participar aprendi como fica fácil montar essa Infraestrutura, diante da grandeza oferecida pela Netflix, onde temos uma aplicação rodando como FrontEnd, como intuito de oferecer aos usuários segurança e diversão, tendo em vista que no FrontEnd temos a parte do usuário acessar o sistema e ser autenticado, e a parte interna, após ser autenticado, onde temos milhões de escolhas de filmes.
OBJETIVO
Como foi dito esse projeto tem Como intuito o apenas para estudos, vamos montar uma Infraestrutura dentro da AWS, utilizado algumas ferramentas básicas, e utilizando o menor recurso para não termos um custo grande no projeto.
Parte 1 - PREPARANDO O AMBIENTE DA NETFLIX NA NUVEM DA AMAZON AWS.
Aqui vamos começar a montar a estrutura, seria ideal conhecer um pouco do ambiente da AWS, para dar inicio as configurações, onde só irei passar o passa a passo, mas não com requintes de detalhes.
Segue a estrutura de exemplo abaixo :
Vamos lá então:
1 - Criando a Instância AWS EC2
Vamos criar uma Instância, utilizando o template do sistema operacional Linux - Amazon Linux, tomar como base, após a instalação vamos acessar a instancia e configurar algumas parâmetros importantes :
 

yum update-y
yum install httpd -y 
chkconfig httpd on 
service httpd start 
cd /var/www/html vim index.html
Obs. Esse arquivo index.html vai receber um código simples para armazenar os vídeos que iria ser utilizados por cada um.
<html><h1><body bgcolor="#F59411">CLOUDFLIX NA AWS</body></h1> <video controls="controls" controsList="nodownload" width="640" height="360"> <souce src="https://XXXXXXXXX.cloudfront.net/XXXXXX" type="video/mp4"></video> <video controls="controls" controsList="nodownload" width="640" height="360"> <souce src="https://XXXXXXXXX.cloudfront.net/XXXXXX" type="video/mp4"></video> </html>

Esse endereço será substituído pelo endereço do CloudFront após ele ser instalado e configurado junto ao AWS S3.
2 - Vamos criar o CloudFront
O Amazon CloudFront - O CloudFront é um serviço de rede de entrega de conteúdo (CDN) rápido, altamente seguro e programável. O CloudFront pode fornecer seu conteúdo de modo seguro por HTTPS a partir de todos os locais da borda do CloudFront.
 

O cache do CloudFront reduz o número de solicitações às quais seu servidor de origem deve responder diretamente. Quando um visualizador (usuário final) solicita um vídeo que você veicula com o CloudFront, a solicitação é encaminhada para um local da borda mais próximo de onde o visualizador está localizado.
3 - Vamos criar o armazenamento AWS S3
 
O Amazon Simple Storage Service (Amazon S3) é um serviço de armazenamento de objetos que oferece escalabilidade, disponibilidade de dados, segurança e performance líderes do setor. Clientes de todos os portes e setores podem armazenar e proteger qualquer quantidade de dados de praticamente qualquer caso de uso, como data lakes, aplicações nativas da nuvem e aplicações móveis.
Parte 2 - CONFIGURANDO A NUVEM AWS PARA MANTER O NETFLIX ONLINE 24x7
Agora vamos fortalecer a infraestrutura com o balaceamento e cargas e a auto disponibilidade, para depois só tratarmos da segurança.
Começamos com o ELB - Elastic Load Balancer:
 
O Amazon Load balancer , é um balanceador de carga que distribui o trafego de redes entre os servidores, para aumentar a escalabilidade e alta disponibilidade de sua aplicação. Vamos utilizar o Application Load Balancer, já que estamos trabalhando com HTTP e HTTPS.
Iremos criar também uma imagem da instancia do EC2, para os próximos passos, que será a criação do AutoScaling, e a alta disponibilidade, podendo crescer ou diminuir o número de servidores dependendo da configuração e métricas utilizadas para o uso do AutoScaling, nesse caso usaremos o processamento dos servidores no uso da aplicação(CPU).
Vamos criar um Security Group para validarmos os acessos as instancias.
Vamos criar o AutoScaling :
 
O AutoScaling é uma ferramenta muito importante para escalabilidade de ambiente dentro da AWS. Ele detecta automaticamente falta de recursos nas EC2, ECS e EKS e adiciona mais capacidade computacional, sempre que for necessário para acomodar os usúarios em seu site. Você pode simplificar a escalabilidade de ambientes criticos por meio de recomendações e escalar automaticamente os seus recursos fazendo com que seu ambiente não sinta diferença alguma.
Vamos configurar agora o serviço de DNS , onde deixaremos a aplicação com nossa cara(Dominio proprio), utilizaremos o serviço Route53, um serviço de DNS publico da AWS, no qual foi proparado para ser altamente disponivel, escalavél e imutavél. Um serviço que foi basicamente criado para fornecer resolução de DNS público para os serviços da AWS.
 
Agora vamos proteger nossa aplicação com o serviço AWS WAF, basicamente um Firewall que vai ficar a frente do CloudFront protegendo entrada e saida de ataques mais comuns a internet, o principal deles DDOS. Esse serviço irá proteger suas aplicações WEB e APIs contra bots e exploits comuns da Web que podem afetar sua disponibilidade, ele permite que seja criado regras e também disponibiliza as suas próprias já prontas para o uso, porém como nosso projeto é apenas para estudos, vamos utilizar apenas uma regra básica de bloqueio, tendo em vista que ele é um serviço bastante caro se usado bem configurado.

 


Espero que todos que acompanharem esse passo a passo e que entendam um pouco de AWS, possam ver o quanto é simples colocar esse ambiente em produção.
