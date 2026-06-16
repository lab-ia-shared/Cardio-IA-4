# 💓 CardioIA - A Nova Era da Cardiologia Inteligente

## 📋 Descrição do Projeto

O **CardioIA** é um projeto acadêmico inovador focado na convergência entre tecnologia de ponta e saúde cardiovascular. O objetivo é desenvolver uma plataforma digital que simule um ecossistema cardiológico moderno, integrando dados clínicos, modelos de Machine Learning, Visão Computacional e IoT para triagem e diagnósticos precoces.

Nesta **Fase 4 – Visão Computacional e Apoio Diagnóstico**, o projeto evolui da telemetria IoT em tempo real para a análise automatizada de imagens médicas. Assumimos o desafio de desenvolver um Assistente Cardiológico Virtual capaz de ingerir, processar e classificar radiografias de tórax para identificar anomalias estruturais, com foco principal na detecção de **Cardiomegalia**.

## 👨‍⚕️ Integrantes do Grupo
- <a href="https://www.linkedin.com/in/nicolas--araujo/">Nicolas Antonio Silva Araujo</a> (RM: 566307)
- <a href="https://www.linkedin.com/in/vitoria-bagatin-31ba88266/">Vitória Pereira Bagatin</a> (RM: 566519)

## 📂 Estrutura de Arquivos

A organização do repositório reflete as etapas do pipeline de Deep Learning:

```text
Cardio-IA-4/
│
├── notebooks/
│   ├── PreProcessamento.ipynb            # Pipeline de Pré-processamento (Parte 1)
│   └── cnn.ipynb                      # Treinamento e Transfer Learning (Parte 2)
│
├── docs/
│   └── CardioIA-4_Relatorio_Pre_Processamento.pdf   # Justificativas técnicas e arquitetura
│
└── README.md                             # Documentação principal do projeto
```

## 📑 1. Seleção de Dados Visuais (Dataset)
Para manter a coerência temática com a cardiologia, a base de dados foca na detecção de anomalias dimensionais do coração humano.

