O Dilema do Servidor em Nuvem

Diário de Bordo – Sistemas Operacionais

Aluno: Wesley Brene Batista
Tema: Escalonamento de Processos e Chamadas de Sistema

⸻

Introdução

A startup CloudData utiliza um servidor de núcleo único para executar dois tipos de processos: processos interativos da aplicação web e processos em lote (batch), responsáveis pela geração de relatórios.

Atualmente, o servidor utiliza o algoritmo de escalonamento FCFS (First-Come, First-Served). Os usuários estão enfrentando problemas de responsividade, pois a interface web apresenta travamentos durante a execução dos processos.

Este trabalho busca analisar o funcionamento das chamadas de sistema e do escalonamento de processos, identificando o problema e apresentando uma possível solução.

⸻

Parte A – Chamadas de Sistema

1. Como o processo solicita a leitura?

O processo faz uma System Call, pedindo para o Sistema Operacional realizar a leitura no disco, já que ele não pode acessar o hardware diretamente.

2. O que acontece nessa solicitação?

O processo está em Modo Usuário. Ao fazer a System Call, o sistema passa para o Modo Kernel, onde tem permissão para acessar o hardware e realizar a operação. Depois, retorna ao Modo Usuário.

⸻

Parte B – Escalonador

1. Por que o FCFS causa o congelamento?

O FCFS executa os processos pela ordem de chegada. Se um processo pesado estiver usando a CPU, os processos da interface precisam esperar, causando demora na resposta.

2. O que significa não preemptivo?

Significa que o processo que está usando a CPU não é interrompido para outro executar. Ele continua até terminar ou ficar bloqueado, fazendo os outros processos esperarem.

⸻

Parte C – Solução

1. Algoritmo escolhido

Eu escolheria o Round-Robin, pois cada processo recebe um tempo de CPU chamado quantum. Quando esse tempo acaba, outro processo pode executar. Assim, a interface não precisa esperar um processo pesado terminar completamente.

2. Starvation

Starvation acontece quando um processo fica esperando por muito tempo porque outros possuem prioridade maior.

Uma solução é o Aging, que aumenta aos poucos a prioridade dos processos que estão esperando, evitando que fiquem sem executar.

⸻

Pesquisa Multimídia

🎥 Vídeo

Tema: Escalonamento de processos
Fonte: YouTube
Link: Sistemas Operacionais - Escalonamento de Processos

O vídeo foi utilizado para entender melhor o funcionamento dos algoritmos de escalonamento.

🎧 Áudio / Podcast

Tema: Sistemas Operacionais
Fonte: Apple Podcasts
Link: Sistemas Operacionais – Ouvir áudio

O áudio apresenta uma breve introdução aos objetivos e às funções dos sistemas operacionais, complementando o tema do trabalho.

📄 Texto / Artigo

Título: Sistemas Operacionais – Técnicas de Escalonamento
Fonte: Universidade Federal do Paraná (UFPR)
Link: Acessar material

O material apresenta algoritmos como FCFS, SJF, SRTF e Round-Robin.

⸻

Síntese Visual

Processo
↓
System Call
↓
Modo Kernel
↓
Escalonador
↓
FCFS → maior espera
↓
Round-Robin → divisão do tempo da CPU
↓

⸻

Referências

As fontes utilizadas no vídeo, podcast e texto foram consultadas para complementar o conteúdo sobre escalonamento de processos, System Calls e Sistemas Operacionais.

