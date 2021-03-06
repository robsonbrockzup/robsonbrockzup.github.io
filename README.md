### index


[![](img/docker_logo.png)](#docker)
[![](img/kafka_logo.png)](#kafka)
[![](img/redis_logo.png)](#redis)
[![](img/git_logo.png)](#git)

---
---



<!-- DOCKER -->

# docker

<p align="center">
  <img src="https://res.cloudinary.com/practicaldev/image/fetch/s--HDTY4BaV--/c_imagga_scale,f_auto,fl_progressive,h_420,q_auto,w_1000/https://dev-to-uploads.s3.amazonaws.com/i/gbg8l5odbobqoabk7s5j.png" width="350" title="hover text">
</p>

**URLs**
- [docker > get start](https://www.docker.com/get-started)
- [instalar o docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
- [instalar o docker - digital ocean](https://www.digitalocean.com/community/tutorials/como-instalar-e-usar-o-docker-no-ubuntu-18-04-pt)
- [Treinamento online - kodekloud.com](https://kodekloud.com/)
- [Limpando contêineres, imagens e volumes - macoratti.net](http://www.macoratti.net/19/02/dock_limp1.htm)

<!-- **ESTUDO**
- https://www.youtube.com/watch?v=sjppkJjIvT4&list=PLf-O3X2-mxDkiUH0r_BadgtELJ_qyrFJ_
- https://www.youtube.com/watch?v=fqMOX6JJhGo
    - 2h10m - Docker Tutorial for Beginners - A Full DevOps Course on How to Run Applications in Containers -->



__


inicia o container com a imagem e, se não existir na minha máquina, o docker vai buscar no dockerHub

    $ docker run <nome-da-imagem>
        * para informar a tag da imagem, usar run image:tag, exemplo $ docker run redis:4.0
        -ti (t=terminal de console | i=interativo)
        -d => roda como um deamon (um processo background)
        -p => externaliza uma porta interna; ex $ run -p 80:5000 indica que a porta 80 do host aponta para a porta 5000 interna
        -v -> mapeia um volume para transferência de arquivos; ex: $ docker run -v /opt/meuDiretorioNoHost:/var/lib/mysql mysql
        -e => para informar uma variável; ex: & run -e MINHA_CHAVE=meuValor
        --name => informa um nome para o container; ex: run --name=db
        --link => informa um link entre containers; pode ter mais de 1 link; ex: --link db:db --link redis:redis

pára a execução de um container 

    $ docker stop <nome-do-container>

Download da imagem sem executar (run)

    $ docker pull <nome-da-imagem>

executa um comando em um container em execução

    $ docker-compose exec kafka \kafka-console-consumer --bootstrap-server localhost:29092 --topic product --from-beginning
    $ docker-compose down --remove-orphans
    $ docker run --name ubuntu_bash --rm -i -t ubuntu bash
    $ docker exec -d ubuntu_bash touch /tmp/execWorks
    $ docker exec -it -e VAR=1 ubuntu_bash bash
    $ docker exec -it -w /root ubuntu_bash pwd
        -w => informa o diretório de trabalho em que o comando será executado

mostra todas as imagens que já existem na minha máquina

    $ docker images

lista os container

    $ docker ps -a
    -a 
        mostra todos os containers da máquina, pois mesmo desligados, eles usam espaço na máquina 

sai do console (bash) do container em execução mas não mata o processo

    $ Ctrl + PQ

conecta no console (bash) do container em uso

    $ docker attach <container_id>

pára um container em execução

    $ docker stop <container_id>

pausa um container em execução

    $ docker pause <container_id>
    
    para 'des'pausar:
        docker unpause <container_id>

informações sobre os recursos utilizados (memória, cpu, rede...)

    $ docker stats <docker_id>

traz os processos que rodam nesse container

    $ docker top <container_id>

logs

    $ docker logs <container_id>
    $ docker logs -f <id>

remove o container

    $ docker rm <container_id>
    -f = força a remoção/deleção se o container está startado
    $ docker container prune
    remove todos os containers

remove uma imagem

    $ docker rmi <nome-da-imagem>
    ! garantir que não tenha nenhum container em execução com a imagem

mostra todas as informações mais detalhadas do container

    $ docker inspect <container_id>
    -f = formata as strings


## **limitando o uso de recursos dos containers**

para memória

    $ free -m
        mostra a quantidade de memória ram de uma máquina linux
        não mostra a quantidade do host, e não da máq. docker

    $ docker inspect f2839fadd0d6 | grep -i mem
        -i mem = inspeciona o container com foco na memória
        -i cpu = foco na cpu (mais infos, siga estas anotações)

    $ docker run -ti --memory 512m --name novo_teste debian
        --memory ou -m
        run = starta um container
        --memory = já limitando o uso de memória para 512m
        --name = dá um nome para o container (ao invés de usar o container_id toda vez)

    $ docker update --memory 256m novo_teste (ou container_id)
        modifica o limite de memória do container mesmo em execução


para cpu

    $ docker run -ti --cpu-shares 1024 --name container1 debian
        --cpu-shares = limita a quantidade de cpu para 1m
        --name container1 = da um nome para o container
        debian = base para criação do container

    $ docker inspect container1 | grep -i cpu
        -i cpu = foco na cpu


volume

é um diretório de 'compartilhamento' entre o host e o container

    $ df -h
        df = mostra as partiçoes e o quanto está utilizando detalhadas

inicia um container já criando um volume chamado '/volume' dentro do container

    $ docker run -ti -v /volume ubuntu /bin/bash


    $ docker run -ti -v /root/primeiro_dockerfile:/volume --name teste_volume2 debian
        /root/volume/path:/docker/volume/path
        -v = path da máq root onde será criado o volume (-v)
        :/volume = path do container que estará apontando para o volume da máq root

        $ ... -volume-from = é para importar dentro de um container o volume de outro container



## **build**

cria a imagem

    $ docker build -t NOME_DA_IMAGEM:1.0/ .
        -t = nome da imagem : versão da imagem
        . = path do arquivo Dockerfile

cria um container utilizando a imagem criada acima

    $ docker run -ti webserver:1.0

mostra os processos que estão rodando

    $ ps -ef

lista os IP ativos na máquina

    $ ip addr

verifica as portas ativas

    $ ss -s ( ou -a )
        ss é igual netstat

verifica se o ip informado está rodando e escutando na porta 

    $ telnet 172.17.0.2 80
        informada (80)
        (digitar quit pra sair)

inicia o apache2

    $ /etc/init.d/apache2 start

___

## Outros

    $ docker inspect ...
    $ docker history <container_id>
    pode acessar o site https://imagelayers.io/

login com acesso ao DockerHub

    $ docker login

tag

    $ docker tag <container_id> <novo_nome>; ex:
    $ docker tag 4daae440bf9e rofideb/webserver:1.0

    $ docker push rofideb/webserver

    $ docker search rofideb

remover uma imagem do dockerHub

    $ docker rmi -f <imageId>
        -f = força a remoção



## Registry
é como uma central de docker images

    $ docker run -d -p 5000:5000 --restart=always --name registry registry:2
        -d = roda com um processo (daemo)
        -p = limkar a porta do docker host com a porta do container
        5000 = é a porta para o Registry
        --restart = se o docker tiver qualquer problema, ou o host for restartado, o registry também será startado, pois é importantíssimo este container estar sempre ligado
        registry:2 = imagem base na versão 2

a nova máquina deve estar rodando

    $ docker ps

para subir uma imagem para o nosso Registry, precisamos tagear a imagem para localhost, ou seja:

    $ docker tag 4daae440bf9e localhost:5000/webserver:1.0

para uma versão gráfica, pode-se pesquisar no google por "docker registry UI"

    $ docker push localhost:5000/webserver

    $ curl localhost:5000/v2/_catalog
        para ver todas as imagens no Registry


Parâmetros de rede

    $ docker run -ti \
        --name um_nome_bacana
        --dns 8.8.8.8 \
        --hostname bla \
        --link <container_id> \
        --expose 88 \
        --publish 8080:80 \
        --mac-address 12:34:de:b0:6b:61 \
        --net=bridge (ou =host) \
        debian
            --dns = para informar um dns =)
            --hostname = define um hostnamen para o container (diferente do nome do container, que é como um identificados do container visto direto no host)
            --link = linka o container a outro container, o que permite que quando este for criado, já conheça (e acesse) o container linkado
            --expose = expoe uma porta do container para o host (igual o visto no dockerfile)
            --publish / -p = bind (ou conecta) a porta do host (8080) com a porta do container (80)
            --net=bridge: usa a docker0 para se comunicar com a internet
            --net=host: usa a máq host para se comunicar com a internet



Exemplo para docker
    $ docker run -ti --name container1 debian
        -> Ctrl + PQ (para sair do console sem matar o container)
    $ docker run -ti --name container2 --link container1 debian
        -> entrar no container2
        $ ping container1
        $ cat /etc/resolv.conf
        $ cat /etc/host

para verificar o netfilter

$ iptables -L -n

para veriricar o redirecionamento de IPs e portas

$ iptables -t nat -L -n
    -> muito legal isso!!!!

Docker Machine

    criar um tipo de host docker nos variados drivers, como Google, AWS, VirtualBox...


Docker compose: maneira simples de containerzar o ambiente

Instalando

    $ wget https://github.com/docker/compose/releases/download/1.6.2/docker-compose-Linux-x86_64
    $ mv docker-compose-Linux-x86_64 /usr/local/bin/docker-compose
    $ chmod +x /usr/local/bin/docker-compose

Verificando se foi instalado e versão

    $ docker-compose --version


---
###### [Voltar ao topo](#index)
---








<!-- KAFKA -->

# kafka

<p align="center">
  <img src="https://miro.medium.com/max/1200/1*8GbrXbHdH5uPGMb5epWhrg.png" width="350" title="hover text">
</p>


iniciar servidor do zookeeper

    $ zookeeper-server-start.sh config/zookeeper.properties

iniciar servidor do KAFKA

    $ kafka-server-start.sh config/server.properties

listar os tópicaos criados

    $ kafka-topics.sh --bootstrap-server localhost:9092 --list

criar um tópico

    $ kafka-topics.sh --bootstrap-server localhost:9092 --topic topico1 --create --partitions 3 --replication-factor 1

detalhes de um tópico

    $ kafka-topics.sh --bootstrap-server localhost:9092 --topic topico1 --describe

deletar um tópico

    $ kafka-topics.sh --bootstrap-server localhost:9092 --topic topico1 --delete

produzindo mensagens em um tópico

    $ kafka-console-producer.sh --broker-list localhost:9092 --topic topico1 --producer-property acks=all
    * isso vai abrir o console para digitar a mensagem

consumindo mensagens de um tópico

    $ kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic topico1 --group grupo1 --from-beginning
        * se não for    informado 'from-beginning' (opcional), então apenas cosumirá as mensagens que foram produzidas a partir deste momento
        * --group (opcional) - para vários consumers de um mesmo producer
    $ kafka-console-producer.sh --broker-list localhost:9092 --topic topico3 --property parse.key=true --property key.separator=:
        * informa uma chave para o kafka direcionar sempre para o mesmo consumer

descrever as informações de um grupo de consumers

    $ kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group grupo1

resetar um consumer para um tópico

    $ kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group grupo1 --reset-offsets --to-earliest --execute --topic topico1
    $ kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group grupo1 --reset-offsets --shift-by -2 --execute --topic topico1

### Exemplos em java

#### Producer (with key and callback)
```java
public class ProducerDemoWithKeys {

    public static void main(String[] args) throws ExecutionException, InterruptedException {

        final Logger logger = LoggerFactory.getLogger(ProducerDemoWithKeys.class);
        String BOOTSTRAP_SERVER = "localhost:9092";

        // create producer properties
        Properties properties = new Properties();
        properties.setProperty(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, BOOTSTRAP_SERVER);
        properties.setProperty(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());
        properties.setProperty(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());

        // create the producer
        KafkaProducer<String, String> producer = new KafkaProducer<String, String>(properties);

        for (int i = 0; i < 20; i++) {

            String topic = "topico1";
            String value = "minha mensagem " + i;
            final String key = "id_" +i;

            // create a producer record
            ProducerRecord<String, String> record = new ProducerRecord<String, String>(topic, key, value);

            Thread.sleep(100);

            // send data - asynchronous
            producer.send(record, new Callback() {
                public void onCompletion(RecordMetadata recordMetadata, Exception e) {
                    if (null == e) {
                        logger.info("Received new metadata: " + "Topic: " + recordMetadata.topic() +
                                "; Particition: " + recordMetadata.partition() + "; Offset: " + recordMetadata.offset() +
                                "; Timestamp: " + recordMetadata.timestamp() + "; Key: " + key
                        );
                    } else {
                        logger.error("Error while producing", e);
                    }
                }
            }).get();
        }

        // just flush
        producer.flush();

        // flush and close
        producer.close();
    }
}
```


#### Consumer
```java
public class ConsumerDemo {

    public static void main(String[] args) {

        Logger logger = LoggerFactory.getLogger(ConsumerDemo.class);

        String BOOTSTRAP_SERVER = "localhost:9092";
        String groupId = "grupo1";
        String topico = "topico1";

        // create consumer properties
        Properties properties = new Properties();
        properties.setProperty(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, BOOTSTRAP_SERVER);
        properties.setProperty(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
        properties.setProperty(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
        properties.setProperty(ConsumerConfig.GROUP_ID_CONFIG, groupId);
        properties.setProperty(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, "earliest");

        // create consummer
        KafkaConsumer<String, String> consumer = new KafkaConsumer<String, String>(properties);

        // subscribe consumer to our topic(s)
        consumer.subscribe(Collections.singleton(topico));

        // poll for new data
        while (true) {
            ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(50));

            for (ConsumerRecord<String, String> record : records) {
                logger.info("Key: " + record.key() + "; Value: " + record.value() +
                        "; Partition: " + record.partition() + "; Offset: " + record.offset());
            }
        }
    }
}
```


---
###### [Voltar ao início do documento](#index) | [Voltar ao início **kafka**](#kafka)
---








<!-- REDIS -->

## redis

<p align="center">
  <img src="https://cdn-images-1.medium.com/fit/t/1600/480/1*RH-nQLRMejTFFL3zunTCAg.png" width="350" title="hover text">
</p>



1. [docker para redis](#docker-para-redis)
2. [resumo do tutorial try.redis](#resumo-do-tutorial-tryredis)
3. [trabalhando com listas](#trabalhando-com-listas)
4. [trabalhando com sets](#trabalhando-com-sets)
5. [trabalhando com sorted set](#trabalhando-com-sorted-sets)
6. [trabalhando com hashes](#trabalhando-com-hashes)

***[redis.io](https://redis.io/)***


## docker para redis

    > docker run --name server-redis -d redis
    > docker run -it --link server-redis:redis --rm redis redis-cli -h redis -p 6379
    > ping
        * isso deve retornar "PONG"

ou

```
version: '2'

services:
  redis:
    image: 'docker.io/bitnami/redis:6.0-debian-10'
    environment:
      # ALLOW_EMPTY_PASSWORD is recommended only for development.
      - ALLOW_EMPTY_PASSWORD=yes
      - REDIS_DISABLE_COMMANDS=FLUSHDB,FLUSHALL
    ports:
      - '6379:6379'
    volumes:
      - 'redis_data:/bitnami/redis/data'

volumes:
  redis_data:
    driver: local


----> docker-compose -f docker-compose-cipri.yml exec redis redis-cli
```

---

## resumo do tutorial [try.redis](http://try.redis.io/)

As a first example, we can use the command SET to store the value "fido" at key "server:name":

    $ SET server:name "fido"

Redis will store our data permanently, so we can later ask "What is the value stored at key server:name?" and Redis will reply with "fido":

    > GET server:name => "fido"

There is a command in order to test if a give key exists or not:

    > EXISTS server:name => 1
    > EXISTS server:blabla => 0
Tip: You can click the commands above to automatically execute them. The text after the arrow (=>) shows the expected output.

Other basic operations provided by Redis are DEL to delete a given key and associated value, INCR to atomically increment a number stored at a given key:

    > SET connections 10
    > INCR connections => 11
    > INCR connections => 12
    > DEL connections
    > INCR connections => 1

It is also possible to increment the number contained inside a key by a specific amount:

    > INCRBY connections 100 => 101

And there are similar commands in order to decrement the value of the key.

    > DECR connections => 100
    > DECRBY connections 10 => 90

When you manipulate Redis strings with incrementing and decrementing commands, you are implementing counters. Counters are a very popular application for Redis.

There is something special about INCR. Why do we provide such an operation if we can do it ourself with a bit of code? After all it is as simple as:

    > x = GET count
    > x = x + 1
    > SET count x

The problem is that doing the increment in this way will only work as long as there is a single client using the key. See what happens if two clients are accessing this key at the same time:

    Client A reads count as 10.
    Client B reads count as 10.
    Client A increments 10 and sets count to 11.
    Client B increments 10 and sets count to 11.

We wanted the value to be 12, but instead it is 11! This is because incrementing the value in this way is not an atomic operation. Calling the INCR command in Redis will prevent this from happening, because it is an atomic operation.

All the Redis operations implemented by single commands are atomic, including the ones operating on more complex data structures. So when you use a Redis command that modifies some value, you don't have to think about concurrent access.

Redis can be told that a key should only exist for a certain length of time. This is accomplished with the EXPIRE and TTL commands, and by the similar PEXPIRE and PTTL commands that operate using time in milliseconds instead of seconds.

    > SET resource:lock "Redis Demo"
    > EXPIRE resource:lock 120

This causes the key resource:lock to be deleted in 120 seconds. You can test how long a key will exist with the TTL command. It returns the number of seconds until it will be deleted.

    > TTL resource:lock => 113
    // after 113s
    > TTL resource:lock => -2

The -2 for the TTL of the key means that the key does not exist (anymore). A -1 for the TTL of the key means that it will never expire. Note that if you SET a key, its TTL will be reset.

    > SET resource:lock "Redis Demo 1"
    > EXPIRE resource:lock 120
    > TTL resource:lock => 119
    > SET resource:lock "Redis Demo 2"
    > TTL resource:lock => -1

The SET command is actually able to accept further arguments in order to directly set a time to live (TTL) to a key, so you can alter the value of a key and set its TTL at the same time in a single atomic operation:

    > SET resource:lock "Redis Demo 3" EX 5
    > TTL resource:lock => 5

It is also possible to cancel the time to live of a key removing the expire and making the key permanent again.

    > SET resource:lock "Redis Demo 3" EX 5
    > PERSIST resource:lock
    > TTL resource:lock => -1

## trabalhando com listas

Redis also supports several more complex data structures. The first one we'll look at is a list. A list is a series of ordered values. Some of the important commands for interacting with lists are RPUSH, LPUSH, LLEN, LRANGE, LPOP, and RPOP. You can immediately begin working with a key as a list, as long as it doesn't already exist as a different type.

This concept is generally true for every Redis data structure: you don't create a key first, and add things to it later, but you can just directly use the command in order to add new elements. As side effect the key will be create if it did not exist. Similarly keys that will result empty after executing some command will automatically be removed from the key space.

RPUSH puts the new element at the end of the list.

    > RPUSH friends "Alice"
    > RPUSH friends "Bob"
LPUSH puts the new element at the start of the list.

    > LPUSH friends "Sam"

LRANGE gives a subset of the list. It takes the index of the first element you want to retrieve as its first parameter and the index of the last element you want to retrieve as its second parameter. A value of -1 for the second parameter means to retrieve elements until the end of the list, -2 means to include up to the penultimate, and so forth.

    > LRANGE friends 0 -1 => 1) "Sam", 2) "Alice", 3) "Bob"
    > LRANGE friends 0 1 => 1) "Sam", 2) "Alice"
    > LRANGE friends 1 2 => 1) "Alice", 2) "Bob"


So far we explored the commands that let you add elements to the list, and LRANGE that let you inspect ranges of the list. A fundamental functionality of Redis lists is the ability to remove, and return to the client at the same time, elements in the head or the tail of the list.

LPOP removes the first element from the list and returns it.

    > LPOP friends => "Sam"

RPOP removes the last element from the list and returns it.

    > RPOP friends => "3"

Note that the list now only has four element:

    LLEN friends => 4
    LRANGE friends 0 -1 => 1) "Alice" 2) "Bob" 3) "1" 4) "2"

Both RPUSH and LPUSH commands are variadic, so you can specify multiple elements in the same command execution.

    > RPUSH friends 1 2 3 => 6

Tip: RPUSH and LPUSH return the total length of the list after the operation.

You can also use LLEN to obtain the current length of the list.

    > LLEN friends => 6


## trabalhando com sets

The next data structure that we'll look at is a set. A set is similar to a list, except it does not have a specific order and each element may only appear once. Both the data structures are very useful because while in a list is fast to access the elements near the top or the bottom, and the order of the elements is preserved, in a set is very fast to test for membership, that is, to immediately know if a given element was added or not. Moreover in a set a given element can exist only in a single copy.

Some of the important commands in working with sets are SADD, SREM, SISMEMBER, SMEMBERS and SUNION.

SADD adds the given member to the set, again this command is also variadic.

    > SADD superpowers "flight"
    > SADD superpowers "x-ray vision" "reflexes"

SREM removes the given member from the set, returning 1 or 0 to signal if the member was actually there or not.

    > SREM superpowers "reflexes" => 1
    > SREM superpowers "making pizza" => 0

SISMEMBER tests if the given value is in the set. It returns 1 if the value is there and 0 if it is not.

    > SISMEMBER superpowers "flight" => 1
    > SISMEMBER superpowers "reflexes" => 0

SMEMBERS returns a list of all the members of this set.

    > SMEMBERS superpowers => 1) "flight", 2) "x-ray vision"

SUNION combines two or more sets and returns the list of all elements.

    > SADD birdpowers "pecking"
    > SADD birdpowers "flight"
    > SUNION superpowers birdpowers => 1) "pecking", 2) "x-ray vision", 3) "flight"

