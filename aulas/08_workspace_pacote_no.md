# Criando Workspace, Pacote e Nó (publisher)

Para desenvolver aplicações próprias no ROS, é necessário entender como estruturar um projeto dentro do seu ecossistema. O ROS organiza o código em **workspaces**, que contêm **pacotes**, e dentro desses pacotes estão os **nodes**, além de possíveis mensagens e serviços associados.

## Pacotes 
Pacotes são a unidade básica de organização no ROS. Eles agrupam tudo que está relacionado a uma funcionalidade específica, como código-fonte, definições de mensagens, serviços e configurações.
Um exemplo é o pacote **turtlesim**, que contém os nodes, serviços e mensagens utilizados na simulação da tartaruga. Outros exemplos importantes são os pacotes **rospy** e **roscpp**, que fornecem suporte para desenvolvimento em Python e C/C++, respectivamente.

Para gerenciar e explorar pacotes instalados no sistema, utilizamos a ferramenta `rospack`, que permite acessar informações importantes sobre eles.


### Comandos de pacotes do ROS: 
#### Rospack
Obter informações sobre os pacotes instalados no sistema: 
```bash
rospack <comando> 

# OU, em muitos casos

rospack <comando> <nome_pacote>
```

Ao utilizar:
```bash
rospack <tab><tab>
```
é possível visualizar diversos comandos disponíveis. Entre os principais, temos:

1) Listar nomes e pastas:
```bash
rospack list
```
Uma outra forma é: 
```bash
rosrun <tab><tab> 

rosrun <nome_pacote> <tab><tab>
```

2) Listar apenas os nomes dos pacotes: 
```bash
ropack list-names
```

3) Verificar a pasta onde está instalado (o caminho): 
```bash
rospack find <nome_pacote>
```
**Exemplo:** 
```bash
rospack find turtlesim 

/opt/ros/noetic/share/turtlesim
```

4) Listar TODAS as dependências (diretas + indiretas), ou seja, lista pacotes dos quais esse pacote depende:
```bash
rospack depends <nome_pacote>
```

5) Listar apenas dependências diretas: 
```bash
rospack depends1 <nome_pacote>
```

6) Ver todos as opções de rospack:
```bash
rospack help
```

#### Roscd (navegação entre pacotes)
1) Mudar (change) a pasta (directory) onde está o pacote: 
```bash
roscd <nome_pacote> 
```

#### Rosls
2) Listar o conteúdo (arquivos e pastas) de um pacote:
```bash
rosls <nome_pacote
```

## Criar os próprios pacotes: 
Antes de criar pacotes próprios, é necessário criar um **workspace**, que é o ambiente onde os pacotes serão desenvolvidos.

A estrutura padrão de um workspace ROS (catkin) é:
```
workspace/
 ├── src/
 ├── build/
 ├── devel/
```

#### Criar um workspace catkin
1) Criar uma pasta "catkin_ws" no diretório \<home> do usuário Linux:
```bash
mkdir -p catkin_ws/src
cd /catkin_ws # É um nome default
```
OBS: Para verificar qual pasta estamos (o caminho da pasta), podemos utilizar o comando:
```bash
pwd
```
**Exemplo:**
```bash
pedro@pedro-Aspire-A515-54G:~$ pwd

/home/pedro
# Estamos no diretório home do usuário 
```

Além disso, a flag -p usada é para criar todos os diretórios necessários no caminho, se não existirem.

2) Criar o workspace:

Dentro da pasta, executar:
```bash
catkin_make
```

**ATENÇÃO:** O nome da pasta é totalmente livre, podemos usar qualquer nome, o que realmente importa é que ela precisa ter a estrutura correta: 
workspace/
 ├── src/
 ├── build/   (criado pelo catkin_make)
 ├── devel/   (criado pelo catkin_make)
Contudo, por convenção da comunidade, é melhor usar catkin_ws.


#### Criar um novo pacote
Vamos criar um novo pacote chamado beginner_tutorials, e esse pacote terá dependências de **std_msgs, roscpp e rospy**:

