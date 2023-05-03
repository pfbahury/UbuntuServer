# Introdução

Essa wiki serve como guia e documenta todas a implementação de um servidor utilizando o sistema operacional [Ubuntu Server](https://ubuntu.com/server) proposto pelo professor [Paulo Henrique Sousa Barbosa](https://github.com/agenteph) na disciplina de Redes de Computadores II do curso de Ciência da Computação do Instituto Federal de Educação, Ciência e Tecnologia do Maranhão - Campus Imperatriz (IFMA). Projeto por [Pedro Fernandes Bahury](https://github.com/pfbahury)

Para o prosseguimento dentro deste "guia", serão necessário algumas preparações antes de qualquer coisa, no caso, alguns softwares serão necessarios para realizar as ações descritas, detalhe, este projeto está sendo realizado no sistema operacional Windows 11, então no caso de usuario com um sistema linux, alguns dos passos descritos neste guia, e alguns programas externos podem não ser necessários.

Para realizar as atividades será necessario a utilização de uma maquina virtual, ou [VirtualBox](https://www.virtualbox.org), e uma imagem `.iso` do sistema operacional que iremos utilizar, neste caso, o [Ubuntu Server](https://ubuntu.com/download/server)

#Configurando a Maquina virtual

Logo de começo, recomendo um guia de criação de uma maquina virtual, para os que não possuem o costume de utilizar uma, recomendo o seguinte [guia](https://tecnoblog.net/responde/como-criar-uma-maquina-virtual-virtualbox/)

AVISO!!
Antes de prosseguir com o uso do arquivo `.iso` será necessario fazer as configurações de rede.

##Configurando a rede

Após criar a sua maquina virtual, clique na engrenagem amarela para acessar as configuração gerais da maquina.

![image](https://user-images.githubusercontent.com/90939515/236021214-17c1840b-55c4-43d9-a5e3-ef9f807140ee.png)

Dentro das configurações, clique nas aba de **rede**

![image](https://user-images.githubusercontent.com/90939515/236023130-c4ee3bc4-07a0-4503-bed7-b54f32ca43b3.png)

As configurações estarão como padrão em modo NAT, mude para modo bridge, e dentro das configurações avançadas, mude o **Modo Promícuo**, para **Tudo Permitido**, deixando do mesmo jeito que a imagem.
