# Remapeamento de Tópicos

O remapeamento de tópicos no ROS é uma maneira de mudar a origem ou o destino de tópicos de comunicação entre nós sem precisar modificar o código dos nós que estão publicando ou assinando os tópicos. Esse remapeamento é útil, por exemplo, quando você tem nós com nomes fixos, mas deseja alterar dinamicamente os tópicos que eles utilizam, seja por questões de organização, ou para lidar com diferentes configurações e sistemas.

## Como o remapeamento de tópicos funciona?

O remapeamento de tópicos é feito no momento em que você executa o nó, por meio de argumentos na linha de comando, ou em arquivos de configuração, como launch files.

### Remapeamento na linha de comando

Ao executar um nó com rosrun ou roslaunch, você pode usar o remapeamento diretamente na linha de comando. Quando você usa o remapeamento, o nome do tópico que o nó publica ou assina é alterado para um novo nome, permitindo que ele se conecte a outros nós ou tópicos sem modificar o código.

```bash
rosrun beginner_tutorial talker _chatter:=/custom_topic
```

Aqui, o nó `talker` irá publicar no tópico `/custom_topic`, em vez do padrão `/chatter`. 
OBS: O nome do tópico que o nó está usando foi alterado através do argumento `_chatter:=/custom_topic`.

#### Exemplo:

Vamos spawnar duas turtles
```bash
# Rodar o roscore em um terminal
roscore
```

```bash
# Rodar a turtle em outro
rosrun turtlesim turtlesim_node
```

```bash
# Spawnar mais uma turtle
rosservice call /spawn "x: 3.0
y: 3.0
theta: 0.0
name: 'turtle2'" 
```

Se analisarmos o node da turtlesim, percebe-se que teremos duas turtles (1 e 2)
```bash
rosnode info /turtlesim
```

Agora vamos informar de qual turtle para qual turtle vamos realizar a cópia dos comandos de posição. Para isso: 
- Executar o nó "mimic" do pacote "turtlesim", que replica os comandos enviados para o tópico "/turtle1/cmd_vel" para o "/turtle2/cmd_vel".
- Para fazer isso, esse nó lê a velocidade linear e angular do tópico "input/pose" e republica isso no tópico "/output/cmd_vel".
- Sendo assim, é necessário remapear o /input/pose para /turtle1/pose, e remapear também o tópico /output/cmd_vel para /turtle2/cmd_vel. 

```bash
rosrun turtlesim mimic input/pose:=turtle1/pose output/cmd_vel:=turtle2/cmd_vel
```

Para ver a mágica acontecendo, rode o nó que lê as teclas do teclado:
```bash
rosrun turtlesim turtle_teleop_key
```

Dessa forma, teremos a turtle2 copiando os movimentos da turtle1. 

### Remapeamento em arquivos .launch (remapeamento automatizado)
Além de ser feito na linha de comando, o remapeamento também pode ser configurado dentro de arquivos launch, o que facilita o controle e a organização de vários nós em um único lugar.

Em um arquivo launch, o remapeamento de tópicos é feito da seguinte maneira:
```xml
<launch>
    <node name="talker" pkg="beginner_tutorial" type="talker.py" output="screen">
        <remap from="chatter" to="/custom_chatter"/>
    </node>
</launch>
```
Nesse exemplo, o nó talker irá publicar no tópico /custom_chatter ao invés de /chatter, graças à tag \<remap>.

#### Exemplo: 
Para isso, vamos baixar alguns arquivos de exemplo: 
```bash
cd catkin_ws
source devel/setup.bash
roscd turtlesim_cleaner
cd launch
wget https://raw.githubusercontent.com/joaofabro/turtlesim_cleaner/update_python3/launch/mimic1.launch
wget https://raw.githubusercontent.com/joaofabro/turtlesim_cleaner/update_python3/launch/mimic2.launch
wget https://raw.githubusercontent.com/joaofabro/turtlesim_cleaner/update_python3/launch/mimic3.launch
```

OBS: Está sendo usado o nome "mimic", pois é para indicar que isso serve para imitar o comportamento de um robô para outro, que no nosso caso será a turtle.

Agora, vamos abrir o `mimic1.launch` e colocar: 
```xml
<launch>

<node pkg="turtlesim" name="turtlesim" type="turtlesim_node" />

<node pkg="rosservice" type="rosservice" name="spawn" args="call /spawn 3.0 3.0 0.0 'turtle2'" />

<node pkg="turtlesim" name="mimic1" type="mimic">
<remap from="input/pose" to ="turtle1/pose" />
<remap from="output/cmd_vel" to ="turtle2/cmd_vel" />
</node>

<node pkg="turtlesim" name="teleop" type="turtle_teleop_key" />

</launch>
```

**Outra maneira de criar o launchfile remapeando diretamente o input para o turtle1 e o output para o turtle2 é:**
Vamos abrir o `mimic2.launch` e colocar:  
```xml
<launch>

<node pkg="turtlesim" name="turtlesim" type="turtlesim_node" />

<node pkg="rosservice" type="rosservice" name="spawn" args="call /spawn 3.0 3.0 0.0 'turtle2'" />

<node pkg="turtlesim" name="mimic2" type="mimic">
<remap from="input" to ="turtle1" />
<remap from="output" to ="turtle2" />
</node>

<node pkg="turtlesim" name="teleop" type="turtle_teleop_key" />

</launch>
```
Nesse caso, todo tópico que possuir input no seu nome, será substituído por turtle1, e todo tópico que possuir output no seu nome, será substituído por turtle2. 

**Mais um maneira é com o conceito de namespaces do ROS (group ns = "nome"):**
Vamos abrir o `mimic3.launch` e colocar: 
```xml
<launch>

<group ns="turtlesim1">
<node pkg="turtlesim" name="sim" type="turtlesim_node" />
</group>

<group ns="turtlesim2">
<node pkg="turtlesim" name="sim" type="turtlesim_node" />
</group >

<node pkg="turtlesim" name="mimic3" type="mimic">
<remap from="input" to ="turtlesim1/turtle1" />
<remap from="output" to ="turtlesim2/turtle1" />
</node>

<node pkg="turtlesim" name="teleop" type="turtle_teleop_key">
<remap from="turtle1/cmd_vel" to ="turtlesim1/turtle1/cmd_vel" />
</node>

</launch>
```

##  Por que o remapeamento é útil?
- Reutilização de código: Você pode criar nós reutilizáveis que sempre publicam ou assinam tópicos com nomes fixos, mas remapear esses nomes para se adaptarem a diferentes cenários.
- Facilidade de configuração: Em sistemas grandes, o remapeamento facilita a configuração de nós sem ter que alterar o código-fonte. Em vez de mudar os tópicos diretamente no código, você pode alterar os tópicos dinamicamente em tempo de execução.
- Evita conflitos de nome: O remapeamento é útil quando você tem múltiplos sistemas ou módulos que precisam se comunicar entre si, mas os tópicos têm os mesmos nomes. Remapeando os tópicos, você pode evitar esses conflitos de nome.
- Organização e modularidade: Em sistemas grandes e modulares, você pode querer alterar o nome dos tópicos dependendo do módulo, configuração ou ambiente. O remapeamento permite que você defina essas mudanças sem necessidade de recompilar ou editar os arquivos de código.