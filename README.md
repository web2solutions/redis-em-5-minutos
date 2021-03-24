<img align="center" src="https://i.imgur.com/cJjTcYk.png" />

# Redis em 5 minutos

# Index:

- **O que é o Redis?**
- **Tipos de dados**
  1. `Sequência de caracteres`
  2. `Mapa / Dicionário`
  3. `Lista`
  4. `Grupo`
  5. `Grupo Ordenado`
  6. `Index Geo Espacial`
  7. `Mapa de bits`
  8. `HyperLogLog`
  9. `Streams`
- **Manipulando dados**
  1. `Sequência de caracteres`
  2. `Mapa / Dicionário`
  3. `Lista`
  4. `Grupo`
  5. `Grupo Ordenado`
  6. `Index Geo Espacial`
  7. `Mapa de bits`
  8. `HyperLogLog`
  9. `Streams`

## O que é o Redis?

O redis é um `armazenador ->em memória<- de estrutura de dados`, que utiliza o mecanismo do tipo `chave/valor` (`key/value`) para acesso á dados. Isto significa que para armazenar qualquer `valor`, uma `chave de acesso` é atribuída á aquele valor. Para se acessar um `valor` é necessário conhecer sua `chave`.

O termo: `estrutura de dados` refere-se ao fato dos `valores` das `chaves` armazenadas poderem conter `estrutura de dados complexas`.

Comumente, Redis é utilizado para implementar `cache` enquanto o banco de dados principal é um RDBMS ou um NoSQL (baseado em disco).

O que faz o Redis ser diferente de outras implementações de cache é sua velocidade e capacidade de persistir dados no disco.

Além disso, Redis pode ser usado como `banco de dados` e `Corretor/Mediador de Mensagens`. 

Cenários onde o Redis armazena dados que não são armazenados em nenhum outro lugar, fazem do Redis um banco de dados.

Como `Mediador de Mensagens`, o Redis implementa o padrão `Publicar/Assinar` (`Publish/Subscribe`) paralelamente á estrutura de dados (particularmente as Listas)

## Tipos de `estruturas de dados` que o Redis oferece:

### String

Sequencia de caracteres. É a estrutura de dados mais `simples` do Redis.

Operações atômicas.


### Hash 

Conhecido como Mapa, Dicionário ou vetor associativo.

Acesso e manipulação rápida de dados.

Basicamente é uma coleção de `campo/valor`

É a estrutura de dados mais flexível do Redis. O redis funciona exatamente como um hash.

Exemplo de Uso: Session, Perfil de Usuário, Documentos JSON


### List

Lista Ordenada de Strings com índices numérico.

Tempo: O(1) para adicionar ou remover itens mesmo com milhões de items

Exemplo de Uso: Filas. Facilita a implementação do padrão `assíncrono` `produtor-consumidor`.


### Set (Grupo)

Grupo desordenado de Strings únicas.
Oferece comandos para `Diferença`, `Interseção` e `União`
São mais perfomáticos do que `hash` para solucionar problemas de `filiação`, por exemplo: `Esse item está presente nesse conjunto de dados?`

Exemplo de Uso: `Interesses` de um consumidor, `Grupo de Amigos` de um usuário.



### Sorted Set with range queries

Grupo ordenado por `classificação` de Strings únicas. Ao armazenar um valor, é associado a sua pontuaçào que é um valor.

`Alta performance` em `ordenamento`

Bom para buscar dados ordenados via classificação numérica e de forma simples.

Exemplo de Uso: Coleção de dados do tipo `série temporal`, lances de um leilão, compras X valor de compra, itens mais visualizados, quadro de classificação de um game.


### Geospatial index

É uma `forma especial` da implementação de `Grupo Ordenado`, onde a classificação númerica do item é uma coordenada geográfica.

Muito rápido para implementação de buscas dentro de uma área específica.



### Bitfield - Bitmap

`Vetores` de `Bit` são usados para `contar` informações, mas limitando o consumo de memória.

Ideal para casos onde é necessário lidar com grande volume de dados e consumir memória de forma eficiente.


### HyperLogLog

É uma estrutura de dados usada para estimar/contar itens únicos. Foco em eficiência de memória e processamento.


### Stream

Fluxo

Implementa uma estrutura de dados do tipo fluxo de log. Primariamente funciona no modo `acrescentar informações no final` somente.

