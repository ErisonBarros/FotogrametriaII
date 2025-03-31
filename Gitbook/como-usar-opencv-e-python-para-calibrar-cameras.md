# Como usar OpenCV e Python para calibrar câmeras

## Calibração de Câmeras com OpenCV e Python

### Introdução

A calibração de câmeras é um processo fundamental na visão computacional, que corrige as distorções ópticas geradas pelas lentes. Essas distorções afetam a precisão das medidas feitas a partir de imagens, comprometendo aplicações como mapeamento, medição e reconstrução 3D.

### Tipos de Distorções em Imagens

As lentes podem causar diversos tipos de distorções:

* **Distorção radial**: Linhas retas aparecem curvas na imagem, especialmente próximas às bordas.

<figure><img src=".gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

* **Distorção tangencial**: Ocorre quando o plano da lente não está perfeitamente paralelo ao plano do sensor da câmera.

<figure><img src=".gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

Essas distorções precisam ser corrigidas para garantir precisão em análises visuais e medições.

### O que é a Calibração de Câmeras?

A calibração de câmeras consiste em determinar os parâmetros intrínsecos (características internas da câmera, como distância focal e ponto principal) e extrínsecos (posição e orientação da câmera em relação ao objeto capturado).

## Por que acontece a distorção?

A distorção acontece porque as lentes fotográficas têm formatos e propriedades ópticas que desviam a luz de forma desigual ao longo da superfície da lente. As principais razões incluem:

* **Formato Esférico das Lentes:** A maioria das lentes possuem superfícies esféricas que fazem com que os raios de luz, principalmente aqueles que passam longe do centro, sofram desvios desiguais, criando a distorção radial.
* **Desalinhamento Mecânico:** Pequenas imprecisões na montagem das lentes podem causar desalinhamentos que resultam na distorção tangencial.

Essas características físicas das lentes fazem com que pontos retos no mundo real sejam registrados de maneira distorcida nas imagens capturadas. A calibração ajuda a compensar esses desvios, tornando as imagens úteis para análises precisas.



Para entender, veja esse exemplo simples de uma modelo de câmera conhecido como _pinhole_ (modelo de câmera pontual). Quando uma câmera foca em um objeto, ela o enxerga de maneira similar aos nossos olhos, concentrando a luz refletida no mundo real. Por meio de um pequeno orifício, a câmera concentra a luz refletida do objeto 3D em um plano na parte de trás da câmera.

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

***

A matriz da câmera, que mapeia uma cena 3D em um plano de imagem 2D, é representada por uma matriz 3×4, também chamada de matriz de projeção. Essa matriz é composta pela multiplicação da matriz intrínseca pela matriz extrínseca. A matriz intrínseca, que representa os parâmetros internos da câmera, é dada por:

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Onde ![f\_x](https://sigmoidal.ai/wp-content/ql-cache/quicklatex.com-f110302ab5b58aefbd1e92367872d303_l3.svg) e ![f\_y](https://sigmoidal.ai/wp-content/ql-cache/quicklatex.com-3f7fec1924cb1dd9c14e2df164d136ee_l3.svg) são as distâncias focais em pixels nas direções ![x](https://sigmoidal.ai/wp-content/ql-cache/quicklatex.com-ede05c264bba0eda080918aaa09c4658_l3.svg) e ![y](https://sigmoidal.ai/wp-content/ql-cache/quicklatex.com-0af556714940c351c933bba8cf840796_l3.svg), respectivamente. ![c\_x](https://sigmoidal.ai/wp-content/ql-cache/quicklatex.com-c0c0c4186f60b1ef46a29787d038637c_l3.svg) e ![c\_y](https://sigmoidal.ai/wp-content/ql-cache/quicklatex.com-c32dd83cca8ff4a563eeda6812c88338_l3.svg) são as coordenadas do ponto principal, ou centro óptico, da câmera na imagem em pixels. A última coluna da matriz é usada para calcular a projeção dos pontos 3D na imagem, mas não é necessária para a calibração da câmera.

A matriz extrínseca, que representa a posição e orientação da câmera no espaço 3D, é dada por:

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

onde u, v e w são as coordenadas homogêneas na imagem 2D. Para obter as coordenadas da imagem (x, y), basta dividir u e v por w:

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Portanto, a calibração da câmera é uma etapa crucial em qualquer aplicação de visão computacional, que pode ter um impacto significativo na precisão e confiabilidade dos resultados obtidos.

Ao reconhecer a importância da calibração de câmera, os desenvolvedores podem garantir que suas soluções baseadas em visão computacional sejam confiáveis e eficazes, independentemente do ambiente ou das condições de captura de imagem.



### Como Calibrar Câmeras Usando OpenCV e Python

#### Materiais Necessários

* Um tabuleiro de xadrez (padrão conhecido).
* Uma câmera digital.
* Bibliotecas Python: OpenCV e NumPy.

#### Passo a Passo da Calibração

**1. Preparar o Ambiente**

```bash
pip install opencv-python numpy
```

**2. Capturar Imagens do Tabuleiro de Xadrez**

Capture diversas fotos do tabuleiro de xadrez em diferentes ângulos e posições.

**3. Código para Calibração com OpenCV**

[**https://colab.research.google.com/drive/1rMBQuCtY6Xg0tAEWk2QS5fojgVye9LDP?usp=sharing**](https://colab.research.google.com/drive/1rMBQuCtY6Xg0tAEWk2QS5fojgVye9LDP?usp=sharing)

```python
import cv2
import numpy as np
import glob

# Parâmetros do tabuleiro
CHECKERBOARD = (7, 6)

# Preparar pontos 3D conhecidos
objp = np.zeros((CHECKERBOARD[0]*CHECKERBOARD[1], 3), np.float32)
objp[:, :2] = np.mgrid[0:CHECKERBOARD[0], 0:CHECKERBOARD[1]].T.reshape(-1, 2)

objpoints = []  # Pontos 3D reais
imgpoints = []  # Pontos 2D nas imagens

images = glob.glob('*.jpg')  # Caminho das imagens

for fname in images:
    img = cv2.imread(fname)
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

    # Encontrar cantos do tabuleiro
    ret, corners = cv2.findChessboardCorners(gray, CHECKERBOARD, None)

    if ret:
        objpoints.append(objp)
        imgpoints.append(corners)

# Calibrar câmera
ret, mtx, dist, rvecs, tvecs = cv2.calibrateCamera(objpoints, imgpoints, gray.shape[::-1], None, None)

print("Matriz da câmera:\n", mtx)
print("Coeficientes de distorção:\n", dist)
```

#### Resultados da Calibração

* **mtx**: matriz da câmera contendo distância focal e o ponto principal.
* **dist**: coeficientes das distorções radial e tangencial.

### Aplicação dos Parâmetros para Correção

Para corrigir imagens:

```python
img_corrigida = cv2.undistort(img, mtx, dist, None, mtx)
cv2.imshow('Imagem Corrigida', img_corrigida)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

### Conclusão

Utilizando OpenCV e Python, é possível realizar a calibração eficiente e precisa de câmeras, possibilitando aplicações mais robustas em visão computacional.
