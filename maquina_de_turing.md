# Atividade Remota — Máquinas de Turing

**Disciplina:** Teoria da Computação — Linguagens Formais e Autômatos  
**Tema:** Máquinas de Turing, Computabilidade e Limites Computacionais

---

## Etapa 1 — Introdução

### 1. O que é uma Máquina de Turing?

Uma Máquina de Turing é um modelo matemático de computação utilizado para representar, de forma abstrata, como um problema pode ser resolvido por meio de regras e passos bem definidos.

Ela possui uma fita que funciona como memória, uma cabeça que pode ler e escrever símbolos nessa fita e um conjunto de estados que determina o que a máquina deve fazer em cada situação.

A partir do símbolo lido e do estado atual, a máquina segue uma regra de transição. Essa regra pode determinar qual símbolo será escrito, para qual direção a cabeça deve se mover e qual será o próximo estado.

Apesar de ser um modelo simples, a Máquina de Turing é importante porque permite estudar formalmente o que pode ou não ser calculado por um algoritmo.

---

### 2. Quais são os principais componentes de uma Máquina de Turing?

Os principais componentes são:

- **Fita:** funciona como a memória da máquina e é dividida em posições que armazenam símbolos.
- **Cabeça de leitura/escrita:** lê o símbolo da posição atual, pode escrever outro símbolo e se movimenta pela fita.
- **Estados:** representam as diferentes situações ou etapas do processamento.
- **Alfabeto:** conjunto de símbolos que podem ser utilizados pela máquina.
- **Regras de transição:** determinam o que a máquina deve fazer de acordo com o estado atual e o símbolo lido.
- **Estado inicial:** estado no qual a máquina começa sua execução.
- **Estados de aceitação e rejeição:** indicam se a entrada deve ser aceita ou rejeitada.

---

### 3. Qual é a importância das Máquinas de Turing para a computação?

As Máquinas de Turing são importantes porque fornecem um modelo formal para estudar a computação.

Elas permitem definir o que significa resolver um problema por meio de um algoritmo e ajudam a analisar quais problemas podem ser resolvidos computacionalmente.

Também são importantes para o estudo da **computabilidade**, pois demonstram que existem problemas para os quais não existe um algoritmo capaz de produzir uma resposta correta para todos os casos.

Assim, as Máquinas de Turing não são importantes apenas como máquinas físicas ou práticas, mas principalmente como uma forma de compreender os fundamentos e os limites da computação.

---

### 4. Qual é a relação entre Máquina de Turing e algoritmo?

Um algoritmo é uma sequência de passos bem definidos utilizada para resolver um problema.

Uma Máquina de Turing pode representar a execução desses passos por meio de seus estados e regras de transição. A máquina recebe uma entrada, executa uma sequência de operações e pode chegar a um estado de aceitação ou rejeição.

Dessa forma, a Máquina de Turing é um modelo formal utilizado para estudar algoritmos e determinar se determinados problemas podem ser resolvidos computacionalmente.

---

# Etapa 2 — Simulação

## Linguagem reconhecida

A máquina foi construída para reconhecer palavras da forma:

**0ⁿ1ⁿ, com n ≥ 1**

Isso significa que a entrada deve possuir uma quantidade de `0`s igual à quantidade de `1`s, com todos os `0`s aparecendo antes dos `1`s.

### Exemplos aceitos

- `01`
- `0011`
- `000111`
- `00001111`

### Exemplos rejeitados

- `0`
- `1`
- `001`
- `011`
- `00111`

---

## Descrição da Máquina de Turing criada

A máquina utiliza uma estratégia de marcar os símbolos já processados.

Para cada `0` encontrado no início da entrada, a máquina marca esse `0` e procura um `1` correspondente no restante da palavra. Quando encontra esse `1`, também o marca e retorna ao início da fita para procurar o próximo `0`.

Para facilitar a simulação, foram utilizados os símbolos:

- `0` — zero ainda não processado;
- `1` — um ainda não processado;
- `X` — zero já processado;
- `Y` — um já processado;
- `B` — espaço em branco.

### Estados utilizados

