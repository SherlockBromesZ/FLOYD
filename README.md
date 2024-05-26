# Algoritmo de Floyd-Warshall em C++

## Introdução

O algoritmo de Floyd-Warshall é uma técnica de programação dinâmica usada para encontrar os caminhos mais curtos entre todos os pares de nós em um grafo ponderado. Este método é eficaz para grafos com pesos positivos ou negativos (mas sem ciclos negativos), e é útil em aplicações que requerem a distância mínima entre cada par de nós.

Este algoritmo tem uma complexidade de tempo de \(O(n^3)\), onde \(n\) é o número de nós no grafo, tornando-o adequado para grafos com até algumas centenas de nós.

## Problema

Dado um grafo com `n` cidades (nós) e `m` estradas (arestas) bidirecionais, cada uma com um comprimento específico (peso), você precisa responder a `q` consultas. Cada consulta pede a distância mais curta entre dois dados nós (cidades), usando o algoritmo de Floyd-Warshall.

## Descrição do Código

O código abaixo implementa o algoritmo de Floyd-Warshall em C++ para resolver o problema descrito. O código lê o número de cidades, estradas e consultas, e depois processa as consultas para determinar o caminho mais curto entre pares de cidades.

```cpp
#include<bits/stdc++.h>
using namespace std;

const int maxn = 510;  // Ajustado conforme as restrições
const long long INF = 1e18;  // Valor grande seguro para evitar overflow

int n, m, q;
long long dist[maxn][maxn];

void floyd_warshall() {
    // Inicializar distâncias de acordo com estradas diretas ou INF
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            if (i == j) dist[i][j] = 0;
            else if (dist[i][j] == 0) dist[i][j] = INF;
        }
    }
    
    // Algoritmo de Floyd-Warshall
    for (int k = 1; k <= n; k++) {
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                // Se passar por k for melhor, atualize a distância
                if (dist[i][k] < INF && dist[k][j] < INF) {
                    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
                }
            }
        }
    }
}

int main() {
    cin >> n >> m >> q;
    
    // Inicializar o array de distâncias
    memset(dist, 0, sizeof(dist));
    
    // Ler as estradas
    for (int i = 0; i < m; i++) {
        int a, b;
        long long c;
        cin >> a >> b >> c;
        if (dist[a][b] == 0 || dist[a][b] > c) {
            dist[a][b] = c;
            dist[b][a] = c;  // Garantir que a estrada seja de duas vias
        }
    }
    
    // Computar todos os caminhos mais curtos entre pares
    floyd_warshall();
    
    // Responder às consultas
    for (int i = 0; i < q; i++) {
        int a, b;
        cin >> a >> b;
        if (dist[a][b] >= INF) cout << -1 << "\n";
        else cout << dist[a][b] << "\n";
    }
    
    return 0;
}
```

## Como Usar o Código

1. **Compilação**: Certifique-se de ter um compilador C++ instalado (como g++). Compile o código usando:
   ```bash
   g++ -o floyd-warshall floyd-warshall.cpp
   ```
   Onde `floyd-warshall.cpp` é o nome do arquivo contendo o código.

2. **Execução**: Após a compilação, execute o programa:
   ```bash
   ./floyd-warshall
   ```
   Você será solicitado a inserir o número de cidades, o número de estradas, e o número de consultas. Após isso, insira cada uma das estradas e as consultas.

3. **Entrada**: O formato da entrada é:
   ```
   n m q
   a1 b1 c1
   a2 b2 c2
   ...
   am bm cm
   x1 y1
   x2 y2
   ...
   xq yq
   ```
   - `n`, `m`, `q` são o número de cidades, estradas e consultas, respectivamente.
   - `ai bi ci` descreve uma estrada entre as cidades `ai` e `bi` com comprimento `ci`.
   - `xi yi` são as cidades para as quais a distância mais curta é solicitada na i-ésima consulta.

4. **Saída**: Para cada consulta, o programa imprime a distância mais curta entre as duas cidades. Se não houver caminho, imprime `-1`.

## Complexidade

O algoritmo tem uma complexidade de tempo de \(O(n^3)\), tornando-o adequado para grafos que têm até algumas centenas de nós, devido ao número de operações necessárias para calcular distâncias entre todos os pares de nós.
