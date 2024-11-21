# Trabalho Prático: Compressão com LZW

## Introdução

Este trabalho tem como objetivo explorar a implementação prática do algoritmo LZW, com foco no uso de estruturas de dados eficientes, como a Trie Compacta, para otimizar o processo de compressão. A abordagem é dividida em duas variantes do algoritmo: uma versão com códigos de comprimento fixo e outra com comprimentos variáveis, ambas projetadas para lidar com diferentes cenários de compressão.

Além disso, a análise prática inclui testes com arquivos de diferentes tamanhos e formatos, avaliando métricas como taxa de compressão, tempo de execução e uso de memória. Por meio dessa implementação, buscamos compreender profundamente os desafios e as vantagens de métodos baseados em manipulação de sequências e suas aplicações em sistemas modernos.

---

## Descrição do Algoritmo

O algoritmo **Lempel-Ziv-Welch (LZW)** é um método de compressão sem perdas baseado no conceito de substituição de padrões repetidos por códigos únicos, armazenados em um dicionário dinâmico. Criado em 1984 como uma extensão das técnicas desenvolvidas por Abraham Lempel e Jacob Ziv, o LZW é amplamente utilizado devido à sua eficiência e simplicidade.

### Funcionamento Básico

1. **Inicialização do Dicionário:**  
   O algoritmo começa com um dicionário inicial contendo todas as combinações possíveis de símbolos únicos do alfabeto da entrada. Por exemplo, para texto ASCII, o dicionário inicial conterá 256 entradas, uma para cada caractere.

2. **Leitura e Compressão:**  
   O algoritmo lê a sequência de entrada caractere por caractere, construindo progressivamente padrões maiores. Se o padrão atual já existe no dicionário, o próximo caractere é adicionado ao padrão. Caso contrário, o padrão anterior é codificado como uma saída numérica, e o novo padrão é adicionado ao dicionário com um código exclusivo.

3. **Saída dos Códigos:**  
   O processo continua até que toda a sequência de entrada seja processada, momento em que o último padrão é codificado e enviado para a saída.

---

### Estruturas de Dados Utilizadas

A eficiência do LZW depende diretamente da estrutura de dados escolhida para representar o dicionário. Neste trabalho, empregamos o uso da **Trie Compacta**, que é adotada para otimizar o armazenamento e a pesquisa de padrões, especialmente em cenários onde o dicionário cresce rapidamente.

---

### Variações Implementadas

1. **Comprimento Fixo dos Códigos:**
   - Os códigos gerados têm todos o mesmo comprimento (por exemplo, 12 bits).
   - **Vantagens:**
     - Simplicidade de implementação.
     - Adequado para sistemas onde tamanhos fixos são necessários.
   - **Desvantagens:**
     - Ineficiente quando a tabela de códigos cresce rapidamente.
     - Pode desperdiçar espaço com códigos menores no início.

2. **Comprimento Variável dos Códigos:**
   - O comprimento dos códigos varia dinamicamente com o crescimento da tabela de dicionários.
   - **Vantagens:**
     - Utiliza códigos mais curtos inicialmente.
     - Melhor utilização do espaço de dicionário.
   - **Desvantagens:**
     - Mais complexo de implementar.
     - Pode ser mais lento devido à necessidade de ajustes.

---

### Diferenças Principais

| **Aspecto**             | **Comprimento Fixo**                  | **Comprimento Variável**           |
|--------------------------|---------------------------------------|-------------------------------------|
| **Tamanho dos Códigos**  | Não muda, é fixado previamente.       | Cresce dinamicamente com a tabela. |
| **Eficiência Inicial**   | Menor (códigos longos desde o início).| Maior (códigos curtos no começo).  |
| **Complexidade**         | Mais simples de implementar.          | Requer maior controle e ajustes.   |
| **Uso de Memória**       | Pode desperdiçar memória.             | Melhor utilização de memória.      |

---

### Etapas do Algoritmo

1. **Compressão:** Recebe uma sequência de entrada e retorna uma sequência comprimida de códigos.
2. **Descompressão:** Reconstrói a sequência original a partir da sequência comprimida, utilizando o mesmo dicionário gerado durante a compressão.

---

### Exemplificação

Considere a sequência de entrada: `"ABABABA"`. O funcionamento do LZW é ilustrado na tabela a seguir:

| **Passo** | **Padrão Atual** | **Próximo Símbolo** | **Código Emitido** | **Atualização do Dicionário** |
|-----------|------------------|---------------------|---------------------|-------------------------------|
| 1         | A                | B                   | 65 (ASCII A)        | AB → 256                     |
| 2         | B                | A                   | 66 (ASCII B)        | BA → 257                     |
| 3         | AB               | A                   | 256                 | ABA → 258                    |
| 4         | BA               | A                   | 257                 | BAA → 259                    |

---

## Árvore de Prefixos (Trie Compacta)

A **Trie Compacta** é uma variação otimizada da estrutura Trie tradicional, projetada para reduzir redundâncias ao condensar nós que possuem apenas um único filho. Essa compactação é especialmente útil em aplicações que lidam com grandes volumes de strings com prefixos comuns, como o algoritmo LZW.

### Características da Trie Compacta

- Os nós armazenam apenas o **prefixo comum** das chaves em sua subárvore.
- Somente os **nós folha** armazenam os valores associados às chaves.
- A compactação é realizada ao unir caminhos lineares (nós com um único filho) em um único nó.

---

### Operações na Trie Compacta

1. **Busca:**  
   A busca segue o mesmo princípio da Trie tradicional, com manipulação de prefixos compactados.
   
2. **Inserção:**  
   A inserção envolve criar nós internos para gerenciar prefixos compartilhados e subprefixos.
   
3. **Remoção:**  
   A remoção reorganiza os nós para manter a compactação.

