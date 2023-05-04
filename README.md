# Introdução

Essa wiki serve como guia e documenta todas a implementação de um servidor utilizando o sistema operacional [Ubuntu Server](https://ubuntu.com/server) proposto pelo professor [Paulo Henrique Sousa Barbosa](https://github.com/agenteph) na disciplina de Redes de Computadores II do curso de Ciência da Computação do Instituto Federal de Educação, Ciência e Tecnologia do Maranhão - Campus Imperatriz (IFMA). Projeto por [Pedro Fernandes Bahury](https://github.com/pfbahury)

Para o prosseguimento dentro deste "guia", serão necessário algumas preparações antes de qualquer coisa, no caso, alguns softwares serão necessarios para realizar as ações descritas, detalhe, este projeto está sendo realizado no sistema operacional Windows 11, então no caso de usuario com um sistema linux, alguns dos passos descritos neste guia, e alguns programas externos podem não ser necessários.

Para realizar as atividades será necessario a utilização de uma maquina virtual, ou [VirtualBox](https://www.virtualbox.org), e uma imagem `.iso` do sistema operacional que iremos utilizar, neste caso, o [Ubuntu Server](https://ubuntu.com/download/server)

#Configurando a Maquina virtual

Logo de começo, recomendo um guia de criação de uma maquina virtual, para os que não possuem o costume de utilizar uma, recomendo o seguinte [guia](https://tecnoblog.net/responde/como-criar-uma-maquina-virtual-virtualbox/)

AVISO!!
Antes de prosseguir com o uso do arquivo `.iso` será necessario fazer as configurações de rede.

## Configurando a rede

Após criar a sua maquina virtual, clique na engrenagem amarela para acessar as configuração gerais da maquina.

![image](https://user-images.githubusercontent.com/90939515/236021214-17c1840b-55c4-43d9-a5e3-ef9f807140ee.png)

Dentro das configurações, clique nas aba de **rede**

![image](https://user-images.githubusercontent.com/90939515/236023130-c4ee3bc4-07a0-4503-bed7-b54f32ca43b3.png)

As configurações estarão como padrão em modo NAT, mude para modo bridge, e dentro das configurações avançadas, mude o **Modo Promícuo**, para **Tudo Permitido**, deixando do mesmo jeito que a imagem.

![image](https://user-images.githubusercontent.com/90939515/236036819-bc44dba1-afed-43c0-8e53-5ffecafbbf5f.png)

> Mas o que seria esse modo NAT?

NAT (Network Address Translation) é uma técnica usada em redes de computadores para permitir que vários dispositivos em uma rede local compartilhem um único endereço IP público na Internet.

O NAT ativado dentro das configurações, entra em conflito com o endereço da maquina principal, sem ser a virtual, ao ativar o `Modo Bridge` o NAT é desabilitadoe permite essa junção de IP's sem conflito, pois ele serve como ponte para conexão de endereços.

Com essa configuração feita, estamos prontos para configurar a sua ISO.

## Configurando a Maquina (Agora de verdade)

Assim que você colocar seu arquivo `.iso` na maquina, e iniciar a instalação, as configurações irão se iniciar, começando pela linguagem.

![image](https://user-images.githubusercontent.com/90939515/236041834-cc4094af-338a-4b95-8435-ea6f64f8cd33.png)

Logo em seguida vem a definição de linguagem do teclado

![image](https://user-images.githubusercontent.com/90939515/236050102-2eced51e-d920-4aac-a1b3-960d5fd3fa93.png)

Após isso vem a escolha de instalação do ubuntu, neste caso foi selecionado, o `Minimized` para ser um processo mais rápido.

![image](https://user-images.githubusercontent.com/90939515/236051160-f5c3d1d4-5a74-499c-9687-9361a2cf6bba.png)

Nas seguintes configurações, siga apenas em frente, sem mudar nenhuma informação

Ao chegar na sessão de configurar o perfil, deve ser definido o nome de usuario, nome de servidor e as senhas para poder manter acesso dentro do ubuntu

![image](https://user-images.githubusercontent.com/90939515/236051875-dc1bc03f-570e-41c2-9825-a7be0220e953.png)

**Pule a instalação do ubuntu pro**, ja que apesar de sua utilidade, nosso projeto não tem a necessidade de mais complicação (haha), e selecione a instalação do ssh

![image](https://user-images.githubusercontent.com/90939515/236052171-216a4e19-e5aa-4604-9655-00cefd7b5a25.png)

Durante a instalação, existem programas extras que podem ser baixados, porém neste caso não iremos precisar, pelo mesmo motivo do Ubuntu Pro, então pularemos essa etapa.

![image](https://user-images.githubusercontent.com/90939515/236052794-d5a4b065-1db7-44e8-990b-7fa3f4dbc6a4.png)

Logo em seguida a instalação de verdade se iniciara, e logo depois podemos colocar para fazer o Reboot assim que disponibilizado

![image](https://user-images.githubusercontent.com/90939515/236053448-f4f88cc2-fe91-484b-9eb2-0ffcaa9aa253.png)

Assim que a instalação terminar, você terá finalmente disponivel o seu ubuntu server para uso! Você vera uma cena parecida com essa

![image](https://user-images.githubusercontent.com/90939515/236054945-61f27a30-cbf5-473f-be96-f032c759662c.png)

Faça seu login, e PARABÉNS! Agora seu ubuntu server está instalado, e podemos começar a configurar o servidor.

# Configurando o Servidor

## Configurações Iniciais

Para inicio, alguns programas terão de ser instalados dentro do Ubuntu. Apesar da interface gráfica se resumir apenas em um terminal, ainda temos acesso a arquivos e diretórios para poder acessar diferentes programas e funções dentro do sistema. Para os não acostumados a utilizar um sistema Linux, todas as ações como instalar, abrir arquivos, editar, compactar, entre outros, são realizados com comandos dentro do terminal.
Já como introdução, podemos utilizar o comando `sudo su` para acessar a pasta root (ou raíz) do sistema.

> Aviso ⚠️: apesar de utilizarmos constantemente o sistema na pasta root, não é recomendado o uso constante do sistema neste modo em situações normais, ja que qualquer mudanças realizadas ao seu sistema poderá ser feito sem a necessidade de senha de usuário, deixando seu sistema completamente exposto a usuarios mal intencionados.

Todos os comandos do Linux possuem significados, vamos usar o que acabamos de comentar de exemplo:

**su**: Sigla para SUPER USER, ou super usuario, ou usuario com todos os privilégios no dentro do sistema.

**sudo**: Significa SUPER USER DO, ou super usuario faça, é um comando que permite que seja realizado ações que necessitam uma permissão por um super usuario.

Ou seja, ao dizer `sudo su`, seria dizer SUPER USER DO SUPER USER, fazendo com que você constantemente esteja com todos os privilégios liberados para acessar qualquer ação, e com acesso a pasta root. Existem diversos outros, porém, por questão de deixar o guia mais dinamico, não será tão focado a explicação todos os comandos. Para os usuários que buscam descobrir o que todos os comandos fazem, recomendo checar o [guia linux](https://guialinux.uniriotec.br) 👍

![Captura de tela 2023-04-18 154345](https://user-images.githubusercontent.com/90939515/236091294-5b006b93-35d5-431e-b15c-f0c4a0629e17.png)

Em seguida, vamos precisar atualizar nosso `apt`, que é o serviço de gerenciamento de pacotes padrão do Linux que iremos utilizar para instalar todos os outros utilitarios e serviços dentro do servidor. 

![Captura de tela 2023-04-18 154625](https://user-images.githubusercontent.com/90939515/236091605-77dddc1a-1bba-4748-adf9-2ca2e959dda9.png)

Como já comentamos de utilitários, podemos instalar mais alguns de utilidade para o servidor, para isso utilizaremos o seguinte comando: `apt install inetutils-ping net-tools openssh-server`

E o que são esses pacotes?

Nano: é um editor de texto simples e fácil de usar, com recursos básicos como edição, busca e substituição de texto, que utilizaremos para editar configuração de diversos arquivos.

Inetutils-ping: é uma ferramenta que permite testar a conectividade da rede, enviando pacotes de dados para um endereço IP e esperando por uma resposta.

Net-tools: é um conjunto de ferramentas de rede que inclui comandos como ifconfig, route e netstat, que permitem visualizar informações de rede, como endereços IP, rotas e conexões abertas.

OpenSSH-server: é um servidor de protocolo SSH que permite o acesso remoto seguro a um computador ou servidor Linux.

![Captura de tela 2023-04-18 155324](https://user-images.githubusercontent.com/90939515/236096338-6c688348-dd5b-48e8-b783-f7cd70b921bf.png)

# Serviço SSH

Para iniciar a configuração do SSH, iremos no arquivo de configuração, utilizando o comando `nano` e em seguida o endereço do arquivo: `/etc/ssh/sshd_config` para editar o texto do arquivo.

![image](https://user-images.githubusercontent.com/90939515/236104334-4bb34ef3-8863-4968-b298-28b6c74dbe18.png)

Dentro do arquivo, terá uma linha escrita `#Porta 22`, retirando o #, a linha ficaria descomentada, fazendo ela ser executada pelo arquivo, logo em seguida, mude a porta, no meu caso, coloquei como 222.

![Captura de tela 2023-04-18 171348](https://user-images.githubusercontent.com/90939515/236104525-52e0bce9-9e37-4926-a148-d74f8443374f.png)

Aperte Ctrl+X para sair e confirme em salvar o arquivo, logo em seguida, o serviço deve ser reiniciado, para isso utilizamos o comando `/etc/init.d/ssh restart`

![Captura de tela 2023-04-18 171437](https://user-images.githubusercontent.com/90939515/236104663-d2f16319-3823-438c-9326-0a0aa5193b16.png)

Para descobrir se o estado do serviço SSH está funcionando como deveria, utilizamos o comando `/etc/init.d/ssh status`

![image](https://user-images.githubusercontent.com/90939515/236105018-609ac877-1c92-408d-8574-056ed2833ce5.png)

## Acessando o SSH

Para fazer a conexão, precisamos do endereço de IP do servidor, para isso utilizamos o comando `ipconfig`

Descobrindo seu IP, seguimos para a conexão na maquina local, no caso do Windows, usaremos um software chamado [**Putty**](https://www.putty.org) que funciona como como cliente SSH para a conexão, e possui uma instalação bem simples, na qual basta apenas este [site](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) e baixar o instalador para o windows.

Para utiliza-lo basta colocar o IP do servidor, e a porta previamente configurada.

![Captura de tela 2023-04-18 171516](https://user-images.githubusercontent.com/90939515/236107447-c2a6b930-ac30-4cc2-9f1e-daec905d897f.png)

![Captura de tela 2023-04-18 171535](https://user-images.githubusercontent.com/90939515/236107465-10618bd2-82e2-49be-8367-85f1762c984c.png)

Faça seu login e terá acesso assim como na maquina virtual.

![Captura de tela 2023-04-18 171550](https://user-images.githubusercontent.com/90939515/236107544-f94b91b9-adb5-451e-971b-d7e515f6af53.png)

# Servidor Web (Apache)

O Apache é um dos servidores web de software livre mais famoso do mundo, usado para hospedar e entregar conteúdo web, como páginas da web, imagens, vídeos, arquivos para download e aplicativos web. Ele funciona como um intermediário entre o navegador do usuário e o servidor de origem que fornece o conteúdo solicitado. Quando um usuário solicita uma página web, o Apache processa a solicitação e envia a resposta de volta para o navegador do usuário.

Para instalar o apache, utilizamos o comando `apt install apache2 apache2-doc`, que instala o apache e a sua documentação

![Captura de tela 2023-04-18 172854](https://user-images.githubusercontent.com/90939515/236109429-9979f39d-7ad5-42e4-a71f-52800210439c.png)

Com a instalação feita, podemos testar pesquisando o IP do servidor dentro de um navegador de internet.

![Captura de tela 2023-04-18 173546](https://user-images.githubusercontent.com/90939515/236109564-ce9993d6-ca95-437b-bad5-0d68b6ae21b1.png)

Antes de configurar, vamos verificar a pasta das configurações do apache usando o comando `cd /etc/apache2` e depois `ls` para listar.

![2023-04-18](https://user-images.githubusercontent.com/90939515/236230701-75c0afe8-9d20-47ec-9439-3ff050868bf3.png)

Para os curiosos em saber o que cada um desses arquivos fazem, estou recomendando o seguinte [GUIA](https://www.digitalocean.com/community/tutorials/how-to-configure-the-apache-web-server-on-an-ubuntu-or-debian-vps) para um estudo mais profundo sobre o uso do apache.

## Configurando o Apache

> Sempre que realizar alguma mudança na configuração de qualquer um dos serviços, recomendo que verifique o `status` do seu serviço, para verificar alguma coisa está errada, seja um comando errado ou um ponto e virgula faltando :+1:

Vamos entrar na pasta **conf-enabled** e abrir o arquivo **charset.conf** para iniciar a configuração

![Captura de tela 2023-04-19 173823](https://user-images.githubusercontent.com/90939515/236232553-cbad9261-a703-468c-b2cd-08aabac1fbcd.png)

Dentro do arquivo, vamos apenas apagar o # do AddDefultCharset UTF-8 para descomenta-lo. Retirar ele permite que nosso servidor aceite caracteres especiais sendo digitados.

![Captura de tela 2023-04-19 173959](https://user-images.githubusercontent.com/90939515/236233092-9ee3e2ed-2e85-423a-9282-4067310fd7de.png)

Agora vamos modificar o arquivo **apache2.conf**, para mudar os valores de Time out, em que diminuiremos o tempo de conexão do cliente para não consumir tantos recursos do servidor. Tambem mudaremos o **MaxKeepAliveRequests** que é a quantidade de solicitações que podem ser enviadas por uma unica conexão mantida ativa.

![Captura de tela 2023-04-19 174141](https://user-images.githubusercontent.com/90939515/236235219-3fe29f77-ff96-48eb-a100-7c1894ea3760.png)

![image](https://user-images.githubusercontent.com/90939515/236235541-9e103db9-4cb5-4f92-b7fd-c411c638548f.png)

Agora iremos modificar o **<Directory /usr/share>** onde nele tem o **Require all granted** iremos modificar para **Require all denied** ele define as configurações para o diretório */usr/share* no sistema de arquivos e nesse caso estamos negando o acesso de todas as requisições.

![Captura de tela 2023-04-19 174450](https://user-images.githubusercontent.com/90939515/236238742-a29879ab-c568-425f-a43c-44f300a42600.png)

Agora fechamos, e salvamos o arquivo.

---

Para fazer a checagem das nossas mudanças, utilizamos o comando `/etc/init.d/apache2 restart` para reiniciar e o comando `/etc/init.d/apache2 status` para conferir como está o serviço.

![Captura de tela 2023-04-19 174543](https://user-images.githubusercontent.com/90939515/236239492-879ded04-082d-4cf6-868b-9ab8625120a2.png)

O **Active (running)** indica que está rodando sem problemas.

---

Agora vamos para a pasta sites-available, dentro dessa pasta tem dois arquivos, o 000-default.conf e o default-ssl.conf, faremos uma cópia do 000-defult.conf com o nome de 010-pedro.conf com o código `cp 000-defult.conf 010-pedro.conf`.

![Captura de tela 2023-04-25 170029](https://user-images.githubusercontent.com/90939515/236242798-88c3ec22-8b88-4c5a-9b93-39cc13769521.png)

Vamos desabilitar a configuração **000-default.conf** com o comando `a2dissite 000-defult.conf`, e já reiniciamos e conferimos se está tudo ok. 

![Captura de tela 2023-04-25 170603](https://user-images.githubusercontent.com/90939515/236243378-5141e9aa-4d29-4b5b-b18f-39b904dd16c1.png)

Agora temos somente a nossa configuração ativa vamos modifica-la entrando no arquivo **010-pedro.conf**. A primeira coisa que vamos fazer é apagar a # de **ServerName www.exemple.com** e modificar a url para a ulr que iremos usar futuramente com DNS que será **www.pedrobahury.com** e em **ServerAdmin webmaster@localhost** vou trocar pelo meu email acadêmico **pedro.f@acad.ifma.edu.br**

![Captura de tela 2023-04-25 170818](https://user-images.githubusercontent.com/90939515/236244741-bb900ab4-6fcc-4165-b07f-0126de5e01fc.png)

Após isso, reinicie o servidor, e veja se está tudo certo.

### Adicionando um Site

Para deixar o site mais customizado, vamos acessar o site https://html5up.net/ onde teremos modelos de sites html para baixar.

Antes de baixarmos vamos deletar o site padrão, indo na pasta onde o site está o arquivo do site que é em `/var/www/html` e digitar o comando `rm index.html` que deletará o arquivo **index.html**

Para baixarmos basta copiar o link de download do site e colar ele nesse comando (dentro da pasta /var/www/html) ``wget --no-check-certificate link_de_download``, e utilizaremos o comando ``zip``, para descompactar com `unzip download`

![Captura de tela 2023-04-25 171610](https://user-images.githubusercontent.com/90939515/236251894-d5577bd8-950b-4561-bfdd-980c048b99d4.png)

![Captura de tela 2023-04-25 171814](https://user-images.githubusercontent.com/90939515/236251978-3d39de19-cc64-4194-9438-6e6a6611308e.png)

Agora podemos ver os arquivos descompactados, e remover o arquivo de download usando `rm nome_do_arquivo`

![Captura de tela 2023-04-25 171921](https://user-images.githubusercontent.com/90939515/236250540-1c39f1ac-25dc-485d-b64d-a6ef7d50a014.png)

Agora acessando nosso site, poderemos ver o novo visual do nosso site.

![Captura de tela 2023-04-25 172030](https://user-images.githubusercontent.com/90939515/236258782-e17c2346-3133-4b55-8751-59f0ac7324e7.png)

# Serviço DNS

DNS (Domain Name System) é um sistema que converte nomes de domínio em endereços IP. Para que uma pessoa acesse um site na internet, ela precisa digitar o endereço do site no navegador. No entanto, os computadores na Internet usam endereços IP para se comunicar entre si. 

O DNS funciona como um diretório de nomes de domínio e seus respectivos endereços IP. Quando um usuário digita um nome de domínio no navegador, o computador faz uma consulta ao servidor DNS para encontrar o endereço IP correspondente.

O processo funciona assim: quando o navegador envia a solicitação de acesso a um determinado site, ele envia essa solicitação para um servidor DNS. O servidor DNS verifica em sua base de dados qual é o endereço IP associado a esse nome de domínio e envia essa informação de volta para o navegador. O navegador, então, usa esse endereço IP para estabelecer uma conexão com o site.

Para baixar utilizaremos esse comando`apt install bind9 bind9-utils bind9-doc`. Para os mais curiosos que procuram uma explicação mais profunda de cada etapa, e o que cada pacote significa, recomendo checar este [guia](https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-a-private-network-dns-server-on-ubuntu-18-04)

![Captura de tela 2023-04-25 210042](https://user-images.githubusercontent.com/90939515/236264956-bf7847ef-a668-43ac-ba25-2d3f5d490765.png)

## Arquivos

Os arquivos de configuração ficam na pasta **/etc/bind**

![Captura de tela 2023-04-25 210105](https://user-images.githubusercontent.com/90939515/236265041-a2c5b470-ceb3-4e11-9d67-0aa4c71fbddc.png)

---
## Configurando

Começamos copiando o arquivo **db.local** com o nome de **db.pedro.com** com o comando `cp nome_do_arquivo nome_do_novo_arquivo`

![Captura de tela 2023-04-25 210403](https://user-images.githubusercontent.com/90939515/236269791-05f6e8a3-5202-4cff-b763-b4b5c7a04809.png)

Agora entramos no arquivo db.pedro.com para configura-lo

![Captura de tela 2023-04-25 210501](https://user-images.githubusercontent.com/90939515/236270610-91baa844-eb00-4234-8b12-7f21738fd01f.png)

O TTL (Time-to-Live) define o tempo que o servidor leva para atualizar a cache dos dominios evitando que fiquem desatualizado

Na linha abaixo do TTL vamos modificar o registro SOA (Start of Authority) para a zona. Ele contém informações de autoridade para a zona, incluindo o nome do servidor primário que é o localhost. e o endereço de e-mail do administrador que é root.localhost que vamos modificar respectivamente para servidor.pedro.com e root.pedro.com as configurações de serial refresh retry expire e negative cache TTL não iremos modificar

Nas linhas abaixo faremos as seguintes modificações

```
@               IN      NS      pedro.com.
                IN      A       192.168.43.61
servidor        IN      A       192.168.43.61
www             IN      A       192.168.43.61
```

No caso o endereço de IP escrito no documento deve ser o do seu servidor, neste exemplo estou utilizando o meu.

