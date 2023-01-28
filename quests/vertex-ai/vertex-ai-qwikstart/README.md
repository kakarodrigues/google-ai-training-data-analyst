# Vertex AI: Qwik Start

Neste laboratório, você usará o BigQuery para processamento de dados e análise exploratória de dados e a plataforma Vertex AI para treinar e implantar um modelo TensorFlow Regressor personalizado para prever o valor da vida útil do cliente (CLV). O objetivo do laboratório é apresentar a Vertex AI por meio de um caso de uso real de alto valor - CLV preditivo. Você começará com um fluxo de trabalho local do BigQuery e do TensorFlow com o qual já esteja familiarizado e progredirá para treinar e implantar seu modelo na nuvem com a Vertex AI, além de recuperar previsões e explicações de seu modelo.

![Vertex AI](./images/vertex-ai-overview.png "Vertex AI Overview")

A Vertex AI é uma plataforma unificada e de última geração do Google Cloud. Ela serve para o desenvolvimento de machine learning e é a sucessora do AI Platform, como anunciamos no Google I/O em maio de 2021. Ao desenvolver as soluções de machine learning na Vertex AI, você aproveita os componentes pré-criados de ML e AutoML mais recentes para melhorar bastante a produtividade do desenvolvimento, a capacidade de escalonar o fluxo de trabalho e as decisões com base em dados, além de acelerar o retorno do investimento.

## Objetivos

* Train a TensorFlow model locally in a hosted .
* Create a  artifact for experiment tracking.
* Containerize your training code with .
* Run a  with your custom model container.
* Use  to visualize model performance.
* Deploy your trained model to a [**Vertex Online Prediction Endpoint**](https://cloud.google.com/vertex-ai/docs/predictions/getting-predictions) for serving predictions.
* Request an online prediction and explanation and see the response.

* Treinar localmente um modelo do TensorFlow em um notebook da Vertex [**Vertex Notebook**](https://cloud.google.com/vertex-ai/docs/general/notebooks?hl=sv)

* Criar um artefato de conjunto de dados tabular para rastrear experimentos [**managed Tabular dataset**](https://cloud.google.com/vertex-ai/docs/training/using-managed-datasets?hl=sv)

* Conteinerizar o código de treinamento com o Cloud Build e enviar para o Artifact Registry do Google Cloud [**Cloud Build**](https://cloud.google.com/build) and push it to [**Google Cloud Artifact Registry**](https://cloud.google.com/artifact-registry)

* Executar um job de treinamento personalizado da Vertex AI com o contêiner de modelo personalizado

* Usar o Vertex TensorBoard para ver o desempenho do modelo [**Vertex TensorBoard**](https://cloud.google.com/vertex-ai/docs/experiments/tensorboard-overview)

* Implantar o modelo treinado em um endpoint de previsões on-line da Vertex para exibir as previsões [**Vertex AI custom training job**](https://cloud.google.com/vertex-ai/docs/training/custom-training)

* Solicitar uma previsão e uma explicação on-line e conferir a resposta



## Setup

### 1. Enable Cloud Services utilized in the lab environment:

#### 1.1 Launch [Cloud Shell](https://cloud.google.com/shell/docs/launching-cloud-shell)

#### 1.2 Set your Project ID

Confirm that you see the desired project ID returned below:
```
gcloud config get-value project
```

If you do not see your desired project ID, set it as follows:
```
PROJECT_ID=[YOUR PROJECT ID]
gcloud config set project $PROJECT_ID
```

#### 1.3 Use `gcloud` to enable the services

```
gcloud services enable \
  compute.googleapis.com \
  iam.googleapis.com \
  iamcredentials.googleapis.com \
  monitoring.googleapis.com \
  logging.googleapis.com \
  notebooks.googleapis.com \
  aiplatform.googleapis.com \
  bigquery.googleapis.com \
  artifactregistry.googleapis.com \
  cloudbuild.googleapis.com \
  container.googleapis.com
```

### 2. Create Vertex AI custom service account for Vertex Tensorboard experiment tracking

#### 2.1. Create custom service account
```
SERVICE_ACCOUNT_ID=vertex-custom-training-sa
gcloud iam service-accounts create $SERVICE_ACCOUNT_ID  \
    --description="A custom service account for Vertex custom training with Tensorboard" \
    --display-name="Vertex AI Custom Training"
```

#### 2.2. Grant it access to GCS for writing and retrieving Tensorboard logs
```
PROJECT_ID=$(gcloud config get-value core/project)
gcloud projects add-iam-policy-binding $PROJECT_ID \
    --member=serviceAccount:$SERVICE_ACCOUNT_ID@$PROJECT_ID.iam.gserviceaccount.com \
    --role="roles/storage.admin"
```

#### 2.3. Grant it access to your BigQuery data source to read data into your TensorFlow model
```
gcloud projects add-iam-policy-binding $PROJECT_ID \
    --member=serviceAccount:$SERVICE_ACCOUNT_ID@$PROJECT_ID.iam.gserviceaccount.com \
    --role="roles/bigquery.admin"
```

#### 2.4. Grant it access to Vertex AI for running model training, deployment, and explanation jobs
```
gcloud projects add-iam-policy-binding $PROJECT_ID \
    --member=serviceAccount:$SERVICE_ACCOUNT_ID@$PROJECT_ID.iam.gserviceaccount.com \
    --role="roles/aiplatform.user"
```

### 3. Creating a Vertex Notebooks instance

An instance of **Vertex Notebooks** is used as a primary lab environment.

To provision the instance follow the [Create an new notebook instance](https://cloud.google.com/vertex-ai/docs/general/notebooks) setup guide. Use the *TensorFlow Enterprise 2.3* no-GPU image. Leave all other settings at their default values.

After the instance is created, you can connect to [JupyterLab](https://jupyter.org/) IDE by clicking the *OPEN JUPYTERLAB* link in the [Vertex AI Notebooks Console](https://console.cloud.google.com/vertex-ai/notebooks/instances).


### 4. Clone the lab repository

In your **JupyterLab** instance, open a terminal and clone this repository in the `home` folder.
```
cd
git clone https://github.com/GoogleCloudPlatform/training-data-analyst.git
```

### 5. Install the lab dependencies

Run the following in the **JupyterLab** terminal to go to the `training-data-analyst/self-paced-labs/vertex-ai/vertex-ai-qwikstart` folder, then pip install `requirements.txt` to install lab dependencies:

```bash
cd training-data-analyst/self-paced-labs/vertex-ai/vertex-ai-qwikstart
pip install -U -r requirements.txt
```

### 6. Navigate to lab notebook

In your **JupyterLab** instance, navigate to __training-data-analyst__ > __self-paced-labs__ > __vertex-ai__ > __vertex-ai-qwikstart__, and open __lab_exercise.ipynb__.

Open `lab_exercise.ipynb` to complete the lab. 

Happy coding!
