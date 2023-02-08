# RESUMO AIFLOW

Baseados em videoaulas no Youtube e a própria documentação da ferramenta.

## Conceitos iniciais
### 1 - O que é Airflow?
Plataforma para monitorar, programar horários de workflows ou pipelines de dados.

Pode ser implementável de várias maneiras, variando de processos simples no seu laptop a uma configuração distribuída que suporte workflows de Big Data.

A principal característica dos workflows do Airflow é porque são definidos por códigos em python - workflows as code. 
Portanto, são flexíveis e dinâmicos, além de possuir inúmeros conectores para integração.

Airflow é ótimo para workflows em batch (ocorrem com certa frequência). Mas não é para workflows em streaming (nesse caso é usado Apache Kafka).

Algumas características sobre os workflows: i) possuem controle de versões; ii) podem ser editados simultaneamente; iii) aceitam testes de validação.

### 2 - O que é uma DAG?
Directed Acyclic Graph - Gráfico Direcionado Acíclico. Nesse tipo de workflow há tarefas (tasks) que podem ocorrer de forma independente.
Os DAGs do airflow são compostos de tasks.
Um exemplo de DAG:

![image](https://user-images.githubusercontent.com/114788556/217649971-7297098b-e325-471a-9c01-aad5372c4b30.png)
