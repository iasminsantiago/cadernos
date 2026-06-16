# Python


- scripts podem ser salvos, terminals não. COm isso, posso reusar código script de python: arquivos que terminam com .py. ABro, clcio run e não precisei escrever novamente.
- 
---
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



## Listas
- como profissionais de dados,trabalharemos com vários pontos de dados. Ao invés de criar uma var para CADA ponto/valor coletado, armazenamos todos os valores numa python list

- 
## Dicionários
{"x":"y", "":""}
- sua a chave para acessar o valor, não pode acessar/pedir/pesquisar pelo valor
- chave é unica, valor pode ser igual a outros
- brakfast_dict = {"eggs":1, "waffles": 2, "milk": 3}
- obter valor da chave: dict_object[key]   brakfast_dict.get("waffles")  ou   breakfast_dict["waffles"]
- adicionar valor a uma chave     breakfast_dict["waffles"] += 1
- adicionar nova chave e valor   breakfast_dict["honey"] = 4

  ---
  vALORES INCONSISTENTES - não seguem padrão, por isso não estão consistentes com ele. Estado: PE, pe
  VALORES INCORRETOS - não estão no formato correto. Valor: R$ 20,00 $20,00

- df.info() mostra estrutura do df - tipo de dados das colunas, quantas linhas tem etc
- df.describe() - mostra estatisticas das colunas (std, count, mean etc)

- df.isnull.sum() mostra quantos valores ausentes tem em cada coluna<img width="181" height="352" alt="image" src="https://github.com/user-attachments/assets/36aa98bf-add8-4ddb-8f4b-0e6d545c745b" />

valores ausentes: 
 - ou preenche com media/mediana/etc df["Age"] = df["Age"].fillna(df["Age"].mean()) a decisão de qal mérica usar dependen dos tiposde dados e objetivo da análise
 - ou usa new_df = df.*dropna()* para tirar linhas que tem valores ausentes

 Ao importar um arquivo CSV para o Pandas, como são representados os campos vazios no DataFrame?
Como valores NaN.
-> NaN = Not a Number

Após o carregamento dos dados, qual é a próxima etapa no pipeline discutido na aula?
Limpeza dos dados, identificando e tratando problemas como valores ausentes e duplicados

O que significa o valor especial 'NaN' em um DataFrame do Pandas?
Representa um valor ausente ou desconhecido em um dataset.

No processo de normalização Min-Max, qual operação é realizada?
Subtrai o valor mínimo e divide pelo intervalo entre o valor máximo e mínimo

Ao criar uma nova coluna no DataFrame com apply e lambda x: x, qual o resultado esperado?
A nova coluna será uma cópia idêntica da coluna original.

No pipeline discutido, ao criar uma nova coluna normalizada, é fundamental que:
Os valores dessa coluna estejam entre 0 e 1, facilitando o uso em análises estatísticas

O que método apply() faz?
Aplica uma mudança/função par todos os elementos das linhas/colunas

O que é reprodutibilidade em análise de dados?
A capacidade de outras pessoas executarem o mesmo código e obterem os mesmos resultados.

No exemplo do arquivo Vendas.txt, o comando '\n' utilizado no método write serve para:
Indicar uma quebra de linha ao gravar os valores

  ---
  para ver erros, podemos usar a  logging: 
  logging.debug(), 
  logging.warning() não indica erro, só possível erro
  logging.error() indica erro
  logging.critical() indica erro crítico
  logging.info() Registrar mensagens informativas sobre o funcionamento do programa.
  <img width="337" height="150" alt="image" src="https://github.com/user-attachments/assets/33557149-f13b-46bf-9593-afd02c248f6b" />

  Em projetos de análise de dados, um erro ignorado pode gerar qual consequência grave?
  Resultados incorretos que interferem na tomada de decisão

  ---
  Modularização - Separar tarefas como cálculo, execução do pipeline e testes em módulos diferentes facilita a escalabilidade e legibilidade do código
  - cálculos - utils
  - Execução - main
  - Testes- tests
  <img width="175" height="126" alt="image" src="https://github.com/user-attachments/assets/9c7e1050-7ae8-479c-96ec-a318cc43826a" />

O arquivo main.py deve ser o ponto de entrada e orquestrar o fluxo do pipeline no projeto.

