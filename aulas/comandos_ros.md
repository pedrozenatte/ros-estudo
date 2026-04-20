# Comandos ROS

Vamos centralizar todos os comandos

---

1) Inicializar a Master ROS:
```bash
roscore
```

---

2) Verificar qual a versão/distribuição do ROS:
```bash
rosversion -d
```

---

3) Executar novos nós do ROS:
```bash
rosrun <nome_pacote> <nome_nó>
```

4) Verificar nós ativos:
```bash
rosnode list
```

---

5) Verificar as informações dos nós, como onde ele publica, onde subscreve, quais serviços:
```bash
rosnode info </nome_nó>
```

---

6) Verificar tópicos existentes (listar todos os tópicos)
```bash
rostopic list
``` 

---

7) Verificar o tipo do tópico:
```bash
rostopic type </nome_tópico> 
```
O Resultado indica a mensagem que pode ser trafegada no tópico. 

---

8) Verificar mais informações do tópico:
```bash
rostopic info </nome_tópico>
```
Dessa forma, podemos ver qual nó está publicando nesse tópico e qual nó está subscrevendo.

---

9) Verificar a lista de todas as mensagens que estão disponíveis em todos os pacotes da instalação do ROS:
```bash
rosmsg list
```

---

10) Verificar detalhes de uma mensagem específica:
```bash
rosmsg show </nome_mensagem>
```

---

11) Acompanhar o que está sendo publicado no tópico pelo nó: 
```bash
rostopic echo </nome_tópico> 
```

---

12) Verificar quantas atualizações (mensagens) estão sendo publicadas no tópico pelo nó a cada segundo? 
```bash
rostopic hz </nome_tópico>
```
O resultado é em hz. 
---

13) Publicar uma mensagem em um tópico:
```bash
rostopic pub </nome_tópico> <tipo_msg> <valores_campos_msg> 
```
OBS: Para os valores de campos, melhor usar o \<tab>\<tab>

---

14) Publicar uma mensagem continuamente em um tópico:
```bash
rostopic pub -r <freq> </nome_tópico> <tipo_msg> <valores_campos_msg> 
```
A frequência é em hz, então se \<freq> = 10, teremos a mensagem sendo publicada 10 vezes por segundo. 

---

15) Abrir a ferramenta *rqt*, basta rodar o comando: 
```bash
rqt
```

---

16) Verificar qual Nó está publicando/subscrevendo em qual tópico de forma gráfica:
```bash
rqt_graph
```
OBS: Também é com a ferramenta *rqt*.


---