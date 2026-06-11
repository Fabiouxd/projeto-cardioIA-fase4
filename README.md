# projeto-cardioIA-fase4
# CardioIA — Fase 4: Diagnóstico com Visão Computacional

**Disciplina:** Inteligência Artificial e Visão Computacional  
**Instituição:** FIAP  
**Projeto:** CardioIA — A Nova Era da Cardiologia Inteligente  

**Integrantes:**
- Bernardo Rupolo
- Fabio Mendes
- Leonardo Magalhães
- Marcos Vinicius dos Santos Morgado

---

## Sobre o Projeto

Este repositório contém a Fase 4 do projeto CardioIA, que tem como objetivo desenvolver um protótipo de assistente cardiológico virtual baseado em Visão Computacional. Nesta fase, foram implementados o pré-processamento de imagens médicas e o treinamento de redes neurais convolucionais (CNNs) para classificação de raios-X torácicos.

---

## Dataset

O dataset utilizado foi o **Chest X-Ray Images (Pneumonia)**, disponível publicamente no Kaggle:  
https://www.kaggle.com/datasets/paultimothymooney/chest-xray-pneumonia

- 5.863 imagens de raios-X torácicos
- Duas classes: `NORMAL` e `PNEUMONIA`
- Já dividido em treino, validação e teste pelo autor
- Coletado do Guangzhou Women and Children's Medical Center

---

## Estrutura do Repositório

```
cardioIA-fase4/
├── README.md
├── notebooks/
│   ├── parte1_preprocessing.ipynb
│   └── parte2_cnn_transfer.ipynb
└── reports/
    └── relatorio_parte1.docx
```

---

## Parte 1 — Pré-processamento

O notebook `parte1_preprocessing.ipynb` implementa o pipeline completo de preparação das imagens:

1. Download do dataset via Kaggle API
2. Análise exploratória e visualização das classes
3. Redimensionamento para 224 x 224 pixels
4. Aplicação de CLAHE para melhoria de contraste
5. Normalização para o intervalo [0, 1]
6. Conversão para 3 canais RGB
7. Salvamento do dataset processado no Google Drive

O relatório detalhando as etapas e justificativas está disponível em `reports/relatorio_parte1.docx`.

---

## Parte 2 — Classificação com CNN

O notebook `parte2_cnn_transfer.ipynb` implementa duas abordagens de classificação:

**CNN treinada do zero**
- 3 blocos convolucionais com MaxPooling e Dropout
- EarlyStopping e ReduceLROnPlateau para controle do treinamento

**Transfer Learning com VGG16**
- Base do VGG16 pré-treinada no ImageNet com camadas congeladas
- Camadas densas adicionadas ao topo para classificação binária

### Resultados

| Modelo | Acurácia | Precisão | Recall | F1-Score |
|---|---|---|---|---|
| CNN do Zero | 87% | 0.86 | 0.85 | 0.86 |
| VGG16 Transfer Learning | 85% | 0.85 | 0.83 | 0.84 |

A CNN treinada do zero apresentou desempenho ligeiramente superior ao VGG16 neste experimento, o que pode ser explicado pelo tamanho relativamente pequeno do dataset e pela natureza das imagens de raios-X, que diferem significativamente das imagens coloridas do ImageNet nas quais o VGG16 foi pré-treinado.

O notebook também inclui um protótipo interativo que permite fazer upload de uma imagem de raios-X e visualizar a classificação com o grau de confiança do modelo.

---

## Como Executar

### Requisitos

- Conta no Google (para o Google Colab)
- Conta no Kaggle com API Token gerado

### Passo a Passo

1. Faça upload dos notebooks na pasta **Colab Notebooks** do Google Drive
2. Abra `parte1_preprocessing.ipynb` pelo Google Drive — ele abrirá automaticamente no Colab
3. Na célula de autenticação, preencha seu `KAGGLE_TOKEN` e `KAGGLE_USERNAME`
4. Execute todas as células — o dataset será baixado e salvo automaticamente no Google Drive
5. Feche a Parte 1 e abra `parte2_cnn_transfer.ipynb`
6. Execute todas as células — o dataset será lido diretamente do Drive

O treinamento dos dois modelos leva aproximadamente 30 a 40 minutos no Google Colab gratuito com GPU T4.

---

## Dificuldades Encontradas

Durante o desenvolvimento, o grupo enfrentou alguns problemas práticos que vale registrar:

**Limitação de sessões simultâneas no Colab gratuito**  
O Google Colab gratuito não permite rodar dois notebooks ao mesmo tempo. Como o dataset é baixado temporariamente na sessão ativa, a Parte 2 não conseguia acessar os dados da Parte 1 ao rodar em sessões separadas. A solução adotada foi salvar o dataset no Google Drive ao final da Parte 1, permitindo que a Parte 2 o leia diretamente de lá em qualquer sessão futura, sem necessidade de novo download.

**Estrutura de pastas duplicada no dataset**  
Ao descompactar o dataset do Kaggle, verificou-se que a estrutura de pastas gerava um caminho duplicado (`chest_xray/chest_xray/train`), o que causava erros nos geradores de dados do Keras. O problema foi identificado inspecionando a estrutura de diretórios e corrigido movendo o conteúdo para o caminho esperado.

**Validação do modelo com imagens reais**  
Após o treinamento, o protótipo interativo foi testado com imagens reais de raios-X de pacientes com pneumonia, obtidas fora do dataset de treino. O modelo classificou corretamente a maioria das imagens, com grau de confiança variando entre 55% e 90% dependendo da clareza dos achados radiológicos. Esse teste informal indicou que o modelo generalizou de forma razoável para imagens fora do conjunto de treinamento, embora limitações existam dado o escopo acadêmico do projeto.

---

## Observações

- Este projeto é de uso exclusivamente acadêmico e não substitui avaliação médica profissional.
- O dataset NIH Chest X-Ray14 (112.120 imagens, 14 classes) foi considerado inicialmente, mas descartado por inviabilidade computacional no ambiente do Google Colab gratuito. Ele representa um caminho natural para trabalhos futuros que disponham de maior capacidade de processamento.

---

## Referências

- Mooney, P. (2018). *Chest X-Ray Images (Pneumonia)*. Kaggle. https://www.kaggle.com/datasets/paultimothymooney/chest-xray-pneumonia
- Kermany, D. et al. (2018). Identifying Medical Diagnoses and Treatable Diseases by Image-Based Deep Learning. *Cell*, 172(5), 1122–1131.
- Simonyan, K., Zisserman, A. (2014). Very Deep Convolutional Networks for Large-Scale Image Recognition. *arXiv:1409.1556*.
- Wang, X. et al. (2017). ChestX-ray8: Hospital-scale Chest X-ray Database and Benchmarks. *IEEE CVPR*, 3462–3471.