O que deve ser incluído no arquivo requirements.txt?
Todas as dependências e bibliotecas necessárias para rodar o projeto

O que significa um código ser escalável no contexto de análise de dados?
Ser capaz de lidar com volumes de dados maiores sem perda significativa de performance.

---
Refatorar - Melhroar o códifgo sem alterar sseu funcionamento

Profilling - Analisar o desempenho do código (rapidez de execução etc). Medir o desempenho do código para identificar gargalos de performance.
Qual das alternativas a seguir é uma vantagem do uso do módulo time em Python?
Permite medir quanto tempo um bloco de código leva para ser executado.

Sobre performance em análise de dados, qual é o maior risco de um código lento?
Impactar o tempo de processamento quando manipula grandes volumes de dados.


<img width="317" height="228" alt="image" src="https://github.com/user-attachments/assets/b64a954e-20d7-4dc8-9dd8-aa69c40e3338" />
<img width="657" height="396" alt="image" src="https://github.com/user-attachments/assets/02f8c23d-fef6-405a-964b-b39e69b1038a" />
<img width="423" height="97" alt="image" src="https://github.com/user-attachments/assets/a28745ee-101f-4881-851c-96c68e0a2440" />
<img width="457" height="263" alt="image" src="https://github.com/user-attachments/assets/29045686-36ba-4ebd-81d0-8c4699061f02" />
<img width="462" height="292" alt="image" src="https://github.com/user-attachments/assets/6f6e6157-c8f8-4dd3-8c9e-dbb774ba9427" />
<img width="402" height="251" alt="image" src="https://github.com/user-attachments/assets/c1bfc792-2d78-4762-9f6e-7b858bf82d6a" />
# Regressão:
<img width="397" height="186" alt="image" src="https://github.com/user-attachments/assets/db95a4c4-71b5-4aa8-8e4d-5b9a190f6f8f" />
<img width="456" height="220" alt="image" src="https://github.com/user-attachments/assets/c632312e-7528-4c1e-a119-f49d62e711b7" />
<img width="442" height="363" alt="image" src="https://github.com/user-attachments/assets/f66f2f28-0c5b-4f16-bcf9-83a0385c8ecc" />
<img width="431" height="350" alt="image" src="https://github.com/user-attachments/assets/62d23c54-7983-40e6-a191-f97fc7b51c0c" />

# Classificação:
<img width="621" height="265" alt="image" src="https://github.com/user-attachments/assets/45a790d7-5517-45db-8f76-a524174c9cdc" />
<img width="441" height="243" alt="image" src="https://github.com/user-attachments/assets/5e9447ab-4355-4c6f-adf6-eefa3fd7d64f" />
<img width="302" height="333" alt="image" src="https://github.com/user-attachments/assets/42d06fe6-acd7-4a7b-ab4f-2df7eb86d704" />
<img width="382" height="387" alt="image" src="https://github.com/user-attachments/assets/a69f8b06-d977-4a3e-8e3c-1ec9ae25e6eb" />
<img width="446" height="233" alt="image" src="https://github.com/user-attachments/assets/a4bb29fc-34f2-4eaa-a3be-9708f2cb2f69" />
<img width="401" height="203" alt="image" src="https://github.com/user-attachments/assets/e57007bd-e5b2-4708-8d22-1992a04dcbbc" />
<img width="323" height="156" alt="image" src="https://github.com/user-attachments/assets/71c8b262-450e-419b-91a5-4c36d1470e80" />
<img width="427" height="388" alt="image" src="https://github.com/user-attachments/assets/15e2afb9-2361-4661-a46e-38b43248e2d7" />

No método de agrupamento (clustering), qual é o objetivo principal da métrica WSS (Within-Cluster Sum of Squares)?
Avaliar a dispersão dos pontos dentro de cada cluster.

Quando é preferível analisar múltiplas métricas de avaliação em vez de uma única métrica isolada?
Quando se deseja uma visão mais completa do desempenho do modelo.



### References

Introduction to Python for Data Science and Data Engineering -  Databricks
DIO - Bootcamp Accenture - cursos Processamento e Limpeza de Dados em Python, Automação de Processos e Análises com Python, Boas Práticas, Testes e Otimização de Código em Python