Para isso, primeiro temos que **estar dentro da pasta do workspace/src** e usar o comando:
```bash
cd <workspace>/src
catkin_create_pkg <nome_pacote> <dependência1> <dependência2> ... <dependênciaN>
```

**Exemplo:** 
```bash
cd catkin_ws
catkin_create_pkg beginner_tutorial std_msgs rospy roscpp
```

Feito isso, dentro da pasta criada, que no nosso exemplo é beginner_tutorial, teremos que ter a seguinte estrutura: 
begginer_tutorial/
 ├── CMakeLists.txt
 ├── include
 ├── package.xml
 ├── scripts
 ├── src

Tanto include quanto scripts são pastas de código fonte. 
CMakeLists.txt é arquivo de compilação e possui a lista dos nós, mensagens e serviços do pacote.
Já o package.xml é o arquivo de configuração do pacote. 
OBS: O scripts normalmente é criado de forma manual. 


#### Criar um Nó em Python do tipo Publisher
Um node em ROS nada mais é do que um programa executável, no caso de Python, é um script em Python.
Primeiro, vamos entrar na pasta do pacote (nesse caso, beginner_tutorial):
```bash
cd beginner_tutorial
```

Agora, vamos criar a pasta *scripts*:
```bash
mkdir scripts
```

Vamos entrar nela e criar o nó
```bash
cd scripts

# Criando o nó
wget https://raw.githubusercontent.com/ros/ros_tutorials/noetic-devel/rospy_tutorials/001_talker_listener/talker.py

# Para dar permissão ao arquivo
chmod +x talker.py
# Até o momento não foi necessário
# Pode ser que se ocorra um problema, seja a falta disso.
```
OBS: Esse wget baixa um arquivo chamado *talker.py*, que está representando o nosso nó. Se não quisermos usar ele, criamos um programa em Python ou C++ com o que queremos que o nó faça. 

#### Registro do Nó no pacote
Agora precisamos realizar a **instalação de executáveis do pacote**, que é informar o que foi feito atraveś do CMakeLists.txt (usando gedit), isto é, fazer com que o ROS reconheça formalmente o script como parte do pacote. Sendo assim, na pasta principal do pacote (no nosso exemplo é beginner_tutorial) faremos:
```bash
gedit CMakeLists.txt
```
E ao final do arquivo colocaremos as seguintes instruções:
```text
catkin_install_python(PROGRAMS
  scripts/talker.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
```
SALVE o arquivo!

**PERGUNTINHA:** Por que fazemos isso? 
O comando catkin_install_python é uma regra de instalação do sistema de build do ROS (catkin) utilizada para registrar scripts como executáveis oficiais de um pacote. Ao utilizá-lo, informamos ao ROS que aquele arquivo deve ser tratado como parte integrante do pacote e, portanto, copiado para um diretório padrão durante o processo de instalação (catkin_make install). Sem essa etapa, o script continua existindo apenas dentro da pasta scripts do workspace e funciona localmente, mas não está formalmente instalado nem padronizado dentro da estrutura do ROS. Já com essa regra, o script passa a ser reconhecido como um executável do pacote, permitindo melhor organização, portabilidade e uso em outros ambientes ou sistemas.

OBS: Para cada nó que criarmos, precisa ser feito isso.

#### Compilação e configuração do ambiente
Agora, vamos adicionar este workspace ao ROS para que rospack, roscd, rosls e rosrun o reconheçam: 
```bash
cd catkin_ws
catkin_make
# Permitindo que o rosrun reconheça o pacote
source devel/setup.bash
```


Esse comando atualiza as variáveis de ambiente do ROS, permitindo que ele reconheça os novos pacotes e nodes criados.
Sem esse passo, comandos como `rosrun` não encontrarão o pacote.

