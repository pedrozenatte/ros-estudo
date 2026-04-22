# ROSLAUCH

É um comando (ferramenta) dos ROS para executar múltiplos nós simultaneamente. 
**Como fazer isso?**
Basta especificar em um arquivo .xml com extensão .launch, o qual deve ser colocado na pasta "launch" de um pacote, os nomes dos nós a serem executados. 
Feito isso, é só executar o `roscore` e dar um `rosrun` para cada nó especificado no launchfile. 

## Passos

**OBS: É muito importante lembrar que SEMPRE que iniciar o novo terminal ao entrar na pasta do workspace (catkin_ws) executar o comando:**
```bash
source devel/setup.bash
```
Até porque, se não o ROS não vai identificar os pacotes da pasta. 

1) Criar (se não existir) a pasta "launche" no pacote:
```bash
roscd <nome_pacote>
mkdir launch
```

2) Criar o arquivo com extensão ".launch" na pasta criada: 
```bash
cd launch
touch <nome>.launch
gedit <nome>.launch
```

3) Executar (lançar) os nós com o comando: 
```bash
roslaunch <nome_pacote> <nome_do_launchfile> 
```

**Qual é a utilidade disso?**
Ao invés de abrir um terminal para cada nó, teremos tudo isso em um terminal. Além disso, ele mesmo inicia o roscore se não estiver iniciado. 

## Especificando um launchfile (arquivo.launch)
Esse arquivo é do tipo XML-eXtended Markup Language (linguagem de Marcações - tags - Extendida)
![Launchfile](../imagens/launchfile.jpeg)

Temos dois tipos de tags:
1) Abertura com \<nome_da_tag> e fechamento com \</nome_da_tag>
2) \<nome_tag ...parâmetros... />

**ATENÇÃO:** O launchfile precisa ter extensão .launch e precisa começar com a tar \<launch> na primeira linha, e finalizar com a tag \</launch> na última linha.

Podemos colocar várias "coisas" no launchfile, porém vamos falar hoje sobre o node. 
Dessa forma, para cada tag \<node ...parâmetros... />, os parâmetros mínimos (obrigatórios) são:
- `name = "nome_do_nó_após_lançado"`: Nome do nó lançado. 
- `pkg = "nome_do_pacote"`: nome do pacote onde está esse nó. 
- `type = "nome_do_nó_original"`:nome usado no rosrun. 

Depois, só executar os nós com o comando:
```bash
roslaunch <nome_pacote> <nome_do_launchfile> 
```

## Exemplo:
No nosso exemplo, teremos o launchfile com os seguintes parâmetros: 

```text
<launch>
<node name = "listener" pkg = "beginner_tutorial" type = "listener.py" output = "screen" />
<node name = "talker" pkg = "beginner_tutorial" type = "talker.py" output = "screen" />
</launch>
```

OBS: O parâmetro "output": output = "log|screen", é um parâmetro opcional, mas sem esse, a saída de um nó será escrita no arquivo de log em $ROS_HOME/log (usualmente a pasta .ros/log do usuário, um arquivo para cada node)

**Executar:**
roslaunch begginer_tutorial \<nome>.launch 