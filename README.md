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

### Operações na Trie Compacta

#### 1. Busca

A busca em uma Trie Compacta segue o mesmo princípio da Trie tradicional, com a adição de manipulação de prefixos compactados:

- Durante a busca, o prefixo acumulado é atualizado conforme se percorre a árvore.
- Para decidir qual ligação seguir, verifica-se o próximo símbolo da chave **pós-prefixo**.
- Caso um nó folha seja encontrado antes de processar todos os símbolos da chave, a chave não está presente.

#### 2. Inserção

O processo de inserção envolve a criação de nós internos para gerenciar prefixos compartilhados e subprefixos. Os passos são:

1. Se a árvore estiver vazia, criar um nó folha com o prefixo igual à chave.
2. Buscar o elemento na árvore para localizar o ponto de inserção.
3. Se houver divergência entre a chave e o prefixo:
   - Criar um nó interno para armazenar o **subprefixo comum** entre a chave e o prefixo do nó encontrado.
   - Ajustar o prefixo do nó existente, removendo o subprefixo comum, e anexá-lo como filho do novo nó interno.
   - Criar um nó folha para o sufixo restante da chave e inseri-lo no novo nó interno.

#### 3. Remoção

A remoção de uma chave envolve a reorganização dos nós para manter a compactação:

- Localizar o nó folha que contém a chave a ser removida.
- Se o nó for encontrado, removê-lo.
- Se o pai do nó removido ficar com apenas um filho:
  - Concatenar o prefixo do pai com o prefixo do filho.
  - Remover o nó filho redundante.

### Exemplo: Construção da Trie Compacta

A frase **"She sells sea shells by the sea"** está sendo inserida na Trie Compacta passo a passo.

### Implementação no Algoritmo LZW

Na implementação do LZW, a árvore de prefixos desempenha o papel de dicionário dinâmico. Cada nó da Trie armazena:

- Um **prefixo parcial** da sequência.
- Um valor associado (código), que representa a sequência comprimida correspondente.

#### Funcionamento

1. **Inserção de Padrões:** Durante a compressão, quando um novo padrão é identificado, ele é adicionado à Trie com um código único. A compactação de caminhos é usada para evitar a duplicação de prefixos.
2. **Busca de Padrões:** Na leitura da entrada, a Trie é percorrida para encontrar o maior padrão já existente no dicionário. Isso é feito de forma eficiente graças à estrutura hierárquica.
3. **Compactação Dinâmica:** Sempre que uma nova sequência é adicionada, caminhos redundantes são simplificados, reduzindo o número de nós e otimizando a memória usada.

### Vantagens da Trie Compacta

- **Desempenho em busca e inserção:** Operações realizadas em tempo proporcional ao comprimento da string, em vez do número total de padrões armazenados.
- **Escalabilidade:** Suporta o crescimento rápido do dicionário sem perda significativa de desempenho.
- **Redução de redundância:** Compacta prefixos repetidos, reduzindo o consumo de memória em cenários onde padrões compartilham sequências iniciais comuns.

### Comparação com Outras Estruturas de Dados

| Estrutura de Dados | Tempo de Busca | Tempo de Inserção | Uso de Memória          |
|---------------------|----------------|-------------------|-------------------------|
| Tabela Hash         | O(1)           | O(1)              | Alto (colisões)         |
| Trie Simples        | O(m)           | O(m)              | Médio                   |
| **Trie Compacta**   | **O(m)**       | **O(m)**          | **Baixo (otimizado)**   |

### Visualização da Trie Compacta

```text
Root
 ├── A -> 65
 │    ├── B -> 256
 │         └── A -> 258
 └── B -> 66
      ├── A -> 257
```
## Nesse exemplo:
- Cada nó representa um padrão ou prefixo.
- O valor no nó é o código associado no dicionário LZW.

---

## Implementação

A implementação do algoritmo LZW foi realizada utilizando Python, com foco na eficiência e clareza do código. O algoritmo foi dividido em duas etapas principais: **compressão** e **descompressão**, ambas baseadas no uso de uma **Trie Compacta** como dicionário dinâmico.

