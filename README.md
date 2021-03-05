# 3x5 fonte em c++ para arduino
Mini projeto feito para atividade escolar de fazer um led piscar(fui um pouco mais longe).

## Recomendações previas
Melhor do que ver essa epxlicação minha toda confusa (me perdi no caminho) é melhor vendo o codigo na pratica, dá uma olhada no [Projeto no TinkerCad](http://google.com) que tém os componentes e tudo lá claramente mais facil de entender, ou dá uma olhada só no [Codigo Inteiro](http://google.com) ou passa no [Fontes](http://google.com) onde tem o valor binario de cada uma das letras e numeros.

## O codigo
O codigo em si é bem Curto, é apenas, recebe uma short int, transforma o short int em varios boleanos, e imprime nos Pinos de saida. Mas a parte interessante esta no short int.
```cpp
void Print3x5(short Char)
{
  for(int8_t i = 0; i < 15; i++)
      digitalWrite(Pins[i] ,Bit(Char,i));
}
```
## Porque um short int ao invés de um simples array. 
Inicialmente, seria  um array de boleanos mesmo e pronto, porem, um boleano, ocupa 1byte(porque não e possivel enderessar apenas 1bit), então um array de 15 booleanos ocupa uns 15bytes(120 bits), sendo que preciso ocupar apenas 15bits, ainda mais que se trada de um arduino(uno), que tem apenas 2kb de memoria para o programa, é se por exemplo, formos armazenar 37 valores diferentes para todos os numeros + Lestras + espaço, ocuparia mais ou menos um quarto da memoria (exatamente 555bytes).

Por isso é preciso armazenar todo o array de boleano em um tipo de 2bytes, que ocupa 16 bits, deixando apenas 1 bit ocupado sem motivo. se formos aplicar isso no exemplo anterior, agora ocupariamos apenas 74 bytes. E se fosse ocupar o mesmo espaço seria possivel 277 simbolos diferentes. e por causa disso, que foi usado um short int.

E isso:
```cpp
bool Char[3][5] = {
  {1,1,1},
  {0,0,1},
  {0,1,0},
  {0,1,0},
  {0,1,0}
};
``` 
Virrou isso:
```cpp
short Char = 0b1000010111110000;
``` 
## Explicação da conversão
 
Temos um array de booleanos como o na tabela abaixo(você pode pensar tambem, como cada quadrado branco, um led acesso), e cada led tem sua respectiva posição, e esta em duas dimeções o array, porem, ela e reduzida para apenas uma, para facilitar encontro da posição, já que onde fica armazenado os valores dos caracteres, esta em uma dimensão. 

-|0|1|2
:-:|:-:|:-:|:-:
0|▮|▮|▮
1|▯|▯|▮
2|▯|▮|▯
3|▯|▮|▯
4|▯|▮|▯


|0 |  1| 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |10 |11 |12 |13 |14 |15|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|▮| ▯ |▯ |▯ |▯ |▮ |▯ |▮ |▮ | ▮ |▮ |▮ |▯ |▯  |▯ |▯

É o mesmo que isso: 
`0b1000010111110000`

## retirada do bit dos Valores

A função que retira o Bit de sua posição é essa abaixo, funciona assim:

Primeiro os bits são movido  direita ( Ultima posição subtraida pela posição porque a leitura é feita pelo computador da direita para esquerda, mas é escrita da da esquerda para direita, então quanto menor o numero, mais a direita está). é por isso que nessa implementação, no final do numero binario sempre tem um zero, qué é o bit extra, porque a ultima posição(15), nunca é ocupada por nada. 

```cpp
bool Bit(short Char, int8_t Pos ){
  return (Char >> 15  - Pos)&1;
};
```
