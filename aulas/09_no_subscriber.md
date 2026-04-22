# Criação de Nós do tipo Subscriber em Python

#### Criação do Nó
Considerando o que já foi feito anteriormente, isto é, toda a criação do workspace e das pastas necessárias, vamos criar um nó do tipo subscriber com `wget https://raw.github.com/ros/ros_tutorials/kinetic-devel/rospy_tutorials/001_talker_listener/listener.py`. 

Na pasta scripts do projeto, faremos: 
```bash
# Pasta
roscd beginner_tutorials/scripts/

# Nó subscriber
wget https://raw.github.com/ros/ros_tutorials/kinetic-devel/rospy_tutorials/001_talker_listener/listener.py

chmod +x listener.py
```

OBS: Esse wget baixa um arquivo chamado *listener.py*, que está representando o nosso nó do tipo subscriber.


#### Registro do Nó no pacote

Vamos novamente editar o arquivo CMakeLists.txt para que o ROS reconheça formalmente o script como parte do pacote. 

**ANTES:**
```text
catkin_install_python(PROGRAMS
  scripts/talker.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
```
nosso arquivo estava assim, agora vamos atualizar. 

**DEPOIS:**
```text
catkin_install_python(PROGRAMS
  scripts/talker.py
  scripts/listener.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
```

Sobre o NÓ: Esse nó vai se subscrever no tópico /chatter. 

#### Compilação e configuração do ambiente
Agora, vamos adicionar este workspace ao ROS para que rospack, roscd, rosls e rosrun o reconheçam: 
```bash
cd catkin_ws
catkin_make
# Permitindo que o rosrun reconheça o pacote
source devel/setup.bash
```

##### Como executar o nó criado ("listener.py") no pacote beginner_tutorial?
```bash
rosrun beginner_tutorial listener.py
```

**ATENÇÃO:** Sempre que abrir um novo terminal para trabalhar com o workspace, é necessário executar:
```bash
source devel/setup.bash
```
Se não fizer isso, o ROS nem reconhece o pacote criado. 