
### Resultados:

#### Sem lock

| Requisções | Tempo médio | Tempo total   | Sucessos | Falhas por falta de estoque | Erros da aplicação |
|------------|-------------|---------------|----------|-----------------------------|--------------------|
| 100        | 1255ms      | Aprox 2s 88ms | 100      | 0                           | 0                  |
| 1000       | 5982ms	      | 12s           | 942     | 58                           | 0                  |
| 10000      | 28120ms         | 4m 43s        | 6815     | 1398                         | 7510               |


##### Inconsistências
- *100 requisições*: o saldo final de estoque veio incorreto ficou em 964
- *1.000 requisições*: O saldo final de estoque veio incorreto, ficou em 858.
- *10.000 requisições*: O saldo de estoque zerou porém foram processados muito mais pedidos do que o necessário


#### Lock  otimista

| Requisções | Tempo médio | Tempo total   | Sucessos | Falhas por falta de estoque | Erros da aplicação |
|------------|-------------|---------------|----------|-----------------------------|--------------------|
| 100        | 1253ms      | Aprox 2s 67ms | 100      | 0                           | 0                  |
| 1000       | 6048ms	      | 12s 89ms           | 918     | 32                           | 50                  |
| 10000      | 28480ms         | 4m 45s        | 8497     | 798                         | 705               |

##### Inconsistências:
- *100 requisiições*: Não houveram
- *1.000 requisições*:  saldo de estoque zerou porém houveram pedidos processados incorretamente
- *10.000 requisições*: saldo de estoque zerou porém houveram pedidos processados incorretamente

#### Lock  pessimista

| Requisções | Tempo médio | Tempo total   | Sucessos | Falhas por falta de estoque | Erros da aplicação |
|------------|-------------|---------------|----------|-----------------------------|--------------------|
| 100        | 1253ms      | Aprox 2s 67ms | 22      | 0                           | 77                  |
| 1000       | 6795ms      | 13s 580ms           | 892     | 22                           | 86                  |
| 10000      | 31987ms         | 	5m 19s        | 8894     | 608                         | 498               |

##### Inconsistências:
- *100 requisiições*: Não houveram Inconsistências, os erros da aplicação foram gerados pelo lock
- 1.000 requisições: Não houveram inconsistências, os erros da aplicação geraram retry automático.
- 10.000 requisições: Houve timeout devido a fila de aguardo para a liberação das transações
