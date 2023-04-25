---
layout: post
title:  array - teoria e algoritmos
date:   2023-04-22 21:00:00
description: array - teoria e algoritmos
tags: ["programação", "estrutura de dados", "array"]
language: pt-br
---
## Introdução

Os arrays, sem dúvida, são uma das mais importantes estruturas de dados na área de ciência da computação. Isso ocorre, pois, ela se caracteriza por ter um tamanho fixo e contínuo de bloco de memória de um mesmo tipo de dado. Esses aspectos refletem no tempo gasto para armazenar e acessar elementos em um array. Trata-se de complexidade O(1). Além disso, arrays funcionam em um sistema de índices entre 0 e (n-1), em que n é o tamanho do array.

Como qualquer estrutura de dados, um array também possui as suas limitações. Quando não se usa o índice para buscar elementos, o que requer sempre um tempo constante, ou seja, O(1), e se usa o valor do elemento, requer-se, nesse caso, uma busca transversão por todo o array, o que significa uma complexidade O(n). O impacto dessa abordagem pode ser amenizado se o array estiver ordenado de alguma forma. 

Outras limitações que devem ser consideradas quando se trabalha com arrays são:
1. remover um elemento de um array exige que todos os elementos que seguem o termo removido seja reposicionado, movendo-os à esquerda. Nesse caso, temos uma complexidade O(n). A alternativa para evitar isso seria sobrescrever o novo valor no lugar do antigo.
2. inserir um elemento em um array, em qualquer posição que não seja a última, exige que todos os elementos que seguem o termo inserido seja reposicionado, movendo-os à direita. Nessa caso, a complexidade também é O(n). Por conta disso, inserções em um array em posições intermediárias devem ser evitadas.
3. arrays têm um tamanho fixo, o que significa que a capacidade é limitada e não pode mudar uma vez que o array for criado. No entanto, em alguns casos é necessário adicionar mais elementos em um array do que o tamanho inicial dele. Nesses casos, técnicas de realocação são utilizadas que no geral criam um array com o dobro do tamanho do original e fazem a cópia dos dados percorrendo todos os elementos do array antigo e copiando os valores para o novo array. A complexidade é linear, ou seja, O(n). Em todo caso, essa abordagem consume tempo e torna-se ineficiência em grandes arrays.
 
