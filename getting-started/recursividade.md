# Recursividade

Requisitos

* Saber o que são funções, variáveis, loops
* Saber o que é uma stack.



## O que é?

Recursão é o momento em que uma função chama a si mesma.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption><p>catei desse site: <a href="http://geraldoferraz.blogspot.com/2011/11/recursividade.html">http://geraldoferraz.blogspot.com/2011/11/recursividade.html</a></p></figcaption></figure>

Exemplos em java:

```java
// Função que vai fazer uma contagem do número N até 0
public void contagemRegressiva(int number){
    if (number == 0) {
        return;
    }
    System.out.println(number);
    contagemRegressiva(number - 1); // <- Recursão
}
```



## Estrutura de uma recursão (macetes)

TODO

## Quando que eu uso?

Parecido com os loops, recursão vai ser usada pra resolver problemas sequenciais, como o fibonacci, fatoriais, algoritmos de busca em árvores, tbm no quicksort, divide and conquer...



O problema é que tem um custo. E o pior, o custo só é descoberto de 2 formas:

1. Tomando um erro na fuça sem saber o que fazer
2. Ouvir de alguém que já tomou um erro na fuça sem saber o que fazer

Vamos ver um exemplo de um cálculo de [fibonacci](https://brasilescola.uol.com.br/matematica/sequencia-fibonacci.htm) recursivo:

```java
// Retornará o número x da sequência de fibonacci
public long fibonacci(int x) {
  if (x == 0 || x == 1 ) {
    return x;
  }

  return fibonacci(x - 1) + fibonacci(x - 2);
}
```

Se a gente chamar essa função  com `x = 2` retorna `1`, `x = 4` retorna `3`  e se `x = 6` retorna `8`

Mas e se eu exagerar e chamar enviando  10 milhões? A resposta é um erro esquisito pra quem nunca viu.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>Esse nome é familiar...</p></figcaption></figure>

Fora o `StackOverflowError`, há também várias linhas gritando&#x20;

```
at org.example.Main.fibonacci(Main.java:9)
at org.example.Main.fibonacci(Main.java:9)
at org.example.Main.fibonacci(Main.java:9)
at org.example.Main.fibonacci(Main.java:9)
```

### O que tá acontecendo?

Toda vez que chamamos uma função ela é guardada numa stack e liberada quando ela retorna. Essa é a _Call Stack (_pilha de chamadas).&#x20;

Daí você pode imaginar o que seria esse StackOverFlow, ele vai surgir quando a call stack estiver cheia. Você pode até aumentar o tamanho da stack pra seu código passar, mas não vai ser o suficiente

Passo a passo:

```java
public static long fibonacci(int x) {
  if (x == 0 || x == 1 ) {
    // 3) Somente nesse return que a função sai da callstack.
    return x;
  }
  
  // 2) Toda vez que a função fibonacci chama ela mesma
  // Essas 2 chamadas são guardadas na callstack
  return fibonacci(x - 1) + fibonacci(x - 2);
}

public static void main(String[] args){
  fibonacci(10_000_000); // 1) primeira chamada indo pra callstack
}
```

Recomendo esse [link](https://developer.mozilla.org/pt-BR/docs/Glossary/Call\_stack) pra saber mais sobre o que é Call Stack.



E os `at org.example.Main.fibonacci(Main.java:9)` que apareceram no console?

Toda vez que uma exception é lançada, nosso runtime puxa as funções que estavam na stack até chegar no primeiro item da callstack. No caso "Main.java:9" diz que foi na linha 9 do arquivo Main.java, onde na minha IDE é a exata linha em que acontece a recursão.

Como resolver?

Então, esse é o trade-off de uma função recursiva, se ela fizer muitas iterações a callstack pode estourar. É daí que a gente troca pra um loop e para de jogar chamadas na stack.

```java
public static long fibonacciIterativo(int x){              
   if (x == 0 || x == 1){                                
       return x;                                         
   }

   long previous = 0, current = 1;                       
   for(long position = 1; position <= x; position++){    
       long next = previous + current;                   
       previous = current;                               
       current = next;                                   
   }

   return current;                                       
}                  

// Pode testar que pelo menos com x = 10 milhões isso roda                                       
```

