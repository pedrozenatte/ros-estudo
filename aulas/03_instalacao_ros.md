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