# Projeto TurtleSim - Parte 2
# Criando um nó "rotate.py" para o TurtleSim
**Fonte:** https://wiki.ros.org/turtlesim/Tutorials/Rotating%20Left%20and%20Right

O objetivo será criar um nó denominado rotate.py que controla o movimneto rotacional da tartaruga no TurtleSim (só Publisher). 

#### Criar um nó em Python denominado rotate.py
```bash
roscd turtlesim_cleaner
cd src
wget https://raw.githubusercontent.com/joaofabro/turtlesim_cleaner/update_python3/src/rotate.py
chmod u+x rotate.py
```

#### Editar o arquivo CMakeLists.txt da pasta do pacote
Vamos fazer com que o ROS reconheça formalmente o script como parte do pacote. Sendo assim, na pasta principal do pacote, turtlesim_cleaner faremos:
```bash
gedit CMakeLists.txt
```

E ao final do arquivo colocaremos as seguintes instruções:

```text
catkin_install_python(PROGRAMS
  src/move.py
  src/rotate.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
```

#### Compilar o ambiente de trabalho
```bash
cd catkin_ws
catkin_make
source devel/setup.bash
```

#### Executar o nó 'rotate.py' no pacote
```bash
roscore # Um terminal
rosrun turtlesim turtlesim_node # Outro terminal
rosrun turtle_cleaner rotate.py # Outro terminal
```

