# Building Machine Learning solutions with Vertex AI: Challenge Lab

Este laboratório é recomendado para os alunos que se inscreveram na Quest [**Building and deploying machine learning solutions with Vertex AI**](). 
You will be given a scenario and a set of tasks. Instead of following step-by-step instructions, you will use the skills learned from the labs in the quest to figure out how to complete the tasks on your own! An automated scoring system (shown on the Qwiklabs lab instructions page) will provide feedback on whether you have completed your tasks correctly.

When you take a Challenge Lab, you will not be taught Google Cloud concepts. To build the solution to the challenge presented, use skills learned from the labs in the Quest this challenge lab is part of. You are expected to extend your learned skills and complete all the **`TODO:`** comments in this notebook.

Are you ready for the challenge?

## Cenário

Recentemente, contrataram você para trabalhar na engenharia de machine learning de uma startup que tem um site de resenhas de filmes. Seu gerente pediu para você criar um modelo que classifica como positivo ou negativo o sentimento das resenhas de filmes feitas pelos usuários. Essas previsões vão ser usadas como uma entrada em sistemas que agregam avaliações de filmes e para exibir as principais críticas positivas e negativas no aplicativo do site. O desafio: você tem apenas seis semanas para produzir um modelo com mais de 75% de precisão para melhorar a solução atual desenvolvida pela empresa. Além disso, após uma análise exploratória no data warehouse da startup, você descobre que ele contém apenas um pequeno conjunto de dados com 50 mil resenhas em texto que vão servir como base para criar uma solução com melhor desempenho.

## Seu desafio

Para criar e implantar rapidamente um modelo de machine learning de alto desempenho com dados limitados, você vai treinar e implantar um classificador de sentimentos personalizado BERT do TensorFlow. Ele vai realizar previsões on-line na plataforma Vertex AI do Google Cloud, que é nossa plataforma avançada de desenvolvimento de machine learning. Nela, é possível usar o AutoML e os componentes pré-criados de ML mais recentes para melhorar muito a produtividade de desempenho, a capacidade de escalonar o fluxo de trabalho e os processos de decisão com dados, além de acelerar o retorno do investimento.

![Vertex AI: Challenge Lab](./images/vertex-challenge-lab.png "Vertex Challenge Lab")

Primeiro, você vai passar por um fluxo de trabalho experimental típico. Nele, você vai criar seu modelo usando componentes **BERT** pré-treinados do TF-Hub e camadas de classificação do `tf.keras` para treinar e avaliar seu modelo em um notebook da Vertex. Em seguida, você vai empacotar o código do modelo em um contêiner do Docker para fazer o treinamento na Vertex AI do Google Cloud. Por último, você vai definir e executar um pipeline do Kubeflow no Vertex Pipelines que faz o treinamento e a implantação do seu modelo em um endpoint da Vertex que oferece previsões on-line para consultas.

## Learning objectives

* Treine um modelo do TensorFlow localmente em um [**Vertex Notebook**](https://cloud.google.com/vertex-ai/docs/general/notebooks?hl=sv).
* Conteinerize seu código de treinamento com [**Cloud Build**](https://cloud.google.com/build) e envie-o para [**Google Cloud Artifact Registry**](https://cloud.google.com/artifact-registry) como parte de um fluxo de trabalho de treinamento de contêiner personalizado da Vertex.
* Defina um pipeline usando o [**Kubeflow Pipelines (KFP) V2 SDK**](https://www.kubeflow.org/docs/components/pipelines/sdk/v2/v2-compatibility) to train and deploy your model on [**Vertex Pipelines**](https://cloud.google.com/vertex-ai/docs/pipelines).
* Consulte seu modelo em um [**Vertex Endpoint**](https://cloud.google.com/vertex-ai/docs/predictions/getting-predictions) usando previsões on-line.
