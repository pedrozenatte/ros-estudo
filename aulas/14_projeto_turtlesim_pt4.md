# Projeto TurtleSim - Parte 4 (Final)
# Controle - Controlando o TurtleSim para ir a um ponto (gotogoal.py)

O objetivo é fazer a turtle "limpar"/percorrer o ambiente, como um aspirador de pó automático (robozinho de aspirar). Aqui, o nó também será tanto publisher quanto subscriber. 

#### Criar um nó em Python denominado cleaner.py
```bash
roscd turtlesim_cleaner
cd src
wget https://raw.githubusercontent.com/joaofabro/turtlesim_cleaner/update_python3/src/cleaner.py
chmod u+x cleaner.py
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
  src/gotogoal.py
  src/cleaner.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
```

#### Compilar o ambiente de trabalho
```bash
cd catkin_ws
catkin_make
source devel/setup.bash
```

#### Executar o nó 'gotogoal.py' no pacote
```bash
roscore # Um terminal
rosrun turtlesim turtlesim_node # Outro terminal
rosrun turtle_cleaner cleaner.py # Outro terminal
```

## Criando o arquivo .launch para automatizar a limpeza 
```xml
<launch>
<node name="turtlesim_nodeOK" pkg="turtlesim" type="turtlesim_node"/>
<node name="cleanerOK" pkg="turtlesim_cleaner" type="cleaner.py" output="screen"/>
</launch>
```

Dessa forma, rodar apenas o comando:
```bash
roslaunch turtlesim_cleaner cleaner.launch 
```