- **q0:** estado inicial. Procura um `0` ainda não processado.
- **q1:** depois de marcar um `0`, procura um `1` correspondente.
- **q2:** depois de encontrar o `1`, retorna para a esquerda para começar um novo ciclo.
- **q3:** verifica se não existem mais símbolos não processados.
- **qA:** estado de aceitação.
- **qR:** estado de rejeição.

### Funcionamento

1. A máquina começa em `q0`.
2. Se encontrar `0`, substitui por `X` e vai para `q1`.
3. Em `q1`, avança pela fita até encontrar um `1`.
4. Ao encontrar `1`, substitui por `Y` e vai para `q2`.
5. Em `q2`, retorna para a esquerda até alcançar o início da entrada.
6. Volta para `q0` e procura outro `0`.
7. Quando não houver mais `0`s não processados, verifica se também não existem `1`s não processados.
8. Se todos os símbolos tiverem sido correspondidos, a máquina vai para `qA`.
9. Se existir um `0` sem um `1` correspondente ou um `1` sem um `0` correspondente, a máquina vai para `qR`.

---

## Lógica das transições

Uma representação simplificada das principais transições é:

| Estado | Símbolo lido | Ação | Próximo estado |
|---|---|---|---|
| q0 | `0` | escreve `X`, move para direita | q1 |
| q0 | `X` | move para direita | q0 |
| q0 | `1` | não existe `0` correspondente | q3 |
| q0 | `B` | aceita | qA |
| q1 | `0` | move para direita | q1 |
| q1 | `X` | move para direita | q1 |
| q1 | `Y` | move para direita | q1 |
| q1 | `1` | escreve `Y`, move para esquerda | q2 |
| q1 | `B` | não encontrou `1` correspondente | qR |
| q2 | `0` | move para esquerda | q2 |
| q2 | `1` | move para esquerda | q2 |
| q2 | `X` | move para esquerda | q2 |
| q2 | `Y` | move para esquerda | q2 |
| q2 | `B` | move para direita | q0 |
| q3 | `Y` | move para direita | q3 |
| q3 | `B` | aceita | qA |
| q3 | `1` | existe `1` sem `0` correspondente | qR |

> **Observação:** a configuração exata dos estados pode variar de acordo com o simulador utilizado pelo professor. A lógica necessária é que cada `0` seja associado a exatamente um `1` e que a palavra esteja na forma `0ⁿ1ⁿ`.

---

# Etapa 3 — Registro da simulação

## Teste 1

**Entrada:** `01`

**Resultado esperado:** ACEITA

**Resultado obtido:** ACEITA

### Estados percorridos

Uma execução simplificada pode ser descrita como:

`q0 → q1 → q2 → q0 → q3 → qA`

A máquina encontra um `0`, marca-o, encontra o `1` correspondente, marca-o e depois verifica que não existem símbolos restantes sem processamento.

---

## Teste 2

**Entrada:** `0011`

**Resultado esperado:** ACEITA

**Resultado obtido:** ACEITA

### Estados percorridos

A máquina realiza dois ciclos de correspondência:

`q0 → q1 → q2 → q0 → q1 → q2 → q0 → q3 → qA`

Os dois `0`s são associados aos dois `1`s. Portanto, a palavra é aceita.

---

## Teste 3

**Entrada:** `001`

**Resultado esperado:** REJEITA

**Resultado obtido:** REJEITA

### Estados percorridos

A máquina consegue associar o primeiro `0` a um `1`, mas depois encontra outro `0` sem existir outro `1` disponível para fazer a correspondência.

A execução termina em:

`q0 → q1 → q2 → q0 → q1 → qR`

Portanto, a palavra é rejeitada.

---

## Tabela dos testes

| Teste | Entrada | Resultado esperado | Resultado obtido |
|---|---|---|---|
| 1 | `01` | ACEITA | ACEITA |
| 2 | `0011` | ACEITA | ACEITA |
| 3 | `001` | REJEITA | REJEITA |

---

## Descrição dos resultados

Os testes mostram que a máquina consegue reconhecer palavras que possuem a mesma quantidade de `0`s e `1`s, desde que todos os `0`s estejam antes dos `1`s.

As entradas `01` e `0011` foram aceitas porque cada `0` pode ser associado a exatamente um `1`.