É a estrutura de dados mais complex do Redis

Permite um grupo de clientes cooperar entre si, consumindo uma porção diferente do mesmo stream de mensagems.

Não é muito diferente de uma `Lista`, exceto pelo fato de possuir uma API `blocking` mais completa.

É composto por `um` ou mais `pares` de `campo-valor`

Os fluxos do Redis oferecem suporte a consultas de intervalo por ID. 




## Comandos

**Iniciar Redis informando um arquivo de configuração**

```bash
  redis-server /path/redis.conf
```

**Iniciar o prompt do Redis**

```bash
  redis-cli
```


## Manipulando Strings.


```bash
  # cria uma chave com um valor associado
  > SET chave valor
  
  # Tempo: O(1)
```

```bash
  # cria uma chave com um valor associado SE a chave não existir
  > SETNX chave valor
  
  # Tempo: O(1)
```

```bash
  # recupera o valor de uma chave
  > GET chave
  
  # Tempo: O(1)
```

```bash
  # associa um novo valor á chave especificada e retorna o valor antigo da chave
  > GETSET chave valor

  # Tempo: O(1)
```

```bash
  # deleta todas as chaves dadas
  > DEL chave1 chave2
  
  # Tempo: O(1)
```


```bash
  # incrementa em mais 1 o valor de uma chave to tipo contador
  > INCR chave
  
  # Tempo: O(1)
```

```bash
  # incrementa em mais X o valor de uma chave to tipo contador, onde X é um número inteiro
  > INCRBY chave numero
  
  # Tempo: O(1)
```

```bash
  # incrementa em mais X o valor de uma chave to tipo contador, onde X é um número float
  > INCRBYFLOAT chave numero
  
  # Tempo: 
```

```bash
  # Decrementa em menos 1 o valor de uma chave to tipo contador
  > DECR chave

  # Tempo: O(1)
```

```bash
  # incrementa em menos X o valor de uma chave to tipo contador, onde X é um número
  > DECRBY chave numero

  # Tempo: O(1)
```


```bash
  # Configura uma chave pra expirar em X segundos, onde X é  um número
  > EXPIRE chave segundos
  
  # Tempo: O(1)
```

```bash
  # Recupera o tempo de vida de uma  chave
  > TTL chave
  
  # Tempo: O(1)
```

```bash
  # Adiciona um valor ao final do valor de uma chave
  > APPEND chave value
  
  # Tempo: 
```



```bash
  # recupera o comprimento de um valor associado á chave dada.
  > STRLEN chave
  
  # Tempo: 
```

```bash
  # cria múltiplas chaves com um valor associado
  > MSET chave1 valor1 chave2 valor2
  
  # Tempo: O(1) para setar cada chave
```

```bash
  # cria múltiplas chaves com um valor associado, somente se nenhuma das chaves existirem
  > MSETNX chave1 valor1 chave2 valor2
  
  # Tempo: O(1) para setar cada chave
```

```bash
  # recupera o valor de múltiplas chaves
  > MGET chave1 chave2
  
  # Tempo: 
```

```bash
  # overwrite part of a string at key starting at the specified offset
  > SETRANGE key offset value         
  
  # Tempo: 
```

```bash
  # get a substring value of a key and return its old value
  > GETRANGE key value                
  
  # Tempo: 
```

```bash
  # count set bits in a string
  > BITCOUNT key [start end]          
  
  # Tempo: 
```




## Manipulando Lists.



```bash
  # put the new value at the end of the list
  > RPUSH key value [value ...]           
  
  # Tempo: 
```

```bash
  # append a value to a list, only if the exists
  > RPUSHX key value                      
  
  # Tempo: 
```

```bash
  # put the new value at the start of the list
  > LPUSH key value [value ...]           
  
  # Tempo: 
```

```bash
  # give a subset of the list
  > LRANGE key start stop                 
  
  # Tempo: 
```

```bash
  # get an element from a list by its index
  > LINDEX key index                      
  
  # Tempo: 
```

```bash
  # insert an element before or after another element in a list
  > LINSERT key BEFORE|AFTER pivot value  
  
  # Tempo: 
```

```bash
  # return the current length of the list
  > LLEN key                              
  
  # Tempo: 
```

```bash
  # remove the first element from the list and returns it
  > LPOP key                              
  
  # Tempo: 
```

