# Deployment com o Watson Machine Learning

## Introdução

A IBM oferece diversos serviços e ferramentas para o desenvolvimento de modelos preditivos e de inteligência artificial que possam melhorar processos, interações e ações. Dentre eles, o [Watson Machine Learning](https://www.ibm.com/cloud/machine-learning) permite que modelos sejam salvos na nuvem, onde podem ser versionados e acessados por meio de APIs, assim permitindo que predições sejam feitas sem a necessidade de se criar e configurar uma instância para hospedar o micro-serviço. 

### Deployment Space


- Mostrar como funciona 

### Script Python 

O script completo pode ser encontrado no [repositório exemplo](https://github.com/MLOPsStudyGroup/dvc-gitactions/blob/master/src/scripts/Pipelines/model_deploy_pipeline.py) e recebe como argumento o arquivo de modelo treinado, o caminho para a raiz do projeto e o arquivo de credenciais.

```python
python3 model_deploy_pipeline.py ./model_file ../path/to/project/ ../credentials.yaml
```

A partir desses argumentos, são definidas _____
```python
import os
import sys
import yaml

MODEL_PATH = os.path.abspath(sys.argv[1])
PROJ_PATH = os.path.abspath(sys.argv[2])
CRED_PATH = os.path.abspath(sys.argv[3])
META_PATH = PROJ_PATH + "/metadata.yaml"
```
Em seguida essas ____ são usadas para carregar os dicionários correspondentes e abrir o modelo, que pode ser feito tanto usando tanto a biblioteca ```joblib``` quanto a ```pickle```.
```python
import pickle
import joblib

with open(CRED_PATH) as stream:
    try:
        credentials = yaml.safe_load(stream)
    except yaml.YAMLError as exc:
        print(exc)


with open(META_PATH) as stream:
    try:
        metadata = yaml.safe_load(stream)
    except yaml.YAMLError as exc:
        print(exc)

with open(MODEL_PATH, "rb") as file:
    # pickle_model = pickle.load(file)
    pipeline = joblib.load(file)
```
O próximo passo é criar um instância do ```client``` do IBM Watson, para isso serão usadas as credenciais e o [Deployment Space](paginaDoDeploySpace) padrão será declarado usando o ID contido no arquivo de credenciais, também são criados ____ com informações do modelo restiradas do arquivode ```metadata```.
```python
from ibm_watson_machine_learning import APIClient

wml_credentials = {"url": credentials["url"], "apikey": credentials["apikey"]}

client = APIClient(wml_credentials)
client.spaces.list()

MODEL_NAME = metadata["project_name"] + "_" + metadata["project_version"]
DEPLOY_NAME = MODEL_NAME + "-Deployment"
MODEL = pipeline
SPACE_ID = credentials["space_id"]

client.set.default_space(SPACE_ID)
```
Antes do deployment, é preciso dizer ao Watson algumas características do modelo, como nome, tipo (no caso do exemplo é  ```scikit-learn_0.23```) e especificações da máquina que vai rodar o micro-serviço.

Depois disso, o modelo é guardado como um ```Asset``` do Watson ML.
```python
model_props = {
    client.repository.ModelMetaNames.NAME: MODEL_NAME,
    client.repository.ModelMetaNames.TYPE: metadata["model_type"],
    client.repository.ModelMetaNames.SOFTWARE_SPEC_UID: client.software_specifications.get_id_by_name(
        "default_py3.7"
    ),
}

model_details = client.repository.store_model(model=MODEL, meta_props=model_props)
model_uid = client.repository.get_model_uid(model_details)
```
Para fazer o deployment nomavente são passadas algumas características do modelo depois o deploy é feito a partir do ID de armazenamento obtido na etapa anterior.
```python
deployment_props = {
    client.deployments.ConfigurationMetaNames.NAME: DEPLOY_NAME,
    client.deployments.ConfigurationMetaNames.ONLINE: {},
}

deployment = client.deployments.create(
    artifact_uid=model_uid, meta_props=deployment_props
)
```
Finalmente, os IDs de armazenamento e deployment são salvos ou atualizados no arquivo ```metadata.yaml```.
```python
deployment_uid = client.deployments.get_uid(deployment)

metadata["model_uid"] = model_uid
metadata["deployment_uid"] = deployment_uid

f = open(META_PATH, "w+")
yaml.dump(metadata, f, allow_unicode=True)
```


### Acessando as Predições do Modelo

Feito o deplyment do modelo, é possível acessa-lo tanto por meio de requests em um end-point como pela biblioteca em Python ```ibm_watson_machine_learning```, onde pode-se mandar tanto somente features para uma única predição como também várias linhas de um dataframe por meio de um payload.

```python
payload = {
    "input_data": [
        {
            "fields": X.columns.to_numpy().tolist(),
            "values": X.to_numpy().tolist(),
        }
    ]
}

result = client.deployments.score(DEPLOYMENT_UID, payload)
```
A resposta do modelo vai conter a predição e certeza???????? correspondente. No caso de um classificardor binário, a resposta seria a seguinte:

        [1, [0.06057910314628456, 0.9394208968537154]],
        [1, [0.23434887273340754, 0.7656511272665925]],
        [1, [0.08054183674380211, 0.9194581632561979]],
        [1, [0.07877206037184215, 0.9212279396281579]],
        [0, [0.5719774367794239, 0.42802256322057614]],
        [1, [0.017282880299552716, 0.9827171197004473]],
        [1, [0.01714869904990468, 0.9828513009500953]],
        [1, [0.23952044576217457, 0.7604795542378254]],
        [1, [0.03055527110545664, 0.9694447288945434]],
        [1, [0.2879899631347379, 0.7120100368652621]],
        [0, [0.9639766912352016, 0.03602330876479841]],
        [1, [0.049694416576558154, 0.9503055834234418]],


### Atualizando o Modelo

### Rollback do Modelo