* **Dataset:** [Cardiomegaly Disease Prediction (NIH Chest X-ray Subset)](https://www.kaggle.com/datasets/rahimanshu/cardiomegaly-disease-prediction-using-cnn)
* **Link para as Imagens:** [Google Drive: dados_visuais](https://drive.google.com/drive/folders/1Kszx-A_djPO3Qvl16BZtZl7vBnXPAhFB?usp=sharing)
* **Fonte Original:** NIH Clinical Center via Kaggle
* **Tipo de Exame:** Raio-X de Tórax (Chest X-ray)
* **Classes:** Classificação binária contendo false (Coração com dimensões normais) e true (Presença de Cardiomegalia).

## ⚙️ 2. Engenharia de Dados e Pré-processamento
Para garantir que as redes neurais convolucionais (CNNs) e arquiteturas pré-treinadas (como VGG16/ResNet) consigam interpretar os exames, implementamos um pipeline matemático rigoroso:

Conversão de Canais (RGB): Transformação das imagens médicas originais em escala de cinza (1 canal) para emulação tridimensional (3 canais), garantindo compatibilidade com modelos de Transfer Learning treinados na ImageNet.

Redimensionamento Padronizado: Aplicação de upscaling para o tamanho exigido pelos modelos de Deep Learning: 224x224 pixels.

Normalização Vetorial: Escalonamento dos pixels da escala RGB (0-255) para tensores de ponto flutuante [0.0, 1.0], acelerando a convergência matemática do Gradient Descent e evitando saturação de pesos.

## 🚀 3. Pipeline Otimizado (TensorFlow) e Divisão de Dados
Devido ao alto volume de processamento de matrizes de imagem, a ingestão tradicional de dados causou gargalos de hardware (esgotamento de RAM). Para solucionar isso, implementamos o fluxo via tf.keras.utils.image_dataset_from_directory.

Lazy Loading & Prefetch: As imagens são processadas estritamente em lotes (Batch Size = 32), e o carregamento ocorre sob demanda, economizando memória volátil.

Divisão Estratificada (Data Splitting):

Treino: 3.551 imagens (80% da base de desenvolvimento) para atualização de pesos.

Validação: 887 imagens (20% da base de desenvolvimento) para monitoramento de overfitting.

Teste: 1.114 imagens reservadas e isoladas para avaliação final de acurácia, precisão e F1-Score (Parte 2).

## 🧠 4. Modelagem de Deep Learning (CNN e Transfer Learning)
Para a classificação das imagens radiográficas, desenvolvemos e comparamos duas abordagens de Redes Neurais Convolucionais utilizando o framework Keras/TensorFlow:

Modelo 1: CNN Customizada (Treinada do Zero): Arquitetura sequencial simples construída com três blocos de extração de características (camadas Conv2D + MaxPooling2D). Na camada densa final, aplicamos Dropout (0.5) para mitigação de overfitting e ativação Sigmoid para a saída binária.

Modelo 2: Transfer Learning (VGG16): Para elevar a robustez do sistema, acoplamos a arquitetura consagrada VGG16 (pré-treinada no gigantesco dataset ImageNet). As camadas convolucionais base foram congeladas para atuar como extratoras de características avançadas, enquanto uma nova rede Densa superior foi acoplada e treinada especificamente para identificar as assinaturas visuais da Cardiomegalia.

## 📊 5. Avaliação de Desempenho Clínico
O modelo foi submetido ao conjunto de Teste inédito (1.114 exames intocados), gerando resultados rigorosos suportados pela biblioteca Scikit-Learn e Seaborn:

Matriz de Confusão: Monitoramento minucioso de Falsos Positivos e Falsos Negativos, cruciais em contextos hospitalares para evitar encaminhamentos equivocados ou negligência.

Métricas de Classificação: Extração dos índices finais de Acurácia global, Precision, Recall (Sensibilidade) e F1-Score, evidenciando a assertividade da Inteligência Artificial em detectar a patologia cardíaca com equilíbrio estatístico.

## 🖥️ 6. Protótipo Interativo: O Assistente Virtual
Como prova de conceito da utilidade clínica do algoritmo, desenvolvemos um Assistente Cardiológico Virtual diretamente dentro do Google Colab (através da biblioteca ipywidgets).

A aplicação funcional apresenta a seguinte dinâmica:

Interação Real: O médico pode realizar o upload de uma radiografia externa salva em seu computador.

Processamento Assíncrono: A imagem submetida passa pelo mesmo pipeline invisível estabelecido na etapa 1 (redimensionamento e normalização tensorizada).

Diagnóstico Imediato: O modelo prediz a condição e emite um painel visual contendo o laudo simulado de saúde ("Coração Normal" vs "ALERTA DE CARDIOMEGALIA DETECTADA"), exibindo simultaneamente a porcentagem exata de confiança algorítmica.

🛡️ Governança e Ética (Pensamento Crítico sobre Vieses na Visão Computacional)
Acreditamos que a automação na saúde exige responsabilidade. O desenvolvimento deste módulo considerou as seguintes diretrizes éticas:

Viés de Representatividade: Datasets de imagens médicas (como o NIH) podem conter desbalanceamentos de atributos demográficos ocultos (gênero, etnia e idade). Reconhecemos que o modelo pode apresentar acurácias distintas dependendo do perfil do paciente, exigindo testes rigorosos de Fairness no futuro.

Viés Tecnológico (Qualidade de Imagem): As radiografias possuem variações em exposição, contraste e artefatos (como fios de marcapasso ou rotação do paciente). Nosso pipeline busca mitigar parte desse problema através do redimensionamento e normalização, mas imagens excessivamente corrompidas podem gerar falsos positivos.

Apoio, não Substituição: O CardioIA foi projetado como uma ferramenta de telemetria e triagem de apoio. A decisão clínica final, especialmente baseada em arquiteturas de "caixa-preta" como Redes Neurais, deve sempre ser validada por um cardiologista humano.
