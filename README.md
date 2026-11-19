# prova2sistemasdistribuidos

Nome: Miguel Luz Andrino
3 semestre
Sistemas Distribuidos.

Foi nos dado uma situação onde uma equipe de Ti identificou 3 erros no funcionamento do sistema, nossa tarefa é executar o codigo e verificar quais são os pontos de falha e corrigilos.

A primeira etapa foi criar o arquivo a qual iriamos conseguir utilizar o codigo, para isso criamos a pasta **simulado_distribuidos**.

E em seguida criamos o arquivo **central_pedidos.py**

-------------------------------------------------------------------------------------------------------------------------------------------------------

No Problema A, a empresa havia estabelecido que apenas dois processos podia estar sendo processados simultaneamente, no entanto o programa estava permitindo três.

Para resolver isto, basta que na variável **MAXIMO_SIMULTANEO** alteremos o valor de 3 para 2 e em seguida retiremos o numero '103' do bloco a seguir:
trabalhadores = [
    threading.Thread(
        target=processar_pedido,
        args=(numero,)
    )
    for numero in [101, 102, 103]
]

-------------------------------------------------------------------------------------------------------------------------------------------------------

O Problema B, dizia que ao realizar a sincronização do estoque o principal realizava a sincronização correta, porém a replica continuava a mostrar o valor anterior.

Foi identificado que na função sincronizar havia duas variaveis nomeadas 'replicas', ou seja uma recebia o valor da outra e no final ambas apresentavam o mesmo valor.

Para resolver isso foi necessario alterar uma delas para 'principal' assim resolvendo o problema. Segue o bloco alterado:

ANTES
def sincronizar():
    replica["estoque"]= replica["estoque"]
    
DEPOIS:
def sincronizar():
    replica["estoque"]= principal["estoque"]


---------------------------------------------------------------------------------------------------------------------------------------------------

Problema C, apos os teste de falha foi identificado que a replica esta indisponivel, após realizar a verificação do programa foi identificado que na função consultar estoque, o bloco esta da seguinte maneira:

ANTES
return "INDISPONIVEL", None

DEPOIS
return "DISPONIVEL", replica["estoque"]

Após fazer esta alteração a replica passou a ficar disponivel.

-----------------------------------------------------------------------------------------------------------------------------------------------------

Questão 1. qual é a função do semaforo no processamento dos pedidos?
R. Esta função faz com que haja um certo tempo de execução entre os processos.

Questão 2. Porque é necessário sincronizar os dados entre os servidor principal e a replica.
R. Esta função serve para que qualquer informação que seja alterada em um servidor, faça com que o outro também sofra essa mudança, mantenho ambos os servidores atualizados.

Questão 3. O que significa tolerância de falhas e como ela aparece neste prgrama?
R. A tolerancia de falhas significa que são processos que podem ser perdidos pois não afetarão no resultado final.

Questão 4. Para que serve o Lock no programa e como o controle de acesso aos recursos ajuda a prevenir problemas de concorrencia?
R. A trava serve para que o servidor não deixe que outros processos sobreescrevam os que já estão sendo executados.

Questão 5. Considerando a sequencia de eventos, a sincronização deve ocorrer antes ou depois do processamento dos pedidos? Explique a relação de casualidade.
R. A sincronização deve ocorrer depois do processamento.
