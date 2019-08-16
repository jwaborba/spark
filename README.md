# spark

--Qual o objetivo do comando cache em Spark?

Para entender melhor o comando cache é necessário ter o conceito de RDD bem claro. RDD (Resilient Distributed Dataset) é uma abstração que o spark faz para paralelizar o processamento. Em termos práticos, você pode salvar um objeto em RDD.
O comando cache entra em cena como uma técnica de otimização. Ele faz com que o spark salve a informação do RDD em memória para que ela possa ser reusada em estágios futuros. Aumentando assim a performance do processamento.


--O mesmo código implementado em Spark é normalmente mais rápido que a implementação equivalente em MapReduce. Por quê? 

A resposta dessa pergunta está na arquitetura do MapReduce e do Spark. Ambos são capazes de processar grandes volumes de dados, porém o fazem de forma diferente: 
O Map Reduce é um algoritmo que processa dados que o faz basicamente em 2 estágios: 
1.	Map – Onde as desagregações são feitas e os dados são mapeados
2.	Reduce – onde os dados são novamente agregados de modo que sejam ‘somados’ cada dado idêntico encontrado. Essas agregações são formas menos custosas para se armazenar grandes conjuntos de dados.
Esse modelo de processamento apesar de funcionar é oneroso computacionalmente pois o MapReduce lerá todas as linhas e aplicará os 2 passos a todos objetos identificados.
Enquanto Spark se utiliza do cluster (HDFS) ‘colocando’ a JVM em cada node e o processamento acontece em paralelo em cada nó do cluster, assim paralelizando o processamento sem precisar mover toda a massa de dados para um ambiente externo ao HDFS.
Para a gestão, tanto do HDFS quanto do Spark dentro do cluster é usado o YARN.

--Qual é a função do SparkContext?

SparkContext é um ‘ambiente’ no qual onde suas ações se relacionam ao spark. Ele se relaciona a todas aplicações que interagem com o spark como os gerenciadores de cluster, linguagem python etc...

--Explique com suas palavras o que é Resilient Distributed Datasets (RDD). 

Parcialmente respondido na primeira, mas reforçando, o RDD (Resilient Distributed Dataset) é uma abastração que o Spark faz, um ‘formato’ de objeto onde o spark consegue aplicar suas principais funções como paralelismo. 

--GroupByKey é menos eficiente que reduceByKey em grandes dataset. Por quê?

reduceByKey o dado vai ser combinado primeiro (feito o reduce) e depois agrupado (mesmo conceito de ‘reduce’ do MapReduce)
GroupByKey não há o processo de ‘reduce’, então toda a massa de dado é vasculhada e agrupada.
A parte da função ‘reduce’ da função ‘reduceByKey’ é responsável pela melhor performance.

** Explique o que o código Scala abaixo faz **

1. val textFile = sc . textFile ( "hdfs://..." )
2. val counts = textFile . flatMap ( line => line . split ( " " ))
3.           . map ( word => ( word , 1 ))
4.           . reduceByKey ( _ + _ )
5. counts . saveAsTextFile ( "hdfs://..." )

--linha 1 - o arquivo texto é lido e atribuído ao objeto textFile
--linha 2 - cada linha é colocada em uma coleção de palavras
--linha 3 - é feito um mapeamento de cada linha colocando em chave-valor onde chave é a linha e valor é 1
--linha 4 - os valores são agrupados pela chave (palavra) e os valores de chaves iguais se somam
--linha 5 - o RDD com o MapReduce feito é salvo em arquivo texto