The return value of SADD is as important as the one of SREM. If the element we try to add is already inside, then 0 is returned, otherwise SADD returns 1:

    > SADD superpowers "flight" => 0
    > SADD superpowers "invisibility" => 1

Sets also have a command very similar to LPOP and RPOP in order to extract elements from the set and return them to the client in a single operation. However since sets are not ordered data structures the returned (and removed) elements are totally casual in this case.

    > SADD letters a b c d e f => 6
    > SPOP letters 2 => 1) "c" 2) "a"
The argument of SPOP after the name of the key, is the number of elements we want it to return, and remove from the set.

Now the set will have just the remaining elements:

    > SMEMBERS letters => 1) "b" 2) "d" 3) "e" 4) "f"
There is also a command to return random elements without removing such elements from the set, it is called SRANDMEMBER. You can try it yourself, it works just like SPOP, but if you specify a negative count instead of a positive one, it may also return repeating elements.

#### trabalhando com sorted set

Sets are a very handy data type, but as they are unsorted they don't work well for a number of problems. This is why Redis 1.2 introduced Sorted Sets.

A sorted set is similar to a regular set, but now each value has an associated score. This score is used to sort the elements in the set.

    > ZADD hackers 1940 "Alan Kay"
    > ZADD hackers 1906 "Grace Hopper"
    > ZADD hackers 1953 "Richard Stallman"
    > ZADD hackers 1965 "Yukihiro Matsumoto"
    > ZADD hackers 1916 "Claude Shannon"
    > ZADD hackers 1969 "Linus Torvalds"
    > ZADD hackers 1957 "Sophie Wilson"
    > ZADD hackers 1912 "Alan Turing"