```bash
  # set the value of an element in a list by its index
  > LSET key index value                  
  
  # Tempo: 
```

```bash
  # trim a list to the specified range
  > LTRIM key start stop                  
  
  # Tempo: 
```

```bash
  # remove the last element from the list and returns it
  > RPOP key                              
  
  # Tempo: 
```

```bash
  # remove the last element in a list, prepend it to another list and return it
  > RPOPLPUSH source destination          
  
  # Tempo: 
```

```bash
  # remove and get the first element in a list, or block until one is available
  > BLPOP key [key ...] timeout           
  
  # Tempo: 
```

```bash
  # remove and get the last element in a list, or block until one is available
  > BRPOP key [key ...] timeout           
  
  # Tempo: 
```


## Manipulando Sets.


SADD key member [member ...]     # add the given value to the set
SCARD key                        # get the number of members in a set
SREM key member [member ...]     # remove the given value from the set
SISMEMBER myset value            # test if the given value is in the set.
SMEMBERS myset                   # return a list of all the members of this set
SUNION key [key ...]             # combine two or more sets and returns the list of all elements
SINTER key [key ...]             # intersect multiple sets
SMOVE source destination member  # move a member from one set to another
SPOP key [count]                 # remove and return one or multiple random members from a set


## Manipulando Sorted Sets


ZADD key [NX|XX] [CH] [INCR] score member [score member ...]  # add one or more members to a sorted set, or update its score if it already exists

ZCARD key                           # get the number of members in a sorted set
ZCOUNT key min max                  # count the members in a sorted set with scores within the given values
ZINCRBY key increment member        # increment the score of a member in a sorted set
ZRANGE key start stop [WITHSCORES]  # returns a subset of the sorted set
ZRANK key member                    # determine the index of a member in a sorted set
ZREM key member [member ...]        # remove one or more members from a sorted set
ZREMRANGEBYRANK key start stop      # remove all members in a sorted set within the given indexes
ZREMRANGEBYSCORE key min max        # remove all members in a sorted set, by index, with scores ordered from high to low
ZSCORE key member                   # get the score associated with the given mmeber in a sorted set

ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]  # return a range of members in a sorted set, by score


## Manipulando Hashes



HGET key field          # get the value of a hash field
HGETALL key             # get all the fields and values in a hash
HSET key field value    # set the string value of a hash field
HSETNX key field value  # set the string value of a hash field, only if the field does not exists

HMSET key field value [field value ...]  # set multiple fields at once

HINCRBY key field increment  # increment value in hash by X
HDEL key field [field ...]   # delete one or more hash fields
HEXISTS key field            # determine if a hash field exists
HKEYS key                    # get all the fields in a hash
HLEN key                     # get all the fields in a hash
HSTRLEN key field            # get the length of the value of a hash field
HVALS key                    # get all the values in a hash


## Manipulando HyperLogLog


PFADD key element [element ...]  # add the specified elements to the specified HyperLogLog
PFCOUNT key [key ...]            # return the approximated cardinality of the set(s) observed by the HyperLogLog at key's)

PFMERGE destkey sourcekey [sourcekey ...]  # merge N HyperLogLogs into a single one


## Manipulando Publicação / Assinatura


PSUBSCRIBE pattern [pattern ...]             # listen for messages published to channels matching the given patterns
PUBSUB subcommand [argument [argument ...]]  # inspect the state of the Pub/Sub subsystem
PUBLISH channel message                      # post a message to a channel
PUNSUBSCRIBE [pattern [pattern ...]]         # stop listening for messages posted to channels matching the given patterns
SUBSCRIBE channel [channel ...]              # listen for messages published to the given channels
UNSUBSCRIBE [channel [channel ...]]          # stop listening for messages posted to the given channels


## Manipulando Streams

```bash
  # * auto generated id
  > XADD meustream * sensor-id 1234 temperatura 19.8
  # returns stream ID 1518951480106-0
```


```bash
  > XLEN meustream 
  # (integer) 1
```

## Outros comandos

```bash
  # RECUPERA todas as chaves dado o padrão glob fornecido.
  KEYS glob
  
  # Exemplos:
  # h?llo retornará hello hallo hhllo
  # h*llo retornará hllo heeeello
  # h[ae]llo retornará hello e hallo, mas não hillo

  # Tempo: O(n)
```



