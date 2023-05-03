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
**su** 
