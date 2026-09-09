# SO e Arquitetura

## Modos de endereçamento

**Notações**

- A indica um endereço da memória  
- R indica um registrador   
- EA é o endereço de fato onde está o operando referenciado
> Atenção para o endereçamento direto por registrador, em que aponta direto para o registrador em que está o valor e não para um endereço de memória.

- (A) indica o conteúdo da memória  
- (R) indica o conteúdo do registrador 
> O conteúdo destes, por sua vez, pode ser um valor ou outro endereço.
- (EA) indica o operando/valor em si, sempre que existe um endereço calculado (endereçamento direto/indireto, por exemplo) 
> Exceção para o endereçamento imediato, porque não tem cálculo de EA. Nesse caso,o valor já vem direto dentro da própria instrução.

**Exemplo geral**
- R = 3 indica o registrador R3 e A = 200 indica o endereço de memória 200 .
- (R) indica o conteúdo do registrador e (A) indica o conteúdo do endereço. Que por sua vez podem ser um valor ou um outro endereço.

**Tamanho da instrução**
- A instrução e os operadores passados nela possuem tamanho limitado: por isso, acessam apenas um determinado número de registradores e de espaços na memória, assim como os valores passados diretamente também têm um limite menor.
- Por sua vez, um registrador ou um endereço de memória na instrução podem apontar para um espaço do tamanho da palavra. Dessa forma, podem apontar para um valor bem maior ou para muitos mais endereços de memória (é essa a lógica do endereçamento indireto, que será visto adiante).

**Endereçamento imediato**

- O valor já está na própria instrução. Não há EA a calcular.
- Vantagem é que é mais rápido (acesso ao dado diretamente pela instrução), mas o valor é limitado ao tamanho do campo de operando da instrução.
- Exemplo: `MOV B, #20H`

**Endereçamento direto x Endereçamento indireto**

- A vantagem do direto sobre o indireto é que ele envolve menos saltos de memória e por isso é mais rápido.
- No entanto, no direto, o tamanho do endereço é limitado (campo de endereço da instrução), e, por isso, acessa menos posições na memória.
- No indireto, por outro lado, é possível acessar campos de endereço do tamanho da palavra, o que possibilita acessar mais posições na memória (o endereço na instrução pode apontar p/ um número limitado de endereços, que por sua vez apontam para endereços com tamanho da palavra).

**Exemplos de endereçamento direto e indireto** 

1. Endereçamento direto usando A (memória)
```
A = 300
EA = A = 300
Operando = (EA) = (300) = 42
```
- 300 é o endereço, 42 é o valor, o operando. 
- Note que o endereço final em que está o operando é passado diretamente, sem parênteses (linha 2).
- Por sua vez, este mesmo endereço do operando é representado por EA, sem os parênteses, enquanto o operando em si está dentro de EA, por isso a notação (EA), entre parênteses (linha 3).
- Aqui damos apenas um salto de memória, por isso é um modo de endereçamento mais ágil.

2. Endereçamento indireto usando A (memória)
```
A = 100
EA = (A) = (100) = 300
Operando = (EA) = (300) = 42
```
- 100 é o endereço de primeiro nível, 300 é o de segundo e o valor (operando) é 42.
- Seguindo o raciocínio, o endereço final, EA, é 300, que está dentro de A.
- Este por sua vez aponta para o operando (EA), que é igual a 42.
- 2 saltos na memória, mais lento.

3. Endereçamento direto usando R (registrador) 
> Também chamado apenas de endereçamento por registradores.
```
R = R1
EA = R = R1
Operando = (EA) = (R) = (R1) = 300
```
- 300 aqui é o valor.
- Nesse caso, aponta direto para o Registrador (e não para um endereço de memória), que aponta para o valor.
- Por isso, EA = R e (EA) = 300.
- Mais rápido. Valor direto no registrador.