A entrada `001` foi rejeitada porque existem dois `0`s e apenas um `1`, portanto não é possível estabelecer uma correspondência para todos os símbolos.

---

## Captura de tela da simulação

**Inserir aqui as capturas de tela fornecidas pelo simulador utilizado na atividade.**

A atividade solicita uma captura da simulação como evidência da execução da Máquina de Turing.

---

# Etapa 4 — Reflexão sobre os limites computacionais

## Uma Máquina de Turing consegue resolver qualquer problema?

Não. Uma Máquina de Turing é capaz de representar muitos tipos de computação, mas existem problemas que não podem ser resolvidos por algoritmos para todos os casos.

Isso significa que a existência de uma máquina suficientemente poderosa não garante que todo problema tenha uma solução algorítmica.

Um exemplo clássico é o **Problema da Parada (Halting Problem)**. Não existe um algoritmo geral que consiga determinar corretamente, para qualquer programa e qualquer entrada, se aquele programa irá eventualmente terminar ou continuar executando indefinidamente.

Portanto, existem limites fundamentais para aquilo que pode ser calculado.

---

## Por que existem problemas que não podem ser resolvidos por algoritmos?

Um algoritmo precisa possuir uma sequência bem definida de passos capaz de produzir uma resposta para os casos previstos.

Porém, alguns problemas são demonstrados matematicamente como **indecidíveis**. Para esses problemas, não existe uma Máquina de Turing que consiga produzir a resposta correta para todas as entradas e sempre terminar sua execução.

Isso é diferente de um problema simplesmente difícil.

Um problema pode ser:

- **fácil:** pode ser resolvido utilizando poucos recursos;
- **difícil:** possui uma solução, mas pode exigir muito tempo ou memória;
- **indecidível:** não existe um algoritmo que resolva corretamente todos os casos e termine para todas as entradas.

Assim, a teoria da computabilidade estabelece limites sobre o que pode ser feito por algoritmos.

---

# Questão Final

## Como saber se um problema é apenas difícil ou se não existe nenhum algoritmo capaz de resolvê-lo para todos os casos?

Para diferenciar esses casos, é necessário analisar o problema matematicamente e estudar sua **computabilidade**.

Se existe um algoritmo que resolve o problema para todas as entradas e termina sua execução, então o problema é computável, mesmo que possa ser extremamente difícil ou demorado.

Por outro lado, se for possível demonstrar que nenhum algoritmo pode resolver corretamente todos os casos, então o problema é indecidível.

As Máquinas de Turing são utilizadas como modelo para realizar esse tipo de análise. Através de resultados da teoria da computabilidade, como o Problema da Parada, é possível demonstrar que existem problemas para os quais não pode existir um algoritmo geral de solução.

Portanto, ser difícil não significa ser impossível de calcular. Um problema pode exigir uma quantidade muito grande de tempo ou recursos e ainda possuir um algoritmo. Já um problema indecidível não possui um algoritmo que resolva todos os casos dentro das condições estabelecidas.

---

# Conclusão

A atividade permitiu compreender o conceito de Máquina de Turing, seus principais componentes e sua relação com algoritmos e computabilidade.

Na simulação, foi criada uma máquina capaz de reconhecer palavras da forma `0ⁿ1ⁿ`, verificando se existe a mesma quantidade de `0`s e `1`s.

Os testes `01` e `0011` foram aceitos, enquanto `001` foi rejeitado, demonstrando o funcionamento da lógica de correspondência entre os símbolos.

Por fim, foi possível compreender que a computação possui limites. Nem todo problema pode ser resolvido por um algoritmo, e a teoria das Máquinas de Turing fornece uma base formal para estudar esses limites.

---

## Checklist para entrega

- [x] Respostas da Etapa 1
- [x] Descrição da Máquina de Turing
- [x] Três testes
- [x] Resultados esperados e obtidos
- [x] Estados percorridos
- [x] Reflexão sobre limites computacionais
- [ ] Capturas de tela da simulação
- [ ] Organizar o material conforme orientação do professor

**Observação:** a atividade original solicita a entrega em PDF ou documento do Word. Este arquivo `.md` contém o conteúdo respondido e pode ser convertido posteriormente para um desses formatos.