OBS: Após o comando *catkin_make*, deve ser reconhecido o novo nó com algo parecido com: 
```text
-- ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
-- ~~  traversing 1 packages in topological order:
-- ~~  - beginner_tutorial
-- ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
-- +++ processing catkin package: 'beginner_tutorial'
-- ==> add_subdirectory(beginner_tutorial)
-- Installing devel-space wrapper /home/pedro/catkin_ws/src/beginner_tutorial/scripts/talker.py to /home/pedro/catkin_ws/devel/lib/beginner_tutorial
-- Configuring done
-- Generating done
-- Build files have been written to: /home/pedro/catkin_ws/build
####
#### Running command: "make -j8 -l8" in "/home/pedro/catkin_ws/build"
####
```

##### Como executar o nó criado ("talker.py") no pacote beginner_tutorial?
```bash
rosrun beginner_tutorial talker.py
```

**ATENÇÃO:** Sempre que abrir um novo terminal para trabalhar com o workspace, é necessário executar:
```bash
source devel/setup.bash
```
Se não fizer isso, o ROS nem reconhece o pacote criado. 





---
# Erros 
Na hora de executar o comando `catkin_make`, houve um problema por conta da versão do python e a utilização do pyenv ao invés do python do sistema. 

```bash
pedro@pedro-Aspire-A515-54G:~/catkin_ws$ catkin_make
Base path: /home/pedro/catkin_ws
Source space: /home/pedro/catkin_ws/src
Build space: /home/pedro/catkin_ws/build
Devel space: /home/pedro/catkin_ws/devel
Install space: /home/pedro/catkin_ws/install
Creating symlink "/home/pedro/catkin_ws/src/CMakeLists.txt" pointing to "/opt/ros/noetic/share/catkin/cmake/toplevel.cmake"
####
#### Running command: "cmake /home/pedro/catkin_ws/src -DCATKIN_DEVEL_PREFIX=/home/pedro/catkin_ws/devel -DCMAKE_INSTALL_PREFIX=/home/pedro/catkin_ws/install -G Unix Makefiles" in "/home/pedro/catkin_ws/build"
####
-- The C compiler identification is GNU 9.4.0
-- The CXX compiler identification is GNU 9.4.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Using CATKIN_DEVEL_PREFIX: /home/pedro/catkin_ws/devel
-- Using CMAKE_PREFIX_PATH: /opt/ros/noetic
-- This workspace overlays: /opt/ros/noetic
-- Found PythonInterp: /home/pedro/.pyenv/shims/python3 (found suitable version "3.11.9", minimum required is "3") 
-- Using PYTHON_EXECUTABLE: /home/pedro/.pyenv/shims/python3
-- Using Debian Python package layout
-- Could NOT find PY_em (missing: PY_EM) 
CMake Error at /opt/ros/noetic/share/catkin/cmake/empy.cmake:30 (message):
  Unable to find either executable 'empy' or Python module 'em'...  try
  installing the package 'python3-empy'
Call Stack (most recent call first):
  /opt/ros/noetic/share/catkin/cmake/all.cmake:164 (include)
  /opt/ros/noetic/share/catkin/cmake/catkinConfig.cmake:20 (include)
  CMakeLists.txt:58 (find_package)


-- Configuring incomplete, errors occurred!
See also "/home/pedro/catkin_ws/build/CMakeFiles/CMakeOutput.log".
Invoking "cmake" failed
```

Isso ocorreu, pois o ROS está usando o Python do pyenv, e o módulo 'empty' não está instado. Portanto:
```bash
sudo apt update
sudo apt install python3-empy
```

Como no ROS Noetic no Ubuntu 20.04, o mais esperado é usar o Python do sistema, é melhor mudar isso: 
Primeiro, vamos verificar se realmente está usando o errado:
```bash
which python3

-> /home/pedro/.pyenv/shims/python3
# Realmente está usando o pyenv

python3 --version

-> Python 3.11.9

# Python do sistema:
/usr/bin/python3 --version

-> Python 3.8.10
```

- Para resolver isso, vamos forçar a criação da pasta com o python do sistema:
```bash
catkin_make -DPYTHON_EXECUTABLE=/usr/bin/python3
```

- Uma outra forma, e tirando o controle do pyenv:
```bash
nano ~/.bashrc
```
E procure linhas como: 
```text
export PATH="$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```
Basta comentá-las. 

---