4. Endereçamento indireto usando R (registrador)
```
R = R1
EA = (R) = (R1) = 300
Operando = (EA) = (300) = 42
```
- 300 é um espaço na memória e 42 é o valor guardado por ele.
- O registrador aponta para o espaço na memória, que é o endereço final em que está o valor, o EA.
- O operando está dentro deste espaço, logo usa-se a notação (EA).
- 1 salto na memória apontada pelo registrador. Mais rápido que 2.

**Endereçamento por deslocamento**

- EA = A + (R)
- De forma geral, soma-se o endereço A com o conteúdo do registrador R e se obtém o endereço de destino.

1. Endereçamento Relativo

- EA = A + (PC), tal que A pode ser negativo ou positivo.
- O registrador da fórmula é o PC (Program Counter). Esse registrador guarda sempre o endereço da próxima instrução.
- A ideia é: ande sempre N posições a partir de onde você está.
- Casos de uso: loops (for/while), condicionais (if/else). Pulos curtos, relativos à posição atual.

2. Endereçamento por Registrador Base

- EA = A + (R_BASE), tal que A só pode ser positivo.
- O registrador nesse caso é um ponto de referência fixo (geralmente guarda o início de um bloco de memória (início de um vetor, segmento de um programa)).
- Casos de uso: realocação de um programa na memória.

3. Indexação

- EA = A + (R_INDICE).
- Mesma coisa do endereçamento por registrador base, só que agora o A que é o endereço fixo. O que varia está no registrador.
- Percorrer um vetor ao longo de um loop: o R_INDICE varia a cada iteração.
- Caso de uso: um for percorrendo uma estrutura de dados na memória.
- *A diferença prática entre o endereçamento por registrador base e a indexação é majoritariamente por convenção de uso.*

**Endereçamento de pilha**

- Caso a parte de endereçamento implícito.
- Um registrador aponta implicitamente para o endereço do topo da pilha.
- Instruções como PUSH e POP empilham e desempilham o topo, sem precisar apontar para o topo da pilha explicitamente, pq o processador já sabe que se referem ao topo.
- Casos de uso: chamadas de função, retornos, recursão.

**Sistema de segmentos padrões (arquitetura x86)**

- O processador x86 por padrão aciona registradores determinados ao realizar suas operações específicas. 
- Na prática, realiza endereçamentos por deslocamentos, assumindo os endereços fixados nesses registradores base, de acordo com a operação que está sendo realizada naquele momento.
- Exemplo: MOV [3420H], AX.
  - É um modo de endereçamento direto, por memória (o conteúdo da AX está sendo movido para o espaço da memória 3420H).
  - Na prática, por ser uma operação de **acesso geral a dados,** utiliza o registrador DS (Data Segment) de base implicitamente. Aí desloca o equivalente a 3420H a partir desse registrador.
  - *Ver tabela completa de tipos de segmento e casos de uso no histórico da conversa com o claude.*

**Registradores de uso geral (arquitetura x86)**

| Registrador | Descrição |
|---|---|
| **EAX** | Acumulador — usado em operações aritméticas |
| **ECX** | Contador — usado em loops |
| **EDX** | Dados — usado em E/S, multiplicação/divisão (extensão do acumulador - EDX:EAX) |
| **EBX** | Base — aponta para dados no segmento DS. Ex. início de vetor (EBX (base) + deslocamento). |
| **ESP** | Stack Pointer — aponta para o **topo da pilha** (endereço mais baixo). Ex. PUSH e POP o decrementam e aumentam automaticamente. |
| **EBP** | Base do Frame — acessa argumentos de procedimentos passados pela pilha??? |
| **ESI** | Source Index — aponta para dados a **copiar de** (fonte)??? |
| **EDI** | Destination Index — aponta para o **destino** dos dados copiados??? |

> De forma geral, o uso específico desses registradores é convencionado. No entanto, para algumas operações em específico, por conta da arquitetura física do processador só cabem as operações determinadas mesmo.

> **MUL, DIV, LOOP**, operações de string, pilha realmente **exigem** aquele registrador específico por regra física do hardware.

> Para instruções genéricas (**MOV, ADD, SUB**), qualquer um dos 8 registradores serve igualmente bem.