<img align="center" src="https://i.imgur.com/cJjTcYk.png" />

# Redis em 5 minutos

Conteúdo em português com o objetivo de mitigar o uso do Redis de forma simples, objetiva e `quase` completa.

## Índice de navegação:

- [**O que é o Redis?**](#oquee)
- [**Tipos de dados**](#tipos)
  1. [`Sequência de caracteres`](#string)
  2. [`Mapa / Dicionário`](#hash)
  3. [`Lista`](#list)
  4. [`Conjunto`](#set)
  5. [`Conjunto Ordenado`](#sortedset)
  6. [`Index Geo Espacial`](#geo)
  7. [`Mapa de bits`](#bitmap)
  8. [`HyperLogLog`](#hyper)
  9. [`Streams`](#stream)
- [**Manipulando dados**]()
  1. [`Sequência de caracteres`](#mstring)
  2. [`Mapa / Dicionário`](#mhash)
  3. [`Lista`](#mlist)
  4. [`Conjunto`](#mset)
  5. [`Conjunto Ordenado`](#msortedset)
  6. [`Index Geo Espacial`](#mgeo)
  7. [`Mapa de bits`](#mbitmap)
  8. [`HyperLogLog`](#mhyper)
  9. [`Streams`](#mstream)


--------------------------

## <a name="oquee"></a> O que é o Redis?

> Cache? Banco de Dados? Mediador de Mensagens?

O redis é um `armazenador` **em memória** `de estrutura de dados`, que utiliza o mecanismo do tipo `chave/valor` (`key/value`) para acesso á dados. Isto significa que para armazenar qualquer `valor`, uma `chave de acesso` é atribuída á aquele valor. Para se acessar um `valor` é necessário conhecer sua `chave`.

O termo: `estrutura de dados` refere-se ao fato dos `valores` das `chaves` armazenadas poderem conter `estrutura de dados complexas`.

Comumente, Redis é utilizado para implementar `cache` enquanto o banco de dados principal é um RDBMS ou um NoSQL (baseado em disco).

O que faz o Redis ser diferente de outras implementações de cache é sua velocidade e capacidade de persistir dados no disco.

Além disso, Redis pode ser usado como `banco de dados` e `Corretor/Mediador de Mensagens`. 

Cenários onde o Redis armazena dados que não são armazenados em nenhum outro lugar, fazem do Redis um banco de dados.

Como `Mediador de Mensagens`, o Redis implementa o padrão `Publicar/Assinar` (`Publish/Subscribe`) paralelamente á estrutura de dados (particularmente as Listas)

--------------------------

## <a name="tipos"></a> Tipos de `estruturas de dados` que o Redis oferece:

### <a name="string"></a> String

Sequência de caracteres. É a estrutura de dados mais `simples` do Redis.

Operações atômicas.


### <a name="hash"></a> Hash 

Conhecido como Mapa, Dicionário ou vetor associativo.

Acesso e manipulação rápida de dados.

Basicamente é uma coleção de `campo/valor`

É a estrutura de dados mais flexível do Redis. O redis funciona exatamente como um hash.

Exemplo de Uso: Session, Perfil de Usuário, Documentos JSON


### <a name="list"></a> List

Lista Ordenada de Strings com índices numérico.

Tempo: O(1) para adicionar ou remover itens mesmo com milhões de items

Exemplo de Uso: Filas. Facilita a implementação do padrão `assíncrono` `produtor-consumidor`.


### <a name="set"></a> Set (Conjunto)

Conjunto desordenado de Strings únicas.
Oferece comandos para `Diferença`, `Interseção` e `União`
São mais perfomáticos do que `hash` para solucionar problemas de `filiação`, por exemplo: `Esse item está presente nesse conjunto de dados?`

Exemplo de Uso: `Interesses` de um consumidor, `Conjunto de Amigos` de um usuário.



### <a name="sortedset"></a> Sorted Set with range queries

Conjunto ordenado por `classificação` de Strings únicas. Ao armazenar um valor, é associado a sua pontuaçào que é um valor.

`Alta performance` em `ordenamento`

Bom para buscar dados ordenados via classificação numérica e de forma simples.

Exemplo de Uso: Coleção de dados do tipo `série temporal`, lances de um leilão, compras X valor de compra, itens mais visualizados, quadro de classificação de um game.


### <a name="geo"></a> Geospatial index

É uma `forma especial` da implementação de `Conjunto Ordenado`, onde a classificação númerica do item é uma coordenada geográfica.

Muito rápido para implementação de buscas dentro de uma área específica.



### <a name="bitmap"></a> Bitfield - Bitmap

`Vetores` de `Bit` são usados para `contar` informações, mas limitando o consumo de memória.

Ideal para casos onde é necessário lidar com grande volume de dados e consumir memória de forma eficiente.


### <a name="hyper"></a> HyperLogLog

É uma estrutura de dados usada para estimar/contar itens únicos. Foco em eficiência de memória e processamento.


### <a name="stream"></a> Stream

Fluxo de dados

Implementa uma estrutura de dados do tipo fluxo de log. Primariamente funciona no modo `acrescentar informações no final` somente.

É a estrutura de dados mais complex do Redis

Permite um grupo de clientes cooperar entre si, consumindo uma porção diferente do mesmo stream de mensagems.

Não é muito diferente de uma `Lista`, exceto pelo fato de possuir uma API `blocking` mais completa.

É composto por `um` ou mais `pares` de `campo-valor`

Os fluxos de dados do Redis oferecem suporte a consultas de intervalo por ID. 


--------------------------

## Comandos

**Iniciar Redis informando um arquivo de configuração**

```bash
  redis-server /path/redis.conf
```

**Iniciar o prompt do Redis**

```bash
  redis-cli
```
--------------------------

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
  
  # Tempo: O(1) para cada chave
```

```bash
  # substituir parte da string armazenada na chave na posição especificada
  # SET chave "Hello World"
  > SETRANGE chave 6 "Redis"
  # "Hello Redis"
  
  # Tempo: O(1)
```

```bash
  # recupera uma substring do valor armazenado na chave, determinada pelas posições 
  # inicio e fim (ambos inclusivos). 
  # -1 no fim significa até o último char
  # -2 no fim significa até o penúltimo char
  > GETRANGE chave inicio fim
  # Ex: 
  # SET chave "Isto é uma substring"
  # GETRANGE chave 0 3
  # "Isto"
  
  # Tempo: O(n)
```

```bash
  # Conta o número de conjunto de bits em uma string.
  > BITCOUNT key [start end]          
  
  # Tempo: O(n)
```


--------------------------

## Manipulando Contadores.



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

--------------------------


## Manipulando Lists.

Listas

Conceito básico, listas se comportam como uma fila, onde o primeiro que entra, é o primeiro que sai (FIFO - `First in, First out`). Diferente de um vetor padrão, a contagem começa da direta pra esquerda.

```bash
  # adiciona um ou mais itens no `começo` da lista fila_atendimento
  > RPUSH fila_atendimento 'Jose Eduardo' 'Maria da Silva' 'Renato Casagrande' 'Daylla Reis'
  
  # Os itens são indexados na mesma ordem dos valores dados.
  # 'Jose Eduardo' 'Maria da Silva' 'Renato Casagrande' 'Daylla Reis'
  # Tempo: O(1) para cada elemento
```

```bash
  # adiciona um ou mais itens no `começo` da lista, SOMENTE SE a chave já existir E 
  # armazenar uma lista
  > RPUSHX fila_atendimento 'Pedro Cabral'           
  # 'Jose Eduardo' 'Maria da Silva' 'Renato Casagrande' 'Daylla Reis' 'Pedro Cabral'
  # Tempo: O(1) para cada elemento
```

```bash
  # adiciona um ou mais itens na `final` da lista
  > LPUSH fila_atendimento 'Joe Biden'
  
  # 'Joe Biden' 'Jose Eduardo' 'Maria da Silva' 'Renato Casagrande' 'Daylla Reis' 'Pedro Cabral'

  # Os itens são indexados inversamente ordenado
  
  # Exemplo:
  > LPUSH fila_atendimento2 'Jose Eduardo' 'Maria da Silva'
  > LRANGE fila_atendimento2 0 1 # 'Maria da Silva' 'Jose Eduardo' 
  
  # Tempo: O(1) para cada elemento
```

```bash
  # recupera sublista compreendida entre o dada a posição do index inicial e final.
  # Caso forneça -1 como último valor, a busca compreenderá até o último item da lista
  # Caso forneça -2 como último valor, a busca compreenderá até o penúltimo item
  # da lista e assim sucessivamente
  > LRANGE fila_atendimento 0 -1
  # "Joe Biden" "Jose Eduardo" "Maria da Silva" "Renato Casagrande" "Daylla Reis" "Pedro Cabral"

  # Tempo: O(n)
```

```bash
  # recupera um elemento de uma lista dado a posição de seu index
  > LINDEX fila_atendimento 1
  # "Jose Eduardo"

  # Tempo: O(n)
```

```bash
  # adiciona um elemento antes ou depois de um elemento da lista
  > LINSERT fila_atendimento BEFORE 'Jose Eduardo' 'Joana Dark'
  # "Joe Biden" "Joana Dark" "Jose Eduardo" "Maria da Silva" "Renato Casagrande" 
  # "Daylla Reis" "Pedro Cabral"
  
  > LINSERT fila_atendimento AFTER 'Jose Eduardo' 'Maria Padilha'
  # "Joe Biden" "Joana Dark" "Jose Eduardo" "Maria Padilha" "Maria da Silva" 
  # "Renato Casagrande" "Daylla Reis" "Pedro Cabral"

  # Tempo: 
  # O(1) {se adicionar na saida da lista}  
  # O(n) {se adicionar na entrada da lista}
```

```bash
  # recupera o tamanho da fila
  > LLEN fila_atendimento                            
  
  # Tempo: O(1)
```

```bash
  # seta o valor de um item da  lista dado o seu index e valor
  > LSET fila_atendimento 2 'Jose Eduardo Almeida'
  # "Joe Biden" "Joana Dark" "Jose Eduardo Almeida" "Maria Padilha" "Maria da Silva" 
  # "Renato Casagrande" "Daylla Reis" "Pedro Cabral"

  # Tempo: O(n) e O(1) se for no inicio ou final da lista
```

```bash
  # corta uma lista de forma que ele compreenda somente os itens dado o alcance especificado
  > LTRIM fila_atendimento 1 4              
  # "Maria Padilha" "Maria da Silva" "Renato Casagrande" "Daylla Reis"

  # Tempo: O(n)
```

```bash
  # atomicamente remove o PRIMEIRO elemento da lista e retorna ele (ÚLTIMO da fila)
  > LPOP fila_atendimento
  
  # Tempo: O(1)
```

```bash
  # remove o ÚLTIMO elemento da lista e retorna ele (PRIMEIRO da fila)
  > RPOP fila_atendimento
  
  # Tempo: O(n)
```

```bash
  # atomicamente remove o ultimo elemento em uma lista (PRIMEIRO da fila), e adiciona no 
  # inicio de uma outra lista (ÚLTIMO da fila)
  > RPOPLPUSH fila_atendimento fila_atendimento_prioritario         
  
  # Tempo: O(1)
```

```bash
  # versào blocking do LPOP. Remove e retorna o primeiro elemento da 
  # lista (ultimo da fila).
  # Se o comando não encontrar nenhum item disponível em nenhuma das listas, 
  # ele bloqueará até alguém executar um LPUSH ou RPUSH em alguma das chaves
  # 0 é o timeout
  > BLPOP fila_atendimento fila_atendimento_prioritario 0       
  
  # Tempo: O(1)
```

```bash
  # versào blocking do  RPOP. Remove e retorna o último elemento de multiplas 
  # listas (primeiro da fila).
  # Se o comando não encontrar nenhum item disponível em nenhuma das listas, 
  # ele bloqueará até alguém executar um LPUSH ou RPUSH em alguma das chaves
  # 0 é o timeout
  > BRPOP fila_atendimento fila_atendimento_prioritario 0
  
  # Tempo: O(1)
```

--------------------------

## Manipulando Sets.

Conjuntos

Conceito básico: como em um conjunto numérico, items não se repetem nunca.

```bash
  # Adiciona um ou mais membros em um conjunto 
  > SADD amigos_do_joao 'Jose' 'Pedro' 'Daylla' 'James'

  > SADD amigos_da_maria 'Lily' 'Luiza' 'Daylla' 'Iza'
  
  # Tempo: O(1) para cada item
```

```bash
  # recupera o número de membros em um conjunto
  > SCARD amigos_do_joao
  # 3
  
  # Tempo: O(1)
```

```bash
  # Remove itens do conjunto
  > SREM amigos_do_joao 'Pedro'
  # 'Jose' 'Daylla' 'James'
  # Tempo: O(n)
```

```bash
  # Checa se o valor dado é membro do conjunto
  > SISMEMBER amigos_do_joao 'Daylla'
  # 1
  # Tempo: O(1)
```

```bash
  # retorna uma lista de todos  os membros do conjunto
  > SMEMBERS amigos_do_joao
  
  # Tempo: O(N) onde N é a cardinalidade do conjunto
```

```bash
  # Faz a união de dois ou mais conjuntos
  > SUNION amigos_do_joao amigos_da_maria
  # 'Jose' 'Pedro' 'Daylla' 'James' 'Lily' 'Luiza' 'Iza'

  # Tempo: O(N)
```

```bash
  # Faz a interseção de dois ou mais conjuntos
  > SINTER amigos_do_joao amigos_da_maria
  # 'Daylla'

  # Tempo: O(N*M) no pior cenário, onde N é a cardinalidade do menor grupo e M é o número de conjuntos
```

```bash
  # Move um item de um conjunto para outro conjunto
  > SMOVE amigos_do_joao amigos_da_maria 'Jose'  # move a member from one set to another
  
  # Tempo: O(1)
```

```bash
  # remove um ou multiplos items do conjunto de forma randômica 
  > SPOP amigos_do_joao 2
  
  # Tempo: O(1)
```

--------------------------

## Manipulando Sorted Sets

Conjuntos ordenados

```bash
  # add one or more members to a sorted set, or update its score if it already exists
    > ZADD key [NX|XX] [CH] [INCR] score member [score member ...]  
  
  # Tempo: O(1)
```
```bash
  # 
    > ZCARD key                           # get the number of members in a sorted set
  
  # Tempo: O(1)
```
```bash
  # 
    > ZCOUNT key min max                  # count the members in a sorted set with scores within the given values
  
  # Tempo: O(1)
```
```bash
  # 
    > ZINCRBY key increment member        # increment the score of a member in a sorted set
  
  # Tempo: O(1)
```
```bash
  # 
    > ZRANGE key start stop [WITHSCORES]  # returns a subset of the sorted set
  
  # Tempo: O(1)
```
```bash
  # 
    > ZRANK key member                    # determine the index of a member in a sorted set
  
  # Tempo: O(1)
```
```bash
  # 
    > ZREM key member [member ...]        # remove one or more members from a sorted set
  
  # Tempo: O(1)
```

```bash
  # 
    > ZREMRANGEBYRANK key start stop      # remove all members in a sorted set within the given indexes
  
  # Tempo: O(1)
```

```bash
  # 
    > ZREMRANGEBYSCORE key min max        # remove all members in a sorted set, by index, with scores ordered from high to low
  
  # Tempo: O(1)
```

```bash
  # 
    > ZSCORE key member                   # get the score associated with the given mmeber in a sorted set
  
  # Tempo: O(1)
```

```bash
  # 
    > ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]  # return a range of members in a sorted set, by score
  
  # Tempo: O(1)
```
--------------------------


## Manipulando Hashes


```bash
  # 
    > HGET key field          # get the value of a hash field
  
  # Tempo: O(1)
```


```bash
  # 
    > HGETALL key             # get all the fields and values in a hash
  
  # Tempo: O(1)
```


```bash
  # 
HSET key field value    # set the string value of a hash field
  
  # Tempo: O(1)
```


```bash
  # 
    > HSETNX key field value  # set the string value of a hash field, only if the field does not exists
  
  # Tempo: O(1)
```


```bash
  # 
    > HMSET key field value [field value ...]  # set multiple fields at once
  
  # Tempo: O(1)
```


```bash
  # 
    > HINCRBY key field increment  # increment value in hash by X
  
  # Tempo: O(1)
```


```bash
  # 
    > HDEL key field [field ...]   # delete one or more hash fields
  
  # Tempo: O(1)
```


```bash
  # 
    > HEXISTS key field            # determine if a hash field exists
  
  # Tempo: O(1)
```


```bash
  # 
    > HKEYS key                    # get all the fields in a hash
  
  # Tempo: O(1)
```


```bash
  # 
    > HLEN key                     # get all the fields in a hash
  
  # Tempo: O(1)
```


```bash
  # 
    > HSTRLEN key field            # get the length of the value of a hash field
  
  # Tempo: O(1)
```


```bash
  # 
    > HVALS key                    # get all the values in a hash
  
  # Tempo: O(1)
```



--------------------------

## Manipulando HyperLogLog

```bash
  # 
    > PFADD key element [element ...]  # add the specified elements to the specified HyperLogLog
  
  # Tempo: O(1)
```

```bash
  # 
    > PFCOUNT key [key ...]            # return the approximated cardinality of the set(s) observed by the HyperLogLog at key's)
  
  # Tempo: O(1)
```

```bash
  # 
    > PFMERGE destkey sourcekey [sourcekey ...]  # merge N HyperLogLogs into a single one
  
  # Tempo: O(1)
```



--------------------------

## Manipulando Publicação / Assinatura

```bash
  # 
    > PSUBSCRIBE pattern [pattern ...]             # listen for messages published to channels matching the given patterns
  
  # Tempo: O(1)
```

```bash
  # 
    > PUBSUB subcommand [argument [argument ...]]  # inspect the state of the Pub/Sub subsystem
  
  # Tempo: O(1)
```

```bash
  # 
    > PUBLISH channel message                      # post a message to a channel
  
  # Tempo: O(1)
```

```bash
  # 
    > PUNSUBSCRIBE [pattern [pattern ...]]         # stop listening for messages posted to channels matching the given patterns
  
  # Tempo: O(1)
```

```bash
  # 
    > SUBSCRIBE channel [channel ...]              # listen for messages published to the given channels
  
  # Tempo: O(1)
```

```bash
  # 
    > UNSUBSCRIBE [channel [channel ...]]          # stop listening for messages posted to the given channels
  
  # Tempo: O(1)
```


--------------------------


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

--------------------------


## Outros comandos

```bash
  # RECUPERA todas as chaves dado o padrão glob fornecido.
  KEYS glob

  keys <padrão # use com cuidado em produção
 	keys prefixo*
 	keys *padrão*
 	keys *sufixo
 	keys [a-c]* # expressoes tipo grep
  
  # Exemplos:
  # h?llo retornará hello hallo hhllo
  # h*llo retornará hllo heeeello
  # h[ae]llo retornará hello e hallo, mas não hillo

  # Tempo: O(n)
```

```bash
  # deleta todas as chaves dadas
  > DEL chave1 chave2
  
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
  # Checa a existência de uma chave dada
  > EXISTS chave
  
  # Tempo: 
```


```bash
  # Assiste os comandos ao vivo. Seja cuidadoso ao usar em produção.
  > MONITOR
  
  # Tempo: 
```

```bash
  # recupera informações sobre o Redis
  > INFO
  
  # Tempo: 
```


<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-1S3V0C209Z"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-1S3V0C209Z');
</script>