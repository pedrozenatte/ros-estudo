# Projeto TurtleSim - Parte 1
# Criando um nó "move.py" para o TurtleSim
**Fonte:** https://wiki.ros.org/turtlesim/Tutorials/Moving%20in%20a%20Straight%20Line

O objetivo será criar um nó "move.py" que controla o movimento em linha reta da turtle no TurtleSim (só Publisher).

- Vamos criar um pacote denominado "turtlesim_cleaner". 

Esse pacote vai permitir a criação de nós capazes de controlar a movimentação do robô "turtle1" do TurtleSim. 

#### Criar um workspace denominado turtlesim_cleaner:
Como já temos um workspace criado, vamos reaproveitar e criar o pacote no mesmo workspace
```bash
cd catkin_ws/src
catkin_create_pkg turtlesim_cleaner geometry_msgs rospy
```
Perceba que temos o nome "turtlesim_cleaner" e duas dependências, "geometry_msgs" e "rospy". 

#### Criar um nó em Python denominado move.py
```bash
roscd turtlesim_cleaner
cd src
wget https://raw.githubusercontent.com/clebercoutof/turtlesim_cleaner/master/src/move.py
```

Vamos tornar o arquivo move.py como executável.
Dentro da pasta src (ou com o caminho dela até o arquivo.py), rodar:
```bash
chmod u+x move.py
```
OBS: "chmod" é o comando linux que muda o modo de um arquivo, e o "u+x" indica que o arquivo passa a ser executável (x) pelo usuário (u). 

#### Editar o arquivo CMakeLists.txt
Vamos fazer com que o ROS reconheça formalmente o script como parte do pacote. Sendo assim, na pasta principal do pacote, turtlesim_cleaner faremos:
```bash
gedit CMakeLists.txt
```

E ao final do arquivo colocaremos as seguintes instruções:

```text
catkin_install_python(PROGRAMS
  src/move.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
```

#### Compilar o ambiente de trabalho
```bash
cd catkin_ws
catkin_make
source devel/setup.bash
```

#### Executar o nó 'move.py' no pacote
**ATENÇÃO:** Uma linha em cada terminal, e sempre lembrando de rodar o `source devel/setup.bash` em cada terminal que for rodar o nó criado da pasta worspace. 
```bash
roscore # Um terminal
rosrun turtlesim turtlesim_node # Outro terminal
rosrun turtle_cleaner move.py # Outro terminal
```

**OBS:** A princípio vai dar erro ao informar os valores, e isso acontece, pois a versão do Python utilizada quando criaram o código é antiga, então ele considera a função "input()" como string, ou seja, precisa colocar float(input(...)) e int(input(...)) no código.  