### Estrutura Geral

1. **Leitura da Entrada:**  
   A entrada é processada como uma sequência de caracteres (ou bytes), a partir de um arquivo ou string.
   
2. **Compressão:**  
   O texto de entrada é percorrido caractere por caractere para identificar os maiores padrões presentes no dicionário. Caso um padrão não seja encontrado, ele é adicionado ao dicionário com um novo código, e o código do maior padrão encontrado é registrado na saída.
   
3. **Descompressão:**  
   Na etapa de descompressão, os códigos gerados durante a compressão são utilizados para reconstruir a sequência original. O dicionário é reconstruído dinamicamente conforme os padrões são encontrados.
   
4. **Gerenciamento do Dicionário:**  
   O dicionário é implementado como uma Trie Compacta, que permite buscar, adicionar e gerenciar padrões de maneira eficiente.

### Fluxo da Compressão

1. Inicialização do dicionário com os padrões básicos (todos os caracteres ASCII ou outro conjunto inicial).
2. Percorrendo a sequência de entrada, o maior padrão já presente no dicionário é identificado.
3. O código correspondente ao padrão encontrado é adicionado à saída.
4. O novo padrão (padrão atual + próximo caractere) é inserido no dicionário com um código único.
5. O processo é repetido até que toda a entrada seja processada.

### Fluxo da Descompressão

1. Inicialização do dicionário com os padrões básicos, similar à compressão.
2. Para cada código recebido na entrada comprimida:
   - O padrão correspondente é identificado e adicionado à saída.
   - O novo padrão (último padrão + primeiro caractere do padrão atual) é inserido no dicionário.
3. O processo continua até que todos os códigos sejam decodificados.

### Resultados

- **Compressão eficiente:** O algoritmo reduz significativamente o tamanho da entrada para diferentes tipos de arquivos com padrões repetidos.
- **Descompressão exata:** A reconstrução da sequência original é realizada sem perdas, validando a integridade do método.

---

## Estatísticas e Testes

Foram realizados testes com diferentes tipos de arquivos, incluindo texto, gifs e imagens. As estatísticas geradas incluem:

- Tempo total de execução.
- Tamanho original do arquivo.
- Tamanho do arquivo comprimido.
- Taxa de compressão.
- Crescimento do Dicionário.
- Dados Processados ao Longo do Tempo.

Os resultados mostraram que a versão com tamanho variável obteve melhor eficiência em compressão para arquivos maiores, enquanto a versão fixa foi mais rápida em casos menores.

---

## Conclusão

A implementação do algoritmo LZW utilizando uma Trie Compacta demonstrou a eficiência desse método em cenários de compressão sem perdas, especialmente na manipulação de padrões repetidos. 

A comparação entre as variações de comprimento fixo e variável dos códigos evidenciou:
- Vantagens e desvantagens de cada abordagem.
- O equilíbrio necessário entre simplicidade, eficiência de memória e desempenho.

Os testes realizados confirmaram que a Trie Compacta é uma escolha adequada para representar o dicionário dinâmico, garantindo buscas e inserções rápidas, além de uma utilização otimizada da memória. Esses resultados validam a eficácia do LZW em diversos formatos de arquivos e oferecem insights sobre o impacto de diferentes configurações no desempenho do algoritmo, contribuindo para aplicações futuras em sistemas modernos de compressão de dados.

---

## Referências

- [LZW Compression - MIT Handout](https://web.mit.edu/6.02/www/s2012/handouts/3.pdf)
- [Data Compression - David Salomon](https://www.davidsalomon.name/DC4advertis/DComp4Ad.html)
- [Compressão sem perdas - Codificação LZW](http://multimedia.ufp.pt/codecs/compressao-sem-perdas/codificacao-baseada-em-dicionarios/lzw/)
- [LZW Compression Technique - GeeksforGeeks](https://www.geeksforgeeks.org/lzw-lempel-ziv-welch-compression-technique/)

---

&copy; 2024 Algoritmos II - Lucas Rafael Costa Santos | Lucca Alvarenga de Magalhães Pinto
