# Árvores Binárias e Recursividade

## 📚 Objetivos de Aprendizagem

Ao concluir esta atividade, você deverá ser capaz de:
* Compreender e aplicar o conceito de **recursividade** em programação
* Entender a estrutura de dados **Árvore Binária**
* Implementar funções recursivas para manipular árvores
* Analisar e compreender código recursivo existente

---

## 🔄 Parte 1: Recursividade

### O que é Recursividade?

**Recursividade** é uma técnica de programação onde uma função chama a si mesma para resolver um problema. É como olhar para um espelho refletido em outro espelho - cada reflexo é uma versão menor do problema original.

#### Componentes de uma Função Recursiva

Toda função recursiva precisa ter:

1. **Caso Base**: A condição que para a recursão (evita loop infinito)
2. **Caso Recursivo**: A chamada da função para uma versão menor do problema
3. **Progresso**: Cada chamada recursiva deve aproximar-se do caso base

### Exemplo 1: Fatorial

```cpp
int fatorial(int n) {
    // Caso base: fatorial de 0 ou 1 é 1
    if (n <= 1) {
        return 1;
    }
    
    // Caso recursivo: n! = n * (n-1)!
    return n * fatorial(n - 1);
}
```

**Como funciona:**
- `fatorial(5)` = 5 × `fatorial(4)`
- `fatorial(4)` = 4 × `fatorial(3)`
- `fatorial(3)` = 3 × `fatorial(2)`
- `fatorial(2)` = 2 × `fatorial(1)`
- `fatorial(1)` = 1 (caso base)
- Resultado: 5 × 4 × 3 × 2 × 1 = 120

### Exemplo 2: Soma de Array

```cpp
int somaArray(int arr[], int tamanho) {
    // Caso base: array vazio
    if (tamanho == 0) {
        return 0;
    }
    
    // Caso recursivo: primeiro elemento + soma do resto
    return arr[0] + somaArray(arr + 1, tamanho - 1);
}
```

### Exemplo 3: Contagem Regressiva

```cpp
void contagemRegressiva(int n) {
    // Caso base
    if (n == 0) {
        cout << "Fim!" << endl;
        return;
    }
    
    // Imprime o número atual
    cout << n << endl;
    
    // Caso recursivo
    contagemRegressiva(n - 1);
}
```

---

## 🌳 Parte 2: Árvores e Árvores Binárias

### O que é uma Árvore?

Uma **árvore** é uma estrutura de dados hierárquica composta por **nós** conectados por **arestas**. Ao contrário de estruturas lineares (arrays, listas), as árvores representam relações hierárquicas.

#### Terminologia Importante

```
        50
       /  \
      30   70
     / \   / \
   20  40 60  80
```

- **Raiz**: O nó superior (50 no exemplo)
- **Nó Pai**: Nó que tem filhos
- **Nó Filho**: Nó descendente de outro nó
- **Nó Folha**: Nó sem filhos (20, 40, 60, 80)
- **Subárvore**: Uma árvore formada por um nó e todos seus descendentes
- **Altura**: Número de arestas do caminho mais longo da raiz até uma folha
- **Nível**: Distância de um nó até a raiz

### Árvore Binária

Uma **árvore binária** é uma árvore onde cada nó tem **no máximo dois filhos**: filho esquerdo e filho direito.

### Árvore Binária de Busca (BST)

Uma **Árvore Binária de Busca** (Binary Search Tree) é uma árvore binária com uma propriedade especial:

> Para cada nó:
> - Todos os valores na **subárvore esquerda** são **menores** que o valor do nó
> - Todos os valores na **subárvore direita** são **maiores** que o valor do nó

**Vantagem**: Permite busca eficiente (similar à busca binária)

#### Exemplo de BST:

```
        50
       /  \
      30   70
     / \   / \
   20  40 60  80
```

- Esquerda de 50: todos < 50 (30, 20, 40)
- Direita de 50: todos > 50 (70, 60, 80)

---

## 💻 Parte 3: Análise do Código Fornecido

### Estrutura do Nó

```cpp
struct NO {
    int valor;      // Dado armazenado no nó
    NO* esq;        // Ponteiro para filho esquerdo
    NO* dir;        // Ponteiro para filho direito
};
```

Cada nó contém:
- Um **valor inteiro**
- Dois **ponteiros** para os filhos (esquerdo e direito)

### Função 1: `criaNO` (Não Recursiva)

