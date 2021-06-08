---
layout: post
title: "Detecção do uso máscaras via redes neurais artificiais"
author: "Thiago S Teixeira"
categories: tutorial
tags: [COVID-19, redes neurais artificiais]
image: mask.jpg
---

Esta postagem demonstra a criação de um sistema utilizando [redes neurais artificiais](https://pt.wikipedia.org/wiki/Rede_neural_artificial) para detecção de faces com máscara e sem máscara, com o objetivo de monitorar o uso de máscara em um espaço determinado, com o auxilio de uma câmera.

### YOLO

Dentre as diversas tecnicas no estado da arte da detecção de objetos, a [You only look once (YOLO)](https://arxiv.org/abs/1506.02640), se destaca pelo modo avaliação em regiões da imagem. Outras tecnicas aplicam o modelo de rede neural em varios locais e escala na imagem, ao contrario do YOLO que aplica a rede neural em toda imagem, dividindo a imagem em regiões e prevendo a probabilidade de detecção em cada região. Esta avaliação de imagem permite que YOLO seja 1000 vezes mais rapido que [R-CNN](https://en.wikipedia.org/wiki/Region_Based_Convolutional_Neural_Networks) e 100 vezes mais que [Fast R-CNN](https://arxiv.org/abs/1504.08083).

### Referências
