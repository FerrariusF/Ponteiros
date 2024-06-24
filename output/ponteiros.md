<p align="center">
    <img width="100" src=".github/assets/capaCPP.png">
</p>

## Introdução

Os ponteiros são um conteito poderoso e comumente temidos para quem está aprendendo C++. Entretanto, são fundamentais para realizar controle do uso de memória, otimização de desempenho e na manipulação de recursos de harware diretamente. Neste artigo, vamos descomplicar ponteiros, explicando o que são, quando e como usa-los, além de dar exemplos práticos dos conceitos apresentados, mostrando suas vantagens.


## O que são ponteiros?

Em C++, um ponteiro é uma variável que aponta para um endereço de mmemória de outra variável. Dessa forma, o programador pode manipular diretamente a memória, permitindo a construção de programas mais eficientes. 

A declaração de um ponteiro ocorre como a declçaração de uma variável comum, entretando apresenta um ("*") antes do nome do ponteiro. E para referenciar um endereço de uma variável é necessário colocar o operador de referencia ("&") antes da variável, como no axemplo a seguir.

```cpp
#include <iostream>
using namespace std;

int main() {
    int var = 42;       // Variável inteira
    int * ptr = &var;   // Ponteiro para o endereço da variável 'var'

    cout << "Valor de var: " << var << endl;            // Valor de 'var', 42
    cout << "Endereço de var: " << &var << endl;        // Endereço de 'var'
    cout << "Valor do ponteiro ptr: " << ptr << endl;   // Endereço de 'var'
    cout << "Valor apontado por ptr: " << *ptr << endl; // Valor de 'var', 42
}
```

Neste exemplo 'ptr' é um ponteiro que guarda o endereço da variável 'var'. A partir da referencia do ponteiro é possivel acessar e modificar o valor da variável em questão diretamente na memória.


## Comparação de código com e sem ponteiros

A seguir tempos o mesmo código, mas em um utilizamos ponteiros para realizar a alteração do valor da variável e veremos como isso impacta em seu valor final.

```cpp
#include <iostream>
using namespace std;

void increment(int var) {
    var++;              // Incrementa o valor da variável
}

int main() {
    int num = 10;
    increment(num);    // Passa o endereço da variável 'num' para a função
    cout << "Valor de num após função de incremento: " << num << endl;
}
```

Ao final da execução a variável 'num' vale 10. Isso acontece pois, ao chamar a função 'increment' passando o valor da variável, é alocado na memória o espaço de um inteiro, esse espaço recebe o valor da variável var, e dentro da função essa variável nova é alterada, não interferindo no valor original da variável 'num'. Ao retornar para a main, 'num' é impressa inalterada.


```cpp
#include <iostream>
using namespace std;

void increment(int* ptr) {
    (*ptr)++;           // Incrementa o valor da variável cujo endereço foi apontado por ptr
}

int main() {
    int num = 10;
    increment(&num);    // Passa o endereço da variável 'num' para a função
    cout << "Valor de num após função de incremento: " << num << endl;
}
```

Ao final da execução a variável 'num' vale 11. Isso aconteceu pois, ao chamar a função passando o endereço de 'num', a própria 'num' pode ser modificada a partir de seu endereço usando o ponteiro, permitindo que seu valor fosse incrementado. 


## Quando usar ponteiros?

Os usos de ponteiros são diversos, alguns deles são:

1. Passagem de Argumentos por Referência: Permite que funções modifiquem variáveis fora de seu escopo local.
2. Manipulação Dinâmica de Memória: Usado para alocar e desalocar memória durante a execução do programa.
3. Estruturas de Dados Dinâmicas: Como listas ligadas, árvores e grafos.
4. Interação com Hardware e APIs de Baixo Nível: Ponteiros são essenciais para trabalhar com endereços de memória específicos e dispositivos de hardware.

## Como usar?

### Passagem de Argumentos por Referência

Podemos modificar uma variável local em outra função ao passar um ponteiro que aponte para o local de memória da variável como argumento para a função.

```cpp
#include <iostream>
using namespace std;

void increment(int* ptr) {
    (*ptr)++;           // Incrementa o valor da variável cujo endereço foi apontado por ptr
}

int main() {
    int num = 10;
    increment(&num);    // Passa o endereço da variável 'num' para a função
    cout << "Valor de num após incremento: " << num << endl;
}
```

### Manipulação Dinâmica de Memória

Com os comandos 'new' e 'delete' você pode controlar diretamente a alocação e desalocação de memória.

```cpp
#include <iostream>
using namespace std;

int main() {
    int* ptr = new int;  // Aloca memória para um inteiro
    *ptr = 100;          // Atribui um valor ao espaço alocado
    cout << "Valor de *ptr: " << *ptr << endl;
    delete ptr;          // Desaloca a memória
}
```

### Estruturas de dados dinâmicas

Podemos utilizar ponteiros para montar e controlar estruturas de dados dinâmicas, vou mnostrar a seguir um exemplo de como montar uma estrutura de árvore binária. A arvore binária é uma estrutura formada por estruturas menores chamadas nós, que possuem no máximo dois filhos, isto é, outros dois nós, que são referenciados como filho da esquerda e filho da direita. Para referenciar cada filho são utilizados ponteiros que apontam para essas estruturas.

