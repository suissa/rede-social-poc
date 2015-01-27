Estou fazendo um trabalho de analista para uma empresa aqui em Vitória onde o sistema a ser montade é o de uma rede social. Você deve estar se pensando:

\- Ah mais uma!?

E eu te respondo:

\- Sim.

Mas isso não vem ao caso e sim o desafio que é arquitetar um sistema desses para que ele possa escalar sem problemas e ter alta disponibilidade utilizando a stack [MEAN](http://bemean.com.br).

Tudo bem mas então quais são esses desafios?

Com nossa stack já escolhida utilizando apenas Javascript, precisamos pensar mais na parte do banco de dados.

**Mas como assim Suissa? Você não disse que iria usar MongoDb?**

Disse sim e ainda vou, porém não apenas ele.

Há mais de 2 anos atrás no [FISL]() eu apresentei uma palestra sobre [Banco de Dados NoSQL de código aberto](http://pt.slideshare.net/suissapg/fisl-banco-de-dados-no-sql-de-cdigo-aberto) e falei sobre 4 tipos de bancos de dados:

- chave/valor
- documento
- grafo
- coluna

**E com isso tem a ver com a rede social?**

Basicamente **TUDO!** Visto que no final da palestra eu dou exemplo EXATAMENTE de uma rede social usando esses 4 tipos de bancos diferentes. São eles:


- chave/valor: [Redis](http://redis.io/)
- documento: [MongoDb](http://mongodb.org/)
- grafo: [Neo4J](http://neo4j.com/)
- coluna: [Cassandra](http://cassandra.apache.org/)

**E como eu posso usar esses 4 bancos em um mesmo sistema?**

Da seguinte forma, primeiramente a escrita dos dados seria feita no Cassandra pois ele tem uma escrita mais rápida porém um modelo bem complexo de modelagem que o MongoDb.

Depois dessa escrita os dados seriam replicados para o MongoDb que possui a mesma modelagem que seu sistema porém de um jeito mais simples de se usar em um sistema usando MEAN. A leitura do MongoDB é melhor que do Cassandra, mas não usamos eles apenas por isso, mas sim por manter nossos dados em um modelo mais simples natural ao sistema.

Eu disse que não usamos a leitura rápida como fator decisivo, pois utilizaremos o Redis para essa função já que ele é um banco de cache que funciona na RAM, mas com possibilidade de persistência.

Então na nossa arquitetura o Cassandra funcionaria mais como um banco de backup/replica onde a escrita é muito rápida e escala muito bem. O MongoDb servirá de repositório central dos dados, utilizando um modelo comum no sistema. O Redis é o nosso banco de cache para além de ganhar velocidade, também eliminar requisições desnecessárias ao MongoDb.

**Eiiii você esqueceu do Neo4J cara!**

Calma meu querido.

É ai que a mágica acontece.

{<1>}![magic](http://weknowgifs.com/wp-content/uploads/2013/03/its-magic-shia-labeouf-gif.gif)

O Neo4J vai ser responsável por **CONECTAR** nossos dados, já que para isso o MongoDb não é muito bom já que não possui JOINs.

Nesse caso todas as conexões entre pessoas, grupos, eventos, interesses e cia; será feito pelo Neo4J que é um banco específico para isso. Caso você não conheça sobre grafos pode ler aqui na [Wikipedia](http://pt.wikipedia.org/wiki/Teoria_dos_grafos).

**Mas por que a mágica mora aí?**

Simples, é aqui onde vamos **descobrir** comportamentos, relacionamentos, interesses. Não há nenhum banco melhor para fazer esse serviço para uma rede social. O último grande banco que saiu foi o [Cayley](https://github.com/google/cayley) do Google e eu escrevi um post sobre o [Levelgraph](http://nomadev.com.br/levelgraph-um-banco-de-dados-de-grafos-para-node-js/) que é um banco de grafos para Node.js.

**Ué então por que você não usa ele?**

Infelizmente ele não possui muita documentação e ainda é muito novo, não posso arriscar um sistema dos outros com uma banco sem muitos testes e o Neo4J já está no mercado há um bom tempo. Mas para meus projetos pessoais usarei apenas o Levelgraph agora.

No sistema que estou arquitetando não usaremos o Cassandra por falta de mão de obra na equipe. :p

Mas podem esperar mais posts sobre os outros bancos, NoSQL é vida.

{<4>}![Bye](http://www.quickmeme.com/img/21/21bf40cfd5d3c170083044535d1029e2608de6e1e7941036de608e2f968ac3b2.jpg)