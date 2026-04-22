# Projeto TurtleSim - Parte 3
# Controle - Controlando o TurtleSim para ir a um ponto (gotogoal.py)
**Fonte:** https://wiki.ros.org/turtlesim/Tutorials/Go%20to%20Goal

O objetivo é fazer a turtle sair de um ponto x e chegar a um ponto y utilizando os dois nós criados anteriormente, portanto este nó novo é tanto publisher quando subscriber. 

#### Criar um nó em Python denominado gotogoal.py
```bash
roscd turtlesim_cleaner
cd src
wget https://raw.githubusercontent.com/joaofabro/turtlesim_cleaner/update_python3/src/gotogoal.py 
chmod u+x gotogoal.py
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
rosrun turtle_cleaner rotate.py # Outro terminal
```

## Conceitos Básicos Importantes: 
1) Distância entre dois pontos
    - Distância Euclidiana

2) O funcionamento da função `atan2()` do Python. 

3) Controle Proporcional de um robô/processo: 
Nesse caso, objetiva-se fazer alguma variável controlada atingir algum valor especificado. Dessa forma, opera-se calculando o erro, isto é, quanto esse variável está longe do valor objetivo, a cada passo, e executa uma ação proporcional ao tamanho do erro, o que visa diminuir esse erro. 