In these examples, the scores are years of birth and the values are the names of famous hackers.

    > ZRANGE hackers 2 4 => 1) "Claude Shannon", 2) "Alan Kay", 3) "Richard Stallman"

#### trabalhando com hashes

Simple strings, sets and sorted sets already get a lot done but there is one more data type Redis can handle: Hashes.

Hashes are maps between string fields and string values, so they are the perfect data type to represent objects (eg: A User with a number of fields like name, surname, age, and so forth):

    > HSET user:1000 name "John Smith"
    > HSET user:1000 email "john.smith@example.com"
    > HSET user:1000 password "s3cret"
To get back the saved data use HGETALL:

    > HGETALL user:1000

You can also set multiple fields at once:

    > HMSET user:1001 name "Mary Jones" password "hidden" email "mjones@example.com"

If you only need a single field value that is possible as well:

    > HGET user:1001 name => "Mary Jones"

Numerical values in hash fields are handled exactly the same as in simple strings and there are operations to increment this value in an atomic way.

    > HSET user:1000 visits 10
    > HINCRBY user:1000 visits 1 => 11
    > HINCRBY user:1000 visits 10 => 21
    > HDEL user:1000 visits
    > HINCRBY user:1000 visits 1 => 1

Check the full list of Hash commands [http://redis.io/commands#hash] for more information.


#### Stream
-> Font: [hackernoon.com](https://hackernoon.com/introduction-to-redis-streams-133f1c375cd3)

##### writing
XADD allows us to write a stream of events. Let’s create a stream of events reflecting the example above.
We will name our stream ‘events’. The ‘*’ is used to separate optional parameters from the set of key values. This will write all these actions onto the ‘events’ steam.

    > XADD stream_name * key1 value1 key2 value2 (...)
    > XADD events * user tim action purchase item travel:fiji
    > XADD events * user tim action preferences subscription daily
    > XADD events * user sam action login platform iPhone

##### reading
We are requesting items in the stream ‘events’, starting at the beginning of the list (by passing ‘0’). We requested only 2 items.

    > XREAD COUNT 2 STREAMS events item-id
    > XREAD COUNT 2 STREAMS events 0


#### Get all keys
-> Font: [How to Get All Keys in Redis](https://chartio.com/resources/tutorials/how-to-get-all-keys-in-redis/)

##### Retrieving All Existing Keys
As it turns out, every SET command we issued above created a new, unique key within our Redis database. To get a list of all current keys that exist, simply use the KEYS command:

    > KEYS *
    > KEYS *mykey*
        - all keys that contain the text "mykey"



---
###### [Voltar ao início do documento](#index) | [Voltar ao início **redis**](#redis)
---









<!-- GIT -->

## git

<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/e0/Git-logo.svg/1200px-Git-logo.svg.png" width="350" title="hover text">
</p>


[Cheats](https://gist.github.com/chrismccoy/8775224)


**_Ritual sagrado_**

    $ git fetch -p origin
    $ git pull
    $ git add .
    $ git commit -m "minha mensagem de commit"
    $ git push origin


Guarda o estado atual da branch

    $ git stash save -u


Resetar user (credential senha password)

    $ git config credential.helper "" && git config --global credential.helper store


Checkout

    $ git checkout -b <remote-branch>


Apagar arquivos não trackeados (desadicionar)

    $ git clean -f -d


Log de todas as branches (exemplo)

    $ git log --pretty=format:"%ad:%an:%d:%B" --date=short --reverse --all --since=2.months.ago --author=Petr


Alterar url local

    $ git config --get remote.origin.url
    $ git remote set-url origin https://{new url with username replaced}


Commitado sem push (ou seja, em Stage)
    $ git log/diff origin/<minha_branch>..HEAD



Configurações iniciais

    $ git config --global user.name "Full Name"
    $ git config --global user.email "email@address.com"
    $ git config --global http.sslVerify false
    $ git config --global credential.helper store


Log

    $ git log --all --graph --decorate --oneline


Apagar arquivos não trackeados (desadicionar)

    $ git clean -f -d


Mostrar branch atual inline, inserir o conteúdo abaixo no arquivo .bashrc

    # Show current git branch in command line
    parse_git_branch() {
        git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
    }
    export PS1="\[\033[32m\]\w\[\033[33m\]\$(parse_git_branch)\[\033[00m\] $ "




---
###### [Voltar ao início do documento](#index) | [Voltar ao início **git**](#git)
---