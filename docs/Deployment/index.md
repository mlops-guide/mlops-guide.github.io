# Deployment com o Watson Machine Learning

## Introdução

A IBM oferece diversos serviços e ferramentas para o desenvolvimento de modelos preditivos e de inteligência artificial que possam melhorar processos, interações e ações. Dentre eles, o [Watson Machine Learning](https://www.ibm.com/cloud/machine-learning) permite que modelos sejam salvos na nuvem, onde podem ser versionados e acessados por meio de APIs, assim permitindo que predições sejam feitas sem a necessidade de se criar e configurar uma instância para hospedar o micro-serviço. 

### Deployment Space


- Mostrar como funciona 

### Script Python 


### Acessando as Predições do Modelo

Feito o deplyment do modelo, é possível acessa-lo tanto por meio de requests em um end-point como pela biblioteca em Python ``` ibm_watson_machine_learning ```, onde pode-se mandar tanto somente features para uma única predição como também várias linhas de um dataframe por meio de um payload.

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