* **Índice**
    * [Introdução](#introduction)
    * [Obter produto de todos os outros elementos](#get-product-of-all-other-elements)
    * [~~Localizar o menor intervalo a ser ordenado~~](#localizar-o-menor-intervalo-a-ser-ordenado)
    * [~~Calcular soma máxima de subarray~~](#calcular-soma-máxima-de-subarray)
    * [~~Encontrar o menor número de elementos à direita~~](#encontrar-o-menor-número-de-elementos-à-direita)
    * [Referências](#referências)

## Obter produto de todos os outros elementos

O problema desse trecho será resolvido com um algoritmo que visa mudar o valor de cada elemento de um array pelo produto de todos os outros elementos. Por exemplo, se temos um array a = [3, 2, 1], o resultado final depois de aplicar o algoritmo será um novo array com os valores [2, 3, 6].

Seguindo essa lógica, utilizaremos teste para direcionar o processo de criação do código. O primeiro teste testará um array vazio, chamando um método que será responsável pela tarefa principal de obter produto de todos os outros elementos. Mas nesta fase, o resultado deve ser um array vazio.

A seguir, mostro os passos para criar isso. O código completo pode ser acessado [aqui](https://github.com/pFransozi/algorithms/tree/main/py-array-get-product-of-all-other-elements){:target="_blank"}.

Primeiro método para teste.

~~~ python
import unittest

class TestsGetProductOfAllOtherElements(unittest.TestCase):

def test_an_empty_array(self):

    productAllOtherelements = main.GetProductOfAllOtherElements
    result = main.GetProductOfAllOtherElements.compute_product([])
    self.assertEqual([], result, "Not equal.")
~~~

Como esperado, aqui o teste quebrou. Para resolver isso, eu criei um arquivo python, chamado main.py, com uma classe e um método, o qual é responsável por computar o array final. Então, criei uma classe `GetProductOfAllOtherElements` e um método principal, `compute_product(self, nums)`.

~~~ python
class GetProductOfAllOtherElements:
    def compute_product(self, nums):
~~~

Com isso, o erro mudou. Agora, `unittest` indica um erro no método `GetProductOfAllOtherElements` porque nada é feito. O método para desenvolver guiado por teste exige isso. Para resolver esse problema, vamos incluir um retorno do array de entrada.

~~~ python
def compute_product(self, nums):
    return nums
~~~

E então, o teste `test_an_empty_array` passou. Próximo passo, criar um teste para um array com um elemento. Nesse caso, o array que se espera deve ser o mesmo da entrada.

~~~ python
def test_an_array_with_one_element(self):
    productAllOtherelements = main.GetProductOfAllOtherElements
    result = main.GetProductOfAllOtherElements.compute_product([7])

    self.assertEqual([7], result, "Not equal.")
~~~

O teste até aqui também passou. Vamos incluir um outro teste para casos de arrays com dois elementos.

~~~ python
def test_an_array_with_one_element(self):
    productAllOtherelements = main.GetProductOfAllOtherElements
    result = main.GetProductOfAllOtherElements.compute_product([7,5])

    self.assertEqual([5, 7], result, "Not equal.")
~~~

Agora o teste não passou e o algoritmo encara um erro em um caso de teste mais complexo, o que significa que o algoritmo precisa ser melhorado considerando a lógica descrita acima: mudar cada valor de um array pelo produto de todos os outros elementos. Mas, primeiro, vamos melhorar o arquivo de teste, incluindo uma fase de `setUp` para inicializar a classe `GetProductOfAllOtherElements`

~~~ python
class TestsGetProductOfAllOtherElements(unittest.TestCase):
    
    def setUp(self):
        self.GetProductOfAllOtherElements = main.GetProductOfAllOtherElements()

    def test_an_empty_array(self):

        result = self.GetProductOfAllOtherElements.compute_product([])

        self.assertEqual([], result, "Not equal.")

    def test_an_array_with_one_element(self):
        result = self.GetProductOfAllOtherElements.compute_product([7])

        self.assertEqual([7], result, "Not equal.")
    
    def test_an_array_with_two_elements(self):
        result = self.GetProductOfAllOtherElements.compute_product([7, 5])

        self.assertEqual([5, 7], result, "Not equal.")
~~~

A classe `GetProductOfAllOtherElements` agora é o foco, e a solução proposta considera a técnica de precomputação para resolver o problema de subarrays. Isso envolve a solução de cada subarray individualmente e, então, combiná-los para construir a solução.


Antes de tudo, incluir um arquivo de teste para verificar se o argumento `nums` é vazio ou é um array de um elemento. Se assim for, então retornar o argumento `nums` sem modificação.

~~~ python
def compute_product(self, nums):
    
    if len(nums) < 2:
        return nums
~~~

Então, calcular os números que estão antes de um dado índice `i`, `prefix_products`, e os números posteriores de um dados índice `i`, `posfix_products`. Ao final, ambos, `prefix_products` e `posfix_products`, são usados para gerar o array final.

~~~ python
def compute_product(self, nums):
    
    if len(nums) < 2:
        return nums

    prefix_precompute = []
    #
    for num in nums:
        if prefix_precompute:
            prefix_precompute.append(prefix_precompute[-1] * num)
        else:
            prefix_precompute.append(num)

    posfix_precompute = []
    
    for num in reversed(nums):
        if posfix_precompute:
            posfix_precompute.append(posfix_precompute[-1] * num)
        else:
            posfix_precompute.append(num)
    
    posfix_precompute = list(reversed(posfix_precompute))

    compute_result = []
    for i in range(len(nums)):
        if i == 0:
            compute_result.append(posfix_precompute[i + 1])
        elif i == len(nums) - 1:
            compute_result.append(prefix_precompute[i - 1])
        else:
            compute_result.append(prefix_precompute[i - 1] * posfix_precompute[i+1])

    return result
~~~

Com esse algoritmo, `test_an_array_with_two_elements` passou. Vamos, então, incluir dois outros testes.

~~~ python
class TestsGetProductOfAllOtherElements(unittest.TestCase):
    
    def setUp(self):
        self.GetProductOfAllOtherElements = main.GetProductOfAllOtherElements()

    def test_an_empty_array(self):

        result = self.GetProductOfAllOtherElements.compute_product([])

        self.assertEqual([], result, "Not equal.")

    def test_an_array_with_one_element(self):
        result = self.GetProductOfAllOtherElements.compute_product([7])

        self.assertEqual([7], result, "Not equal.")
    
    def test_an_array_with_two_elements(self):
        result = self.GetProductOfAllOtherElements.compute_product([7, 5])

        self.assertEqual([5, 7], result, "Not equal.")

    def test_an_defined_array_a(self):
        result = self.GetProductOfAllOtherElements.compute_product([3, 2, 1])
        self.assertEqual([2, 3, 6], result, "Not Equal.")

    def test_an_defined_array_b(self):
        result = self.GetProductOfAllOtherElements.compute_product([1, 2, 3, 4, 5])
        self.assertEqual([120, 60, 40, 30, 24], result, "Not equal.")
~~~

Quando todos os testes passam, é possível refatorar o código de modo mais seguro.

~~~ python
class GetProductOfAllOtherElements:

    def is_less_than_two_elements(self, nums):
        if len(nums) < 2:
            return True

        return False
    
    def compute_element_and_insert(self, products, num):
        if products:
            return products[-1] * num
        else:
            return num

    def compute_prefix(self, nums):
        prefix = []
        for num in nums:
            prefix.append(self.compute_element_and_insert(prefix, num))

        return prefix
    
    def compute_posfix(self, nums):
        
        posfix = []
        for num in reversed(nums):
            posfix.append(self.compute_element_and_insert(posfix, num))
        
        posfix = list(reversed(posfix))
        return posfix
    
    def get_element(self, i, prefix, posfix, nums):
        
        if i == 0:
            return posfix[i + 1]
        elif i == len(nums) - 1:
            return prefix[i - 1]
        else:
            return prefix[i-1] * posfix[i + 1]
        
    
    def is_first_element(self, i):
        return i == 0
    
    def is_last_element(self, i, nums):
        return i == len(nums) - 1

    def compute_product(self, nums):
        if self.is_less_than_two_elements(nums):
            return nums

        prefix = self.compute_prefix(nums)
        posfix = self.compute_posfix(nums)
        
        result = []
        for i in range(len(nums)):
            if self.is_first_element(i):
                result.append(self.get_element(i, prefix, posfix, nums))
            elif self.is_last_element(i, nums):
                result.append(self.get_element(i, prefix, posfix, nums))
            else:
                result.append(self.get_element(i, prefix, posfix, nums))

        return result
~~~

[⇡](#introdução)

## Localizar o menor intervalo a ser ordenado

## Calcular soma máxima de subarray

## Encontrar o menor número de elementos à direita

[⇡](#introdução)

## Referências

Cormen, T. H., Leiserson, C. E., Rivest, R. L., & Stein, C. (2022). Introduction to Algorithms, fourth edition. MIT Press.

Miller, A., & Wu, L. (2019). Daily Coding Problem: Get Exceptionally Good at Coding Interviews by Solving One Problem Every Day.

[en.wikipedia.org: Array_(data_structure)](https://en.wikipedia.org/wiki/Array_(data_structure)){:target="_blank"}.

[⇡](#introdução)