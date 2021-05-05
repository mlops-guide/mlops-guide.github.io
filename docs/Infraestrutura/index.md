# Setting Up the IBM Enviroment with Terraform

## Introducao

Criar um script para automatizar a criacao da infraestrutura é uma das praticas herdadas de DevOps, permitindo a criacao inicial, atualizacao ou ate migracao de um provedor a outro sem necessitar nenhum esforco manual. Isso garante velocidade e robustez.

Para esse projeto será utilizado o [Terraform](https://www.terraform.io/) e um script em Python. O primeiro sera focado em cuidar dos recursos (IBM COS, Watson ML e Openscale) e o segundo cuidará das configuracoes internas do DataPak.

### Terraform

O Terraform permite que a infraestrutura seja descrita de forma facil e rapida, alem de manter o estado atual e mudar apenas aquilo que foi atualizado.


- mostrar codigo

- mostrar plan

### Script Python 

O modulo atual da IBM para o Terraform ainda esta em desenvolvimento e algumas funcionalidades mais especificas nao foram implementadas, uma delas eh a integracao com o DataPak.  Esse script sera responsavel por criar e configurar o projeto e criar o *deployment space*.

Como ele sera um complemento do Terraform, ele foi desenvolvido pensando na mesma funcionalidade, so sera modificado o que for necessario, dado uma mudanca no codigo.

