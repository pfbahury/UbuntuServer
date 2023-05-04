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
