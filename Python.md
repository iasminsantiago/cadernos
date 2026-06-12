Função x método -  transforma input em output
---------------------------------------------
- Argumentos:  valores específicos, funções fazxem os cálculos sobre esses valores específicos
- 
\-> para ver métodos acessíveis ao objeto, digita object. (com ponto) 

\-> saber o qeu o método indicado faz: help(object.method) -> def é palavra para criar funçao e método.

\-- Função: transforma input em output; bloco independente, foi definido FORA de classe; pode chamar ele direto pelo nome. function\name("argumento") // escopo: global

\-- Método: função que mora dentro de classe // o objeto será seu primeiro argumento method\name(self): o objeto é passado pro parâmetro self // construida num objeto, designado para funcionar só pro objeto // objeto.Metodo(parametros)

 

- só prcisa do __init__ quando quero que objeto NASÇA com alguma informação ou estado inicial (valor inicial pra informação)

- - variável da classe: não é criada no __init__, é criada na classe. Seja no arquivo .class ou no arquivo main (object = Class() ) Se mudar valor nela, muda em todos os objetos que tem ela.
 
    
  - variável de instância: criada no __init__. Se mudar valor em 1 objeto, não muda nos outros.
 
    
  - Ambas as variáveis acima são da classe, e os pobjetos criados terão elas. Mas a __init__ faz cada objeto ter seu valor de variável.
  ```python
  class Carro:
    # Isto é uma VARIÁVEL DE CLASSE (todas as instâncias compartilham)
    rodas = 4 

    def __init__(self, marca):
        # Isto é uma VARIÁVEL DE INSTÂNCIA (cada carro tem a sua)
        self.marca = marca
  # Criando dois carros diferentes
  carro1 = Carro("Ferrari")
  carro2 = Carro("Fusca")
  # Se eu mudar a marca do carro1, o carro2 continua sendo um Fusca
  carro1.marca = "Porsche"
  # Mas se eu mudar as rodas na CLASSE, todos mudam:
  Carro.rodas = 6
  print(carro1.rodas) # Exibe 6
  print(carro2.rodas) # Exibe 6

  ```

  chama-se o método no objeto assim: object.method(argumetn) /// argument é o input


## classes e objetos
- quando uma avriável é criada/armazenada na classe, é chamada atributo
- Criar objeto: object_name = Classname()  // my_dog = Dog()

## Listas

brekakfast_list = ['pancakes', 'chocolate', 'eggs']

append()

concatenar com +   breakfast_list = breakfast_list + ['yogurt']

index  

  - trazer elemento: lista[index]  breakfast_list[3] =  breakfast_lost[-1] -> yogurt  // com (-), começa do fim pro inicio
  breakfast_list[-2] = ["eggs"]

  - contracorrer: lista(start, stop] stop é exclusivo, start é inclusivo  breakfast_list[1:3] => ['panckaes', 'eggs']
  - correr com paradas: se não informa um deles, ele assume começo e fim da lista
    brakfast_list[1:} = ['chocolate', 'eggs', 'yogurt']
  - overwrite item da lista: só iguala   breakfast_list[1] = "sausage" // print(brakfast_list) = ['pancakes', 'sausage', 'eggs', 'yogurt']


in
  - testar se elemento ta na lista:   "waffles" in breakfast_list = False


## Dicionários
{"x":"y", "":""}
- sua a chave para acessar o valor, não pode acessar/pedir/pesquisar pelo valor
- chave é unica, valor pode ser igual a outros
- brakfast_dict = {"eggs":1, "waffles": 2, "milk": 3}
- obter valor da chave: dict_object[key]   brakfast_dict.get("waffles")  ou   breakfast_dict["waffles"]
- adicionar valor a uma chave     breakfast_dict["waffles"] += 1
- adicionar nova chave e valor   breakfast_dict["honey"] = 4

### References

Introduction to Python for Data Science and Data Engineering -  Databricks
