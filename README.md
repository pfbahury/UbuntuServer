# Introdução

Essa wiki serve como guia e documenta todas a implementação de um servidor utilizando o sistema operacional [Ubuntu Server](https://ubuntu.com/server) proposto pelo professor [Paulo Henrique Sousa Barbosa](https://github.com/agenteph) na disciplina de Redes de Computadores II do curso de Ciência da Computação do Instituto Federal de Educação, Ciência e Tecnologia do Maranhão - Campus Imperatriz (IFMA). Projeto por [Pedro Fernandes Bahury](https://github.com/pfbahury)

Para o prosseguimento dentro deste "guia", serão necessário algumas preparações antes de qualquer coisa, no caso, alguns softwares serão necessarios para realizar as ações descritas, detalhe, este projeto está sendo realizado no sistema operacional Windows 11, então no caso de usuario com um sistema linux, alguns dos passos descritos neste guia, e alguns programas externos podem não ser necessários.

Para realizar as atividades será necessario a utilização de uma maquina virtual, ou [VirtualBox](https://www.virtualbox.org), e uma imagem `.iso` do sistema operacional que iremos utilizar, neste caso, o [Ubuntu Server](https://ubuntu.com/download/server)

# Configurando a Maquina virtual

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
                IN      A       192.168.2.61
servidor        IN      A       192.168.2.61
www             IN      A       192.168.2.61
```

No caso o endereço de IP escrito no documento deve ser o do seu servidor, neste exemplo estou utilizando o meu.

Agora vamos criar a zona reversa do nosso dominio, para isso vamos copiar o arquivo **db.127** com o nome **db.seu_ip_ao_contrario.in-addr.arpa** que no meu caso ficou **db.2.168.192.in-addr.arpa**

Dentro do arquivo faremos mudanças parecidas com a anterior, mas com algumas mudanças

```
@       IN      NS      servidor.pedro.com.
        IN      PTR     servidor.pedro.com.
61      IN      PTR     servidor.pedro.com.
61      IN      PTR     www.
```

> Os valores marcados como 61, devem são os ultimos digitos do seu IP

> Ao terminar, não esqueça de salvar, reiniciar o serviço e verificar o status.

Agora vamos criar as zonas DNS, basta irmos no arquivo `named.conf.local` e iremos adicionar as seguintes configurações.

```
zone "pedro.com" {
        type master;
        file "/etc/bind/db.pedro.com";
};

zone "2.168.192.in-addr.arpa"{
        type master;
        file "/etc/bind/db.2.168.192.in-addr.arpa";
};
```

![Captura de tela 2023-04-26 172158](https://user-images.githubusercontent.com/90939515/236294501-7dbc0fd7-a1de-49f2-86df-c335cf5eb0d8.png)

Agora vamos mudar o dns do nosso servidor indo em `/etc/resolv.conf` e colocando o ip do nosso servidor.

![Captura de tela 2023-04-26 172447](https://user-images.githubusercontent.com/90939515/236295705-a9bfef03-a9f1-49aa-9f52-be91975cbde6.png)

Podemos testar usando o `nslookup ip_ou_dominio` e abrindo pelo navegador.

![image](https://user-images.githubusercontent.com/90939515/236469654-88c9e515-55ad-4e4f-9a03-6835931aa9d2.png)

Nesta print estou utilizando outra maquina virtual, com o sistema [Pop!_OS](https://pop.system76.com) para realizar testes.

# Serviço FTP

FTP (File Transfer Protocol) é um protocolo usado para transferir arquivos pela Internet entre computadores. Ele foi criado para permitir que usuários de diferentes sistemas operacionais possam compartilhar arquivos de forma simples e eficiente.

## Instalação

Para instalar vamos utilizar o comando `apt install proftpd` que baixará o pacote proftpd

## Configurando o Serviço

Para iniciarmos a configuração, iremos ao arquivo `/etc/proftpd/proftpd.conf` nele o nome do servidor "Debian "deve ser trocado pelo seu nome.

![Captura de tela 2023-04-27 173253](https://user-images.githubusercontent.com/90939515/236477395-edbd8e6d-303c-48ad-8428-5c5cba5e493b.png)

![image](https://user-images.githubusercontent.com/90939515/236477456-878dd3a1-4360-44a9-8b19-d1454ab86390.png)

Em **ShowSymLinks** deixaremos em off, essa configuração determina se os links do sistema aparecem ou não para o usuário, como não queremos isso, deixaremos desligado.

![image](https://user-images.githubusercontent.com/90939515/236478306-01caa5f3-2412-4f0d-8fe5-64b1fdc35672.png)

Vamos descomentar as configurações **DefaultRoot~** e **RequireValidShell off**, e colocar um espaço na primeira configuração deixando `DefaultRoot ~` para evitar qualquer erro. O **DefaultRoot** define o diretório inicial padrão para o usuário FTP ao fazer login, no nosso caso o **~** representa o diretório home do usuário,após o login, o usuário será direcionado para o seu diretório home no servidor que no nosso caso será o **/home/usuarioftp1** já o **RequireValidShell off** define se permite que usuários com um shell inválido façam login no servidor FTP, como o valor que colocamos é off isso permite que qualquer usuário faça login, mesmo que o shell listado em /etc/passwd seja inválido.

![Captura de tela 2023-04-27 173532](https://user-images.githubusercontent.com/90939515/236479889-77759a0a-2040-4554-93c4-faddafb470c1.png)

No final do arquivo adicionamos as configurações TransferRate RETR 30:100 e RootLogin Off onde o TransferRate RETR 30:100 define o limite de taxa de transferência de download para os arquivos, o limite é definido como sendo de 30 a 100 KB/s. Já o RootLogin Off desabilita o login de usuários com o usuário root.

![Captura de tela 2023-04-27 173657](https://user-images.githubusercontent.com/90939515/236481195-507970c2-d598-4270-9ed8-1d92424532d0.png)

Após isso, salve o serviço utilizando o comando `/etc/init.d/proftpd restart` e `/etc/init.d/proftpd status` para ver o status do serviço.

![Captura de tela 2023-04-27 174335](https://user-images.githubusercontent.com/90939515/236482197-23bc26e5-027f-4b42-82d8-593078f8f520.png)

Após Configurarmos, será criado um usuario para ter acesso ao ftp, para fazer isso, só é preciso utilizar o comando `adduser nome_do_usuário` e emm seguida criar uma **senha**.

![Captura de tela 2023-04-27 174533](https://user-images.githubusercontent.com/90939515/236490778-678e2044-2d0a-4222-bf2d-70453c54e8b0.png)

O formulário não é obrigatório de se preencher, pode pular as opções apertando "Enter"

---

Agora vamos modificar o arquivo `/etc/passwd` (*:warning: Cuidado ao mexer nessa configuração pois você pode perder o acesso ao servidor permanentemente, modifique somente o usuário que criou*) vamos modificar a pasta do usuário que acabamos de criar para ficar assim `/home/usuarioftp1/ftp` e a pasta do shell para `/usr/sbin/nologin` por esse motivo, configuramos o **RequireValidShell** em **off**

![Captura de tela 2023-04-27 174645](https://user-images.githubusercontent.com/90939515/236492185-aad3f8e7-2381-4f82-98c3-08db58ff225b.png)

![Captura de tela 2023-04-27 174814](https://user-images.githubusercontent.com/90939515/236492228-a183efe6-e5fc-4078-a136-e91cfa5a5d79.png)

Agora criaremos a pasta ftp já que definimos anteriormente a pasta do usuário como `/home/usuarioftp1/ftp`, para isso vamos na pasta `/home` e no usuário que no caso é **usuarioftp1** em `/home/usuarioftp1` e estando nela digitamos `mkdir ftp` para criar a pasta **ftp**

![Captura de tela 2023-04-27 174859](https://user-images.githubusercontent.com/90939515/236495097-6891cbd8-29ce-4fa8-a140-5c9ce2f72afe.png)

Em seguinte salve, reinicie , e verifique o status do serviço.

Vamos mudar as permissões da pasta com o comando `chown usuarioftp1:usuarioftp1 ftp` que troca o usuário da pasta **ftp** para o **usuarioftp1** pois como criamos ela com usuário root as suas permissões estavam somente para o usuário padrão, e o `chmod 770 ftp` esse comando  modifica permissões de acesso a um arquivo ou diretório.

![Captura de tela 2023-04-27 175000](https://user-images.githubusercontent.com/90939515/236495747-139b19f9-38ce-4207-91e0-f6524e82cae0.png)

Salve, reinicie e verifique novamente o status.

Para conectar com o seu ftp, temos duas opções, dentro do windows, será necessario usar um programa externo, tal como o [FilleZilla](https://filezilla-project.org). Já no caso do linux, utilize o gerenciador de arquivos, apenas digite a url ftp//ip_do_servidor.

![image](https://user-images.githubusercontent.com/90939515/236496671-e41209d9-1f40-49a4-864b-28aeffbec51d.png)

Faça o login, com seu usuário criado, e terá acesso a sua pasta.

![image](https://user-images.githubusercontent.com/90939515/236496929-45c6ddfb-6a05-4789-b1fa-06bbf9d990f8.png)

# Samba

O samba é uma outra solução de compartilhamento de arquivos, também de impressoras. Com o Samba, é possível criar um servidor de arquivos e impressoras em um sistema operacional Linux ou Unix, por exemplo, que pode ser acessado por computadores Windows. 

## Instalação

Para instalar o Samba basta usar o comando `apt install samba samba-common`

![Captura de tela 2023-05-01 210346](https://user-images.githubusercontent.com/90939515/236500049-08b54b27-82de-4c7d-b659-5d22a7c2629d.png)

## Configuração do Serviço

Antes de iniciar a configuração, vamos realizar o backup do arquivo de configuração **smb.conf** que está na pasta `/etc/samba`

![Captura de tela 2023-05-01 210752](https://user-images.githubusercontent.com/90939515/236500558-8efb7c7f-2a5b-4388-ae83-33fa942e807f.png)

Vamos abrir o arquivo **smb.conf** para configura-lo, selecione e apague toda essa configuração que veio nele.

No linux, pode selecionar e segurar `Shift + seta pra baixo`, e para apagar, pressione `ctrl + k`. No caso do Windows, se estiver utilizando o Putty, pressione `alt + shift + t`, para recortar todo o arquivo.

![Captura de tela 2023-05-01 211640](https://user-images.githubusercontent.com/90939515/236503326-bd443fa0-a563-498a-89c3-5f3a6edf9b53.png)

Vamos adicionar um exemplo de configuração.

```
[global]
netbios name = usuarioftp1
workgroup = usuarioftp1
server string = Servidor dos Alunos
security = user

[public]
path = /home/samba
guest ok = yes
browseable = yes
writeable = yes
printable = no
create mask = 0777
force create mode = 0777

[printers]
comment = Compartilhando as Impressoras
print ok = yes
guest ok = yes
path = /var/spool/samba

[pedrobahury]
Comment = Diretórios dos alunos
path = /etc/samba/pedrobahury
valid users = usuarioftp1
create mask = 0777
force create mode = 0777
read only = no
```

![Captura de tela 2023-05-01 211640](https://user-images.githubusercontent.com/90939515/236505771-19bbaca0-2dcf-43f4-ae10-f1d251503919.png)

Agora iremos reiniciar o serviço digitando o comando `/etc/init.d/smbd restart` e para verificar o status do serviço `/etc/init.d/smbd status`

![Captura de tela 2023-05-01 211759](https://user-images.githubusercontent.com/90939515/236506061-1ae818d8-9888-463b-9c0b-2f04ebad365c.png)

Vamos criar as pastas agora para poder acessá-las. Primeiro criei a pasta **pedrobahury** usando o comando `mkdir nome_da_pasta`, listei as permissões da pasta com `ls -l` alterei o usuario da pasta para o usuarioftp1 com o comando `chown usuário:grupo_do_usuário pasta` e em seguida alterei as permissões com o comando `chmod 777 pasta` permitindo que a pasta seja totalmente aberta.

![Captura de tela 2023-05-01 212443](https://user-images.githubusercontent.com/90939515/236507003-344a51c8-56cb-4cd4-9880-3cdd00352441.png)

Agora vamos adicionar o usuarioftp1 aos usuarios samba com o comando `smbpasswd -a nome_de_usuario`

![Captura de tela 2023-05-01 212509](https://user-images.githubusercontent.com/90939515/236507812-07a60f1d-0e4e-42e0-98f3-6b2cb5aa553b.png)

Assim como o podemos fazer login utilizando um endereço smb e utilizar a pasta.

![image](https://user-images.githubusercontent.com/90939515/236508161-f1d4f80f-a3d4-4a9a-8d93-dac3965e3993.png)

![image](https://user-images.githubusercontent.com/90939515/236508198-9d483705-37ef-4e9a-a955-f5c8e32ea79c.png)

# Serviço DHCP

DHCP (Dynamic Host Configuration Protocol) é um protocolo de rede que permite a atribuição dinâmica de endereços IP e outras configurações de rede para dispositivos conectados em uma rede.

## Instalação

Para instalar o serviço basta utilizar o comando `apt install isc-dhcp-server` que baixará os pacotes

## Configurando o Serviço

Primeiramente configuramos o arquivo `nano /etc/default/isc-dhcp-server`, nesse arquivo apontamos qual será o nosso arquivo de configuração e também qual interface de rede nosso serviço atuará. Para isso vamos descomentar o `DHCPDv4_CONF=/etc/dhcp/dhcpd.conf` que é onde nosso arquivo de configuração está, agora vamos definir nossa interface de rede a qual nosso serviço atuará, para saber qual interface de rede vamos utilizar comando `ifconfig` e escolher a interface que está utilizando, para saber qual interface está utilizando basta ver qual delas tem o **IP** da rede em que você está conectado. no meu caso é a **enp0s3**

![image](https://user-images.githubusercontent.com/90939515/236511379-7502c01f-a2b4-4b56-936b-565fe8e9b48a.png)

![Captura de tela 2023-05-01 213955](https://user-images.githubusercontent.com/90939515/236511479-eb17b9a0-52bb-4a56-95a4-a7b62ff01220.png)

Agora podemos ir para a pasta de serviço `cd /etc/dhcp/` e abrir o arquivo **dhcpd.conf** que faz a configuração do serviço DHCP e define as configurações de rede.

Abrindo ele faremos as seguintes configurações, em domain-name irei colocar o meu pedrobahury.com que foi criado anteriormente e em domain-name-servers colocarei o ip do meu servidor de DNS que é o 192.168.2.102 e mais outro sendo ele o servidor de DNS da CloudFlare 1.1.1.1. O default lease time que é o tempo em que o cliente pode usar o endereço IP atribuído antes que ele precise renovar com o servidor DHCP vamos deixar em 86400 que é 24 horas em segundos, já o max lease time é o tempo máximo que o endereço IP pode ser mantido pelo cliente antes de precisar ser renovado colocaremos em 172800 que dá 48 horas em segundos. Vamos adicionar o ignore client-updates que faz que o servidor não atualize o DNS do cliente automáticamente quando ele solicita um novo ip.

![image](https://user-images.githubusercontent.com/90939515/236514437-3d3ddf8f-b48b-4423-87ee-38324c2e9da9.png)

Agora adicionaremos uma maquina para ter um ip fixo, e um exemplo de configuração de subnet.

```
host meupcfixo {
  hardware ethernet 08:00:07:26:c0:a5;
  fixed-address 192.168.2.12;
}

subnet 192.168.2.0 netmask 255.255.255.0 {
  range 192.168.2.2 192.168.43.254;
  option domain-name-servers 192.168.43.227;
  option domain-name "pedrobahury.com";
  option routers 192.168.2.1;
  option broadcast-address 192.168.43.255;
  default-lease-time 600;
  max-lease-time 7200;
}
```
Adapte código para a sua rede.

![Captura de tela 2023-05-01 215038](https://user-images.githubusercontent.com/90939515/236514620-b828c525-cc3d-4ab3-8df4-049209d80ebf.png)

Vamos reiniciar e conferir o status com `/etc/init.d/isc-dhcp-server restart` e `/etc/init.d/isc-dhcp-server status`

![Captura de tela 2023-05-01 215143](https://user-images.githubusercontent.com/90939515/236525937-56155a1c-e2e0-459c-ae71-f0051548013a.png)

Em caso de erro você pode verificar o log do serviço com esse comando `journalctl -u isc-dhcp-server`.

# Servido Proxy (Squid)

O Squid funciona como um intermediário entre os clientes da rede e os servidores da Internet, armazenando em cache as páginas da Web que são frequentemente acessadas pelos usuários. Quando um cliente solicita uma página, o Squid verifica se ela já está armazenada em seu cache. Se sim, o Squid retorna a página armazenada em cache, em vez de recuperá-la novamente da Internet. Isso reduz o tempo de espera e a largura de banda necessária para acessar o conteúdo da Web.

## Instalação 

Para instalar o serviço utilizamos o comando `apt install squid` e a instalação se iniciará

## Configuração do Serviço

Vamos entrar na pasta de serviço `cd /etc/squid` e abrir o arquivo com `nano squid.conf`

![Captura de tela 2023-05-01 215758](https://user-images.githubusercontent.com/90939515/236536109-20bb9528-32b2-4792-9627-b24cedd1cbf1.png)

Vamos limpar todo o arquivo com o comando `echo "0" > squid.conf` que substituirá todos os dados do arquivo po "0", assim o arquivo terá apenas um 0 dentro dele.

![Captura de tela 2023-05-01 215903](https://user-images.githubusercontent.com/90939515/236536342-e2be429a-02fa-414a-bd4c-c8381afa03a3.png)

Colocaremos essa configuração de exemplo no Squid.

```
http_port 3128 
error_directory /usr/share/squid/errors/Portuguese
cache_mem 1024 MB
cache_dir ufs /var/spool/squid 1000 16 256
maximum_object_size_in_memory 64 KB
maximum_object_size 50 MB
cache_swap_low 70
cache_swap_high 95

access_log daemon:/var/log/squid/access.log squid
cache_log /var/log/squid/cache.log

acl localnet src 192.168.2.0/24
acl Safe_ports port 80 # http
acl Safe_ports port 21 # ftp
acl Safe_ports port 443 # https
http_access deny !Safe_ports

acl sitesproibidos url_regex -i "/etc/squid/sitesproibidos"
http_access deny localnet sitesproibidos
http_access allow localnet
http_access allow all
```

Para um guia de explicação recomendo seguir este [Tutorial](https://www.digitalocean.com/community/tutorials/how-to-set-up-squid-proxy-on-ubuntu-20-04)

As pastas ficam deste jeito com essa configuração

![image](https://user-images.githubusercontent.com/90939515/236538187-1a84a221-a49c-413d-ab3c-d85410c90b71.png)

Dentro da pasta **sitesprobidos**, podemos ver os endereços que queremos bloquear.

![image](https://user-images.githubusercontent.com/90939515/236538708-a9fa4cc2-e0c1-4682-99c1-58a84ff29d85.png)

Agora iremos configurar o arquivo `/proc/sys/net/ipv4/ip_forward` que é usado para ativar ou desativar o encaminhamento de pacotes IP entre as interfaces de rede de um sistema Linux, fazemos isso apenas mudando seu valor de 0 para 1 com o comando `echo 1 > /proc/sys/net/ipv4/ip_forward`

![Captura de tela 2023-05-01 220945](https://user-images.githubusercontent.com/90939515/236539262-9131acc6-95d6-421b-b9cc-7f743535ca73.png)

Agora utilizamos o comando `iptables -t nat -A POSTROUTING -o enp0s3 -s 192.168.43.0/24 -j MASQUERADE` para adicionar uma regra a tabala **nat** do iptables, permitindo o mascaramento de endereço de rede.

Reinicie seu serviço logo em seguida e verifique o status, se estiver tudo bem, nosso proxy ja está funcionando.

![Captura de tela 2023-05-01 221448](https://user-images.githubusercontent.com/90939515/236539786-b8260c6d-1e75-43c4-a542-27fd958244d5.png)

Vamos configurar o nosso sistema operacional, irei usar o Pop_OS, mas este [guia](https://canaltech.com.br/windows/windows-10-como-configurar-um-proxy/) demonstra como usar no Windows 10 para adicionar nosso proxy. Para isso vamos nas configurações e depois em rede, para configurarmos manualmente, e estaremos pronto.

![image](https://user-images.githubusercontent.com/90939515/236540584-58ae9571-2d1a-45dd-8a4f-6512e6dc2ae2.png)

# Serviço IPSec

IPsec (Internet Protocol Security) é um protocolo de segurança para redes de computadores que visa garantir a confidencialidade, integridade e autenticidade dos dados transmitidos na rede. Ele é usado para proteger a comunicação entre dispositivos em uma rede, fornecendo um conjunto de mecanismos de segurança para proteger os dados.

## Instalação 

Para realizar a instalação o IPSec, pdemos utilizar o comando  `apt-get install strongswan`.

## Configurando o Serviço

Para configurar o serviço, começaremos editando o arquivo `/etc/ipsec.conf`

![Captura de tela 2023-05-01 221602](https://user-images.githubusercontent.com/90939515/236542050-169beb2c-9ecd-4baf-9c17-d5865d111c19.png)

Existem diversas configurações diferentes neste serviço, porém faremos apenas uma para funcnionar entre 2 computadores.
Adicionaremos as seguintes configurações.

![Captura de tela 2023-05-01 221705](https://user-images.githubusercontent.com/90939515/236542958-5fdebb50-9fda-46b5-be73-c4175ad5565a.png)

Adicionaremos o seguinte comando no arquivo `/etc/ipsec.secrets`

![image](https://user-images.githubusercontent.com/90939515/236543569-04c66ed0-3e61-4976-b786-621dd5c3d8c4.png)


Aqui colocamos o IP de origem e em seguida o de destino, logo após PSK (Pre-Shared Key) que significa Chave Pré Compartilhada que é um método de autenticação, logo em seguida, digaremos a senha, na qual voce deve colocar uma senha forte.

Agora podemos reiniciar e ver o status do serviço com `/etc/init.d/ipsec restart` e `/etc/init.d/ipsec status`

![Captura de tela 2023-05-01 221850](https://user-images.githubusercontent.com/90939515/236543742-a5febbb2-79cc-483e-bf2c-82709518d74b.png)

Sua maquina agora está configurada, caso queira fazer a comunicação entre duas maquinas, apenas configure da mesma maneira que configurou esta, e logo em seguida, você pode realizar testes como o de Ping, ou utilizando o programa [Wireshark](https://www.wireshark.org)

# Webmin

O Webmin é uma interface de usuário baseada na web para gerenciamento de sistemas Unix-like. Ele permite que os administradores de sistema gerenciem facilmente as configurações do sistema, serviços, contas de usuário, arquivos e muito mais, tudo através de uma interface amigável baseada em navegador.

## Instalação

Para instalar o webmin basta usar o comando `curl -o setup-repos.sh https://raw.githubusercontent.com/webmin/webmin/master/setup-repos.sh` e depois disso executar o comando `sh setup-repos.sh` após finalizar o processo, é digitamos `apt-get install webmin` após executar esse comando seu Webmin estará funcionando.

## Acesso

Para acessar o Webmin, só é preciso digitar o seu ip na porta 10000, no meu caso `192.168.2.100:10000`, ira aparecer uma mensagem de aviso, mas não se preocupe, isso ocorre devido a comunicação criptografada do HTTPS, mas é totalmente seguro de se acessar.

![image](https://user-images.githubusercontent.com/90939515/236548202-c3c1d18b-622a-4bc0-b9aa-da5c91dca96d.png)

Com ele podemos ver diversas informações sobre o nosso sistema 

![image](https://user-images.githubusercontent.com/90939515/236548352-943652f0-4d9c-49c6-811e-59398bde5947.png)

Indo na aba de servidores podemos checar todos os nossos serviços.

![image](https://user-images.githubusercontent.com/90939515/236548592-7bcd3f1f-78da-42c4-970d-55f505f9a2c8.png)

Incluindo configurações feitas como no DHCP

![image](https://user-images.githubusercontent.com/90939515/236548740-f1ecf3a2-9106-4a8f-a339-0659671d575a.png)

Até mesmo os arquivos de configuração podem ser modificados por aqui

![image](https://user-images.githubusercontent.com/90939515/236552406-746e15dd-3be8-4b04-87a7-4f1f2addd184.png)

Existem diversas outras funções dentro do Webmin, esses são apenas alguns exemplos.

# Monitorix

O Monitorix é um software livre e de código aberto que funciona como uma ferramenta de monitoramento de sistema e rede em tempo real. Ele é projetado para monitorar o desempenho do sistema e dos serviços em tempo real e exibir as informações em gráficos fáceis de entender e personalizáveis.

## Instalação

Para instalar o monitorix basta digitar o comando `sudo apt install monitorix` 

## Detalhes

Após instalar, para acessar o serviço, basta ir no ip do seu site na porta 8080/monitorix no meu caso ficou assim 192.168.2.100:8080/monitorix. 

![image](https://user-images.githubusercontent.com/90939515/236554212-4b7aaaab-0dbf-462f-8cda-429796d830ef.png)

Ele exibe diversos gráficos como:

> Uso da CPU

> Uso da memória

> Uso de disco

> Uso de rede (taxa de transferência, pacotes, erros)

> Atividade do sistema (processos em execução, interrupções)

Entre outros.

# Serviço SSL

SSL (Secure Sockets Layer) é um protocolo de segurança que fornece comunicação criptografada e autenticada entre clientes e servidores na internet. Ele é usado para proteger a integridade e a privacidade dos dados transmitidos entre um navegador da web e um servidor web.

## Instalação

Para instalar o SSL basta digitar esse comando `apt-get install openssl` que o SSL será instalado em seu servidor.

## Configurando

Para iniciar nossas configurações vamos habilitar nosso SSL no apache com o comando `a2enmod ssl` e agora reiniciar o serviço, dessa vez iremos utilizar o comando `systemctl restart apache2` para reiniciar o apache.

Vamos criar a pasta **ssl** dentro da pasta do apache com o comando `mkdir /etc/apache2/ssl`, será nessa pasta onde vamos colocar nossos certificados.

![image](https://user-images.githubusercontent.com/90939515/236555314-e15c5e18-b347-4a55-85b5-cb5ecf9edcb0.png)

Agora vamos criaremos nossos certificados com o comando 
`openssl req -x509 -nodes -days 365 -newkey rsa:4096 -keyout /etc/apache2/ssl/elvis.key -out /etc/apache2/ssl/elvis.crt`

Quando o comando for executado, se iniciará um formulario, que deve ser preenchido para por as informações do certificado. No caso, n coloquei respostas muito precisas, mas em ambientes profissionais, é recomendado o uso de informações corretas.

![Captura de tela 2023-05-04 202940](https://user-images.githubusercontent.com/90939515/236555832-c957b0b9-2e25-4d33-b576-8bfbc906abe6.png)

Acessando a pasta, veremos nossos certificados
![Captura de tela 2023-05-04 203010](https://user-images.githubusercontent.com/90939515/236555923-12b9fe3b-e39a-4080-93bb-74cb195e3d2e.png)

Após criado, iremos acessar a configuração do ssl com o comando `nano /etc/apache2/sites-available/default-ssl.conf`, e em **SSLCertificateFile** passaremos o endereço do certificado em **SSLCertificateKeyFile**
 passaremos o endereço da nossa chave privada, no meu caso `/etc/apache2/ssl/pedro.crt` e `/etc/apache2/ssl/pedro.key` respectivamente. 
