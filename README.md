# O Dilema do Servidor em Nuvem

## Diário de Bordo – Sistemas Operacionais

**Aluno:** Wesley Brene Batista  
**Tema:** Escalonamento de Processos e Chamadas de Sistema

---

## Introdução

A startup CloudData utiliza um servidor de núcleo único para executar dois tipos de processos: processos interativos da aplicação web e processos em lote (batch), responsáveis pela geração de relatórios.

Atualmente, o servidor utiliza o algoritmo de escalonamento FCFS (First-Come, First-Served). Os usuários estão enfrentando problemas de responsividade, pois a interface web apresenta travamentos durante a execução dos processos.

Este trabalho busca analisar o funcionamento das chamadas de sistema e do escalonamento de processos, identificando o problema e apresentando uma possível solução.

---

## Parte A – Chamadas de Sistema

### 1. Como o processo solicita a leitura?

O processo faz uma **System Call**, pedindo para o Sistema Operacional realizar a leitura no disco, já que ele não pode acessar o hardware diretamente.

### 2. O que acontece nessa solicitação?

O processo está em **Modo Usuário**. Ao fazer a System Call, a execução passa para o **Modo Kernel**, onde o sistema tem permissão para acessar o hardware e realizar a operação.

Se precisar aguardar o disco, o processo fica bloqueado e libera a CPU para outro processo. Após a leitura, volta ao estado pronto e, quando executar novamente, retorna ao Modo Usuário.

---

## Parte B – Escalonador

### 1. Por que o FCFS causa o congelamento?

O **FCFS** executa os processos pela ordem de chegada à fila de prontos. Se um processo pesado estiver usando a CPU, os processos da interface precisam esperar, causando demora na resposta.

Quando o processo pesado fica bloqueado aguardando o disco, ele libera a CPU. A demora causada pelo FCFS acontece enquanto ele permanece processando na CPU.

### 2. O que significa não preemptivo?

Significa que o processo que está usando a CPU não perde seu turno apenas porque outro chegou ou porque um intervalo de tempo acabou.

Ele continua até terminar, ficar bloqueado ou liberar a CPU voluntariamente, fazendo os outros processos esperarem.

---

## Parte C – Solução

### 1. Algoritmo escolhido

Eu escolheria o **Round-Robin**, pois cada processo recebe um tempo de CPU chamado **quantum**.

Ele é preemptivo: quando esse tempo acaba, o processo que ainda precisa executar volta ao fim da fila de prontos, permitindo que outro use a CPU.

Assim, a interface não precisa esperar um processo pesado terminar completamente. Um quantum muito pequeno aumenta as trocas de contexto; um muito grande pode prejudicar a responsividade.

### 2. Starvation

**Starvation** acontece quando um processo pronto fica esperando indefinidamente porque outros possuem prioridade maior e são sempre escolhidos.

Uma solução é o **Aging**, que aumenta aos poucos a prioridade dos processos que estão esperando, evitando que fiquem sem executar.

---

## Pesquisa Multimídia

### 🎥 Vídeo

**Tema:** Escalonamento de processos  
**Fonte:** UNIVESP – YouTube  
**Título:** Sistemas Operacionais – Aula 06 - Escalonamento de Processo

**Link:**

https://www.youtube.com/watch?v=MWbPgxOCrFk

A aula explica o escalonamento em sistemas interativos, incluindo Round-Robin e prioridades.

### 🎧 Áudio / Podcast

**Tema:** Sistemas Operacionais  
**Fonte:** Apple Podcasts  
**Autora:** Juliana Augusta  
**Título:** Sistemas Operacionais, para que serve!

**Link:**

https://podcasts.apple.com/us/podcast/sistemas-operacionais/id1565021712?l=pt-BR

O áudio apresenta uma breve introdução aos objetivos e às funções dos sistemas operacionais, complementando o tema do trabalho.

### 📄 Texto / Material didático

**Título:** Escalonamento de Processos  
**Fonte:** Instituto Federal da Bahia (IFBA)  
**Autora:** Flávia Maristela Santos Nascimento

**Link:**

https://ads.ifba.edu.br/dl1017

O material explica o escalonamento de processos, incluindo FCFS, Round-Robin e prioridades.

---

## Síntese Visual

![Diagrama de chamadas de sistema e escalonamento](diagrama.png)

**Fonte:** elaboração própria, com base nas fontes indicadas.

O podcast apresenta as funções do Sistema Operacional. A leitura do disco depende de uma System Call, executada pelo kernel. Se o processo ficar bloqueado, outro pode usar a CPU.

O vídeo e o texto explicam o escalonamento: o FCFS pode atrasar tarefas interativas, enquanto o Round-Robin divide o tempo da CPU em turnos.

---

## Referências

AUGUSTA, Juliana. **Sistemas Operacionais, para que serve!** In: Sistemas Operacionais. [S. l.]: Juliana Augusta, 25 abr. 2021. Podcast. Disponível em: https://podcasts.apple.com/us/podcast/sistemas-operacionais/id1565021712?l=pt-BR. Acesso em: 8 out. 2026.

DÖRR, Jéfer Benedett. **Escalonamento de Processos no Linux**. Palotina: Universidade Federal do Paraná, [s.d.]. Material didático online. Disponível em: https://docs.ufpr.br/~jefer/professor/disciplinas/slides/dee355pratica-escalonadores.html. Acesso em: 8 out. 2026.

UNIVESP. **Sistemas Operacionais – Aula 06 - Escalonamento de Processo**. Professor Jó Ueyama. [S. l.]: UNIVESP, 22 maio 2017. Vídeo. Disponível em: https://www.youtube.com/watch?v=MWbPgxOCrFk. Acesso em: 8 out. 2026.