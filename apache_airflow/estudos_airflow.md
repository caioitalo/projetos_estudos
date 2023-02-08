# RESUMO AIFLOW

Baseados em videoaulas no Youtube e a própria documentação da ferramenta.

## O que é Airflow?
Plataforma para monitorar, programar horários de workflows ou pipelines de dados.

Pode ser implementável de várias maneiras, variando de processos simples no seu laptop a uma configuração distribuída que suporte workflows de Big Data.

A principal característica dos workflows do Airflow é porque são definidos por códigos em python - workflows as code. 
Portanto, são flexíveis e dinâmicos, além de possuir inúmeros conectores para integração.

Airflow é ótimo para workflows em batch (ocorrem com certa frequência). Mas não é para workflows em streaming (nesse caso é usado Apache Kafka).

Algumas características sobre os workflows: i) possuem controle de versões; ii) podem ser editados simultaneamente; iii) aceitam testes de validação.
