# Trabalho Prático: Compressão com LZW

## Colaboradores

<table>
  <tr>
    <td align="center">
      <a href="#">
        <sub>
          <b><a href="https://github.com/LrcSantos">Lucas Rafael Costa Santos</a></b>
        </sub>
      </a>
    </td>
     <td align="center">
      <a href="#">
        <sub>
          <b><a href="https://github.com/luccaamp">Lucca Alvarenga de Magalhães Pinto</a></b>
        </sub>
      </a>
    </td>
  </tr>
</table>

Veja o relatório aqui:
https://lrcsantos.github.io/Site_TP1_ALG2/

# Como executar o programa no terminal

## Passo a Passo

1. Navegue até o diretório raiz do projeto `TP1_ALG2` com o seguinte comando no terminal:

    ```bash
    cd ./TP1_ALG2
    ```

2. O arquivo principal do programa é `lzw.py`, localizado na pasta `src`.

3. Use os argumentos abaixo para executar o programa:

### Argumentos Obrigatórios

- **Tipo de algoritmo:**
  - `f`: Algoritmo LZW de tamanho fixo
  - `v`: Algoritmo LZW de tamanho variável

- **Modo de operação:**
  - `c`: Comprimir o arquivo
  - `d`: Descomprimir o arquivo

### Argumentos Opcionais

- **Número máximo de bits:**
  - `num`: Especifica o número máximo de bits para codificação/decodificação (padrão: 12 bits).

### Formato Geral do Comando

```bash
python3 ./src/lzw.py <fixo/variável> <compressão/descompressão> <caminho arquivo para comprimir> <caminho do arquivo de saída> [max_bits]
```

4. Após a execução, o programa:
  - Salvará o arquivo de saída na pasta `./output`
  - Escreverá os resultados de estatísticas na pasta `./stats `

##  Exemplos Práticos

 ### Exemplo: MobyDick:
 
 ```bash
cd ./TP1_ALG2
python3 ./src/lzw.py f c ./input/txt/MobyDick.txt ./output/MobyDick_comprimido.bin 16
python3 ./src/lzw.py f d ./output/MobyDick_comprimido.bin ./output/MobyDick_descomprimido.txt 16
```

### Exemplo: jetplane

 ```bash
cd ./TP1_ALG2
python3 ./src/lzw.py f c ./input/tiff/jetplane.tiff ./output/jetplane_comprimido.bin
 python3 ./src/lzw.py f d ./output/jetplane_comprimido.bin ./output/jetplane_descomprimido.tiff
```

### Exemplo: Frankenstein
 ```bash
cd ./TP1_ALG2
python3 ./src/lzw.py v c ./input/txt/Frankenstein.txt ./output/Frankenstein_comprimido.bin
python3 ./src/lzw.py v d ./output/Frankenstein_comprimido.bin ./output/Frankenstein_descomprimido.txt
```