```cpp
#include <iostream>

// Definição da estrutura do nó
struct Node {
    int data;                   // Conteúdo do nó
    Node* left;                 // Ponteiro para nó da esquerda
    Node* right;                // Ponteiro para nó da direita
};

// Função para criar um novo nó
Node* newNode(int data) {
    Node* node = new Node();    // Aloca memória para nó e usa ponteiro para referenciar a estrutura
    node->data = data;          // Aramazena conteúdo do nó na variável data do nó apontado pelo ponteiro
    node->left = nullptr;       // Atribui valor nulo ao filho da esquerda do nó apontado pelo ponteiro
    node->right = nullptr;      // Atribui valor nulo ao filho da direita do nó apontado pelo ponteiro
    return node;                // Retorna o ponteiro que referencia a estrutura de nó montada
}

// Função para percorrer a árvore binária em ordem (in-order traversal)
void inOrder(Node* root) {
    if (root != nullptr) {              // Se nó não tfor nulo
        inOrder(root->left);            // Mostra primeiro o conteúdo de seu filho da esquerda
        std::cout << root->data << " "; // Mostra seu conteúdo
        inOrder(root->right);           // Mostra o conteúdo de seu filho da direita
    }
}

int main() {
    Node* root = newNode(1);        // Cria nó princiapal chamdo root
    root->left = newNode(2);        // Cria nó e o torna filho da esquerda de root
    root->right = newNode(3);       // Cria nó e o torna filho da direita de root
    root->left->left = newNode(4);  // Cria nó e o torna filho da esquerda do filho da esquerda de root
    root->left->right = newNode(5); // Cria nó e o torna filho da direita do filho da esquerda de root

    std::cout << "Percurso in-order da árvore binária: ";
    inOrder(root);                  // Percorrer nós da arvore binária mostrando seus conteúdos
}
```

### Interação com Hardware e APIs de Baixo Nível: Ponteiros são essenciais para trabalhar com endereços de memória específicos e dispositivos de hardware.

Ponteiros podem ser usados para acessar diretamente registradores de hardware, simulando uma interação de baixo nível. Essa técnica é usada para programação de sistemas embarcados e drivers de dispositivo, esse controle direto sobre a memória e os recursos de hardware é essencial para o funcionamento eficiente e correto do software.

Imagine que estejamos trabalhando com um hardware controlador de portas paralelas que permite ler e escrever em seus registradores de controle. Podemos usar ponteiros para acessar esses registradores.

```cpp
#include <iostream>
#include <cstdint>

// Simulação de endereços de memória dos registradores
#define DATA_REGISTER 0x1000
#define CONTROL_REGISTER 0x1004

// Função para simular a escrita em um registrador de hardware, recebe endereço do registrador e o valor que deve ser escrito nele
void writeRegister(uintptr_t address, uint8_t value) {      
    uint8_t* reg = reinterpret_cast<uint8_t*>(address);     // Converte o endereço para uint8_t e faz ponteiro apontar para o endereço
    *reg = value;                                           // O endereço do registrador apontado elo ponteiro recebe o valor
    std::cout << "Escreveu " << static_cast<int>(value) << " no endereço " << std::hex << address << std::endl;
}

// Função para simular a leitura de um registrador de hardware, recebe endereço do registrador
uint8_t readRegister(uintptr_t address) {
    uint8_t* reg = reinterpret_cast<uint8_t*>(address);     // Converte o endereço para uint8_t e faz ponteiro apontar para o endereço
    uint8_t value = *reg;                                   // Pega o conteúdo do endereço apontado e guarda na variável valor
    std::cout << "Leu " << static_cast<int>(value) << " do endereço " << std::hex << address << std::endl;  
    return value;                                           // Retorna conteúdo do registrador
}

int main() {
    // Escrevendo em registradores de hardware simulados
    writeRegister(DATA_REGISTER, 0xAB);
    writeRegister(CONTROL_REGISTER, 0x12);

    // Lendo dos registradores de hardware simulados
    uint8_t dataValue = readRegister(DATA_REGISTER);
    uint8_t controlValue = readRegister(CONTROL_REGISTER);

    // Usando os valores lidos
    std::cout << "Valor do registrador de dados: " << std::hex << static_cast<int>(dataValue) << std::endl;
    std::cout << "Valor do registrador de controle: " << std::hex << static_cast<int>(controlValue) << std::endl;
}

```


## Melhorando códigos com ponteiros

O uso de ponteiros no seu código pode trazer eficiência e desempenho ao permitir manipulações diretas da memória, sem ter que realizar cópias desnecessárias de grandes estruturas de dados, que poderiam trazer grande aumento de complexidade espacial do seu código. Além disso, ponteiros facilitam o uso de estruturas de dados complexas de com manipulação dinâmica de memória, permitindo maior flexibilidade para sua implementação.


## Conclusão

Ponteiros são uma ferramentas poderosa para o programador, que permitem que melhore seu controle sobre a memória ao conseguir trabalhar de maneira direta com ela, além de criar e manipular estruturas de dados e algorítmos eficientes. O funcionamento de ponteiros além de aprofundar o conhecimento de hardware do desenvolvedor também é indispensável no desenvolvimento de softwares de melhor performance.

