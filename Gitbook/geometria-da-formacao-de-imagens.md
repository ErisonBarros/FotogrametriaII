---
description: Prof. Erison Barros
---

# Geometria da formação de imagens

### **A configuração**

Para entender o problema facilmente, digamos que você tenha uma câmera instalada em uma sala.

Dado um ponto 3D **P** nesta sala, queremos encontrar as coordenadas de pixel (u, v) deste ponto 3D na imagem tirada pela câmera.

Há três sistemas de coordenadas em jogo nesta configuração. Vamos analisá-los.

#### 1. Sistema de Coordenadas Mundial

<figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption><p>Figura 1: O Sistema de Coordenadas do Mundo e o Sistema de Coordenadas da Câmera são relacionados por uma Rotação e uma translação. Esses seis parâmetros (3 para rotação e 3 para translação) são chamados de parâmetros extrínsecos de uma câmera.</p></figcaption></figure>

[Home](https://learnopencv.com/) > [Calibração da câmera](https://learnopencv.com/category/camera-calibration/) >Geometria da formação da imagem

## Geometria da formação de imagens



Neste post, explicaremos a formação da imagem de um ponto de vista geométrico.

Especificamente, abordaremos a matemática por trás de como um ponto em 3D é projetado no plano da imagem.

Este post foi escrito com iniciantes em mente, mas é de natureza matemática. Dito isso, tudo o que você precisa saber é multiplicação de matrizes.

### **A configuração**

Para entender o problema facilmente, digamos que você tenha uma câmera instalada em uma sala.

Dado um ponto 3D **P** nesta sala, queremos encontrar as coordenadas de pixel (u, v) deste ponto 3D na imagem tirada pela câmera.

Há três sistemas de coordenadas em jogo nesta configuração. Vamos analisá-los.

#### 1. Sistema de Coordenadas Mundial

<figure><img src="https://learnopencv.com/wp-content/uploads/2020/02/world-coordinates-and-camera-coordinates.png" alt="" height="540" width="960"><figcaption><p>Figura 1: O Sistema de Coordenadas do Mundo e o Sistema de Coordenadas da Câmera são relacionados por uma Rotação e uma translação. Esses seis parâmetros (3 para rotação e 3 para translação) são chamados de parâmetros extrínsecos de uma câmera.</p></figcaption></figure>

Para definir localizações de pontos na sala, precisamos primeiro definir um sistema de coordenadas para esta sala. Requer duas coisas.

1. **Origem** : Podemos fixar arbitrariamente um canto da sala como origem ![(0,0,0)](https://learnopencv.com/wp-content/ql-cache/quicklatex.com-6bf72e6c5a966b1de992ea5360115533_l3.png).
2. **Eixos X, Y, Z** : Também podemos definir os eixos X e Y da sala ao longo das duas dimensões no chão e o eixo Z ao longo da parede vertical.

Usando o exposto acima, podemos encontrar as coordenadas 3D de qualquer ponto nesta sala medindo sua distância da origem ao longo dos eixos X, Y e Z.

Este sistema de coordenadas anexado à sala é chamado de **Sistema de Coordenadas do Mundo** . Na Figura 1, ele é mostrado usando eixos de cor laranja. Usaremos fonte em negrito (eg ![\mathbf{X\_w}](https://learnopencv.com/wp-content/ql-cache/quicklatex.com-5f5a4b9e6fc413d48ed3dd60f3e6e041_l3.png)) para mostrar o eixo e fonte regular para mostrar uma coordenada do ponto (eg ![X\_w](https://learnopencv.com/wp-content/ql-cache/quicklatex.com-908f4fe44f003c647a3f69b6a624967c_l3.png)).

Vamos considerar um ponto P nesta sala. No sistema de coordenadas do mundo, as coordenadas de P são dadas por ![(X\_w, Y\_w, Z\_w)](https://learnopencv.com/wp-content/ql-cache/quicklatex.com-4f853d7fb605651865f9b11c9fafaf11_l3.png). Você pode encontrar as coordenadas ![X\_w](https://learnopencv.com/wp-content/ql-cache/quicklatex.com-908f4fe44f003c647a3f69b6a624967c_l3.png), ![Y\_w](https://learnopencv.com/wp-content/ql-cache/quicklatex.com-0787ab090c5d77ba905e12d3c874f3cf_l3.png), e ![Z\_w](https://learnopencv.com/wp-content/ql-cache/quicklatex.com-0d5b942e6ef4a7d061a8922e76b07f6a_l3.png)deste ponto simplesmente medindo a distância deste ponto da origem ao longo dos três eixos.