```cpp
NO* criaNO(int valor) {
    NO* novo = (NO*)malloc(sizeof(NO));
    if (novo == NULL) {
        return NULL;
    }
    
    novo->valor = valor;
    novo->esq = NULL;
    novo->dir = NULL;
    
    return novo;
}
```

**O que faz:**
- Aloca memória para um novo nó
- Inicializa o valor
- Define ambos os filhos como NULL (nó folha)
- Retorna o ponteiro para o novo nó

### Função 2: `insereArvore` (RECURSIVA) ⭐

```cpp
NO* insereArvore(NO* no, int valor) {
    // Caso 1: Inserir à esquerda (valor menor)
    if (no->valor > valor && no->esq == NULL) {
        no->esq = criaNO(valor);
        return no->esq;
    }
    // Caso 2: Inserir à direita (valor maior)
    else if (no->valor < valor && no->dir == NULL) {
        no->dir = criaNO(valor);
        return no->dir;
    }
    // Caso 3: Continuar buscando à esquerda (RECURSÃO)
    else if (no->valor > valor) {
        return insereArvore(no->esq, valor);
    }
    // Caso 4: Continuar buscando à direita (RECURSÃO)
    else if (no->valor < valor) {
        return insereArvore(no->dir, valor);
    }
    // Caso 5: Valor duplicado (não insere)
    else {
        return NULL;
    }
}
```

**Como funciona:**

1. **Compara** o valor a inserir com o valor do nó atual
2. Se **menor** e não há filho esquerdo → insere à esquerda
3. Se **maior** e não há filho direito → insere à direita
4. Se há filho → **chama recursivamente** para esse filho
5. **Caso base**: quando encontra uma posição NULL para inserir

**Exemplo de inserção:**
```
Inserir 25 na árvore:
    50
   /  \
  30   70

Passo 1: 25 < 50 → vai para esquerda
Passo 2: 25 < 30 → vai para esquerda de 30
Passo 3: esq de 30 é NULL → insere aqui!

Resultado:
    50
   /  \
  30   70
 /
25
```

### Função 3: `elementosArvore` (RECURSIVA) ⭐

```cpp
int elementosArvore(NO* no) {
    // Caso base: árvore vazia
    if (no == NULL) {
        return 0;
    }
    
    // Caso recursivo: 1 (nó atual) + elementos esquerda + elementos direita
    return 1 + elementosArvore(no->esq) + elementosArvore(no->dir);
}
```

**Como funciona:**

- **Caso base**: Se o nó é NULL (árvore vazia), retorna 0
- **Caso recursivo**: Conta 1 (nó atual) + elementos da subárvore esquerda + elementos da subárvore direita

**Visualização:**
```
        50
       /  \
      30   70
     /
   20

elementosArvore(50) = 1 + elementosArvore(30) + elementosArvore(70)
elementosArvore(30) = 1 + elementosArvore(20) + elementosArvore(NULL)
elementosArvore(20) = 1 + elementosArvore(NULL) + elementosArvore(NULL)
elementosArvore(NULL) = 0

Resultado: 1 + (1 + (1 + 0 + 0) + 0) + (1 + 0 + 0) = 4 elementos
```

---

## 🎯 Parte 4: Sua Missão

### Atividade Proposta

Você deve implementar a função `exibirElementosArvore` que está vazia no código fornecido.

```cpp
void exibirElementosArvore(NO* no) {
    // SEU CÓDIGO AQUI
}
```

### Requisitos

A função deve:
- **Exibir todos os elementos** da árvore
- Usar **recursividade**
- Percorrer a árvore de forma ordenada

### Tipos de Percurso em Árvores

Existem três formas principais de percorrer uma árvore binária:

#### 1. **Percurso em Ordem (In-Order)**
Esquerda → Raiz → Direita

```
Árvore:     50
           /  \
          30   70
         / \
        20  40

Saída: 20 30 40 50 70
```
✅ **Resulta em ordem crescente em uma BST!**

#### 2. **Percurso Pré-Ordem (Pre-Order)**
Raiz → Esquerda → Direita

```
Saída: 50 30 20 40 70
```

#### 3. **Percurso Pós-Ordem (Post-Order)**
Esquerda → Direita → Raiz

```
Saída: 20 40 30 70 50
```

### 💡 Dicas para Resolver

1. **Escolha um tipo de percurso**: Para uma BST, o percurso em ordem mostra os elementos ordenados

2. **Pense recursivamente**:
   - Qual é o **caso base**? (Quando parar?)
   - O que fazer com o **nó atual**?
   - Como processar as **subárvores**?

3. **Estrutura sugerida** para percurso em ordem:
   ```cpp
   void exibirElementosArvore(NO* no) {
       // Caso base: ?
       
       // Processar subárvore esquerda
       
       // Processar nó atual (exibir valor)
       
       // Processar subárvore direita
   }
   ```

4. **Compare com `elementosArvore`**: A estrutura é similar, mas ao invés de somar, você vai exibir

5. **Teste sua implementação**: Insira vários valores e verifique se aparecem em ordem

### Exemplo de Saída Esperada

```
Digite elementos: 50, 30, 70, 20, 40, 60, 80

Ao exibir (em ordem):
20 30 40 50 60 70 80
```

---

## 📤 Entrega

### Como Entregar sua Atividade

1. Faça um **fork** deste repositório no GitHub
2. Clone o fork para sua máquina local
3. Implemente a função `exibirElementosArvore` no arquivo `ArvoreBinaria.cpp`
4. Teste seu código para garantir que funciona corretamente
5. Faça commit e push das suas alterações
6. **Entregue via Microsoft Teams**:
   - Acesse a tarefa correspondente no Teams
   - Cole a **URL do seu fork** no GitHub
   - Exemplo: `https://github.com/seu-usuario/repo-arvore-binaria`

### Checklist de Entrega

- [ ] Fork do repositório criado
- [ ] Função `exibirElementosArvore` implementada
- [ ] Código testado e funcionando
- [ ] Commit realizado com mensagem descritiva
- [ ] URL do fork entregue no Teams

---

## 📖 Referências Adicionais para Estudo

### Recursividade

- [Recursividade - GeeksforGeeks](https://www.geeksforgeeks.org/introduction-to-recursion-data-structure-and-algorithm-tutorials/)
- [Visualizador de Recursão](https://www.recursionvisualizer.com/)
- [Recursão em C++ - Programiz](https://www.programiz.com/cpp-programming/recursion)

### Árvores Binárias

- [Árvores Binárias - Estruturas de Dados Descomplicada](https://www.estruturas.ufsc.br/)
- [Binary Search Tree Visualizer](https://visualgo.net/en/bst) - Ferramenta interativa para visualizar operações em BST
- [Árvores - USP](https://www.ime.usp.br/~pf/algoritmos/aulas/bintree.html)
- [Tree Traversals - GeeksforGeeks](https://www.geeksforgeeks.org/tree-traversals-inorder-preorder-and-postorder/)

### Vídeos Recomendados

- [Recursividade - Curso em Vídeo (YouTube)](https://www.youtube.com/@CursoemVideo)
- [Árvores Binárias - Programação Descomplicada](https://www.youtube.com/c/Programa%C3%A7%C3%A3oDescomplicada)

### Livros

- **Estruturas de Dados e Seus Algoritmos** - Szwarcfiter e Markenzon
- **Algoritmos: Teoria e Prática** - Cormen et al. (Capítulo sobre Árvores)
- **Estruturas de Dados Usando C** - Tenenbaum, Langsam e Augenstein

### Prática Online

- [HackerRank - Tree Data Structure](https://www.hackerrank.com/domains/data-structures?filters%5Bsubdomains%5D%5B%5D=trees)
- [LeetCode - Binary Tree Problems](https://leetcode.com/tag/tree/)
- [Beecrowd (antigo URI)](https://www.beecrowd.com.br/) - Problemas de estruturas de dados

---

## 🤔 Perguntas Frequentes

**P: Por que usar recursividade em árvores?**  
R: Árvores são estruturas naturalmente recursivas. Cada subárvore é também uma árvore, tornando a recursão a abordagem mais natural e elegante.

**P: E se eu não conseguir implementar recursivamente?**  
R: Tente primeiro no papel. Desenhe a árvore e simule a execução passo a passo. A recursão em árvores segue sempre o mesmo padrão.

**P: Posso usar outro tipo de percurso?**  
R: Sim! Você pode implementar qualquer tipo de percurso. O importante é usar recursividade.

**P: Como sei se meu código está correto?**  
R: Teste com vários exemplos. Para uma BST com percurso em ordem, os valores devem aparecer ordenados crescentemente.

---

## 🎓 Bons Estudos!

Lembre-se: a prática leva à perfeição. Não desanime se não conseguir de primeira. Recursividade é um conceito que requer tempo para ser internalizado.

**Dica de Ouro**: Sempre que estiver em dúvida, desenhe a árvore no papel e simule a execução passo a passo!

---



**Avaliação:** Será considerada a corretude da implementação, uso adequado de recursão e boas práticas de programação.
