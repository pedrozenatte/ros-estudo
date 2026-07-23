# Instalação do ROS Noetic - Ubuntu 20.04 LTS

## Documentação oficial

Para instalar o ROS Noetic, utilize a documentação oficial:

- Tutoriais: https://wiki.ros.org/ROS/Tutorials  
- Instalação geral: https://wiki.ros.org/ROS/Installation  
- Instalação específica (Noetic): https://wiki.ros.org/noetic/Installation/Ubuntu  

---

## Configuração do ambiente

Sempre que abrir um terminal para usar o ROS, é necessário carregar as variáveis de ambiente:

```bash
source /opt/ros/noetic/setup.bash
```

Esse comando configura o ambiente para que os comandos do ROS funcionem corretamente.

#### Automatizando (recomendado)

Para não precisar executar esse comando manualmente toda vez, você pode adicionar ao arquivo de configuração do shell.

##### bash:
```bash
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

Assim, o ambiente do ROS será carregado automaticamente ao abrir o terminal.

## Testando a instalação

Após a instalação e configuração, teste com:

```bash
roscore
```

Se tudo estiver correto, o ROS Master será iniciado e o sistema estará pronto para uso.

Verificar a versão instalada: 
```bash
rosversion -d
```

---

## Utilizando imagem do ROS com DOCKER
Acesse o Docker hub do ROS: https://hub.docker.com/_/ros

Nas Tags, pesquise pelo ROS a ser utilizado, mas nesse caso, vamos usar o **noetic**.
```bash
docker pull ros:noetic-ros-base-focal
```
OBS: Tem outras lá, mas vamos usar essa.

Agora, para utilizar o ROS em outra versão do ubuntu ou linux, podemos criar um container e deixar ele em background, após isso, ao abrir outro terminal, basta entrar no container que está em segundo plano e rodar o comando do ROS para carregar as variáveis de ambiente. 

```bash
docker run -dit --name ROS ros:noetic-ros-base-focal

pedro@pedro-Aspire-A515-54G:~$ docker ps
CONTAINER ID   IMAGE                       COMMAND                  CREATED         STATUS         PORTS     NAMES
e73104f1999d   ros:noetic-ros-base-focal   "/ros_entrypoint.sh …"   3 seconds ago   Up 2 seconds             ROS
```

Depois, em quantos terminais quiser:
```bash
docker exec -it ROS bash

# No root que abrir:
root@e73104f1999d:/# source /opt/ros/noetic/setup.bash
```

Se não quiser ficar rodando o comando que realiza a configuração de ambiente, basta rodar:
```bash
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
```
OBS: Isso precisa ser feito toda vez que o container é criado. 

#### ATENÇÃO: Quando estamos trabalhando com container, muitas vezes não vão vir pacotes instalados. Além disso, o container não tem acesso gráfico disponível, então precisamos fazer algumas adaptações. 

Para instalar um pacote, dentro do container:
```bash
apt update
apt install -y ros-$ROS_DISTRO-nomePacote

# $ROS_DISTRO, no nosso caso, é o noetic

# Recarregue as configurações de ambiente:
source /opt/ros/$ROS_DISTRO/setup.bash
```

Exemplo para o noetic:
```bash
apt update
apt install -y ros-noetic-turtlesim

source /opt/ros/noetic/setup.bash

rosrun turtlesim turtlesim_node
```

**CONTUDO**, isso apenas não basta, pois precisamos permitir que programas rodando em containers Docker locais acessem o servidor gráfico X11 do computador. Portanto, rode fora do container:
```bash
# Para dar permissão
xhost +local:docker

# Para tirar a permissão
xhost -local:docker
```
Na prática, ele libera o Docker para abrir janelas na sua tela.

Dessa forma, na hora de criar o container, passar o display e o socket do X11:
```bash
docker run -dit \
  --env DISPLAY=$DISPLAY \
  --volume /tmp/.X11-unix:/tmp/.X11-unix \
  IMAGEM_DOCKER
```