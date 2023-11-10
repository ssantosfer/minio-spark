# Processamento de Dados com Apache Spark e Armazenamento no MinIO

## Descrição

Este projeto demonstra o uso do Apache Spark para processamento de dados e do Minio para armazenamento de dados em larga escala. A combinação dessas tecnologias permite a análise eficiente de grandes volumes de informações e o armazenamento seguro e escalável de dados brutos e resultados. 

## Conteúdo

1. **Introdução**

Você sabe o que é o *Apache Spark* e o *MinIO*? Abaixo vou trazer um breve contexto do que é cada uma dessas duas tecnologia e aplicabilidade no projeto desenvolvido.

`Apache Spark`

O Apache Spark é um framework de código aberto projetado para processamento de dados em larga escala e análise de dados. Ele foi desenvolvido para ser rápido, flexível e fácil de usar.
O Spark processa os dados em **memória** sempre que possível, o que o torna significativamente mais rápido que os sistemas de processamento de dados tradicionais, como o Hadoop.

Sendo assim, o Spark é amplamente usado em projetos de análise de dados, aprendizado de máquina, processamento de dados em tempo real e ETL (Extract, Transform, Load). É uma escolha popular para organizações que precisam lidar com grandes volumes de dados de forma eficiente.

`MinIO`

Minio é um servidor de armazenamento de objetos de código aberto que permite o armazenamento, recuperação e gerenciamento eficiente de objetos de dados, como imagens, vídeos, documentos e outros tipos de arquivos.


No projeto, o **spark** foi utilizado para consultar os dados, em CSV, armazenados na camada *Landing* no **MinIO**. Processa-lós e trata-lós usando o spark e armazena-lós novamente nas camadas de *processing* (dado sem nenhum modificação, apenas com o formato do arquivo alterado para parquet) e *curated* (dado já agregado para a necessidade do negócio)

2. **Requisitos**

 Para realizar o projeto, é necessário ter o **MinIO** e o **Spark** configurado localmente no seu computador. 
    
3. **Configuração**

Realizei todo o processo de configuração dos programas em uma máquina virtual linux (Ubuntu) por conta da facilidade nas configurações. Logo, o passo a passo abaixo será pensando em um SO Linux:

**MinIO**:
```shell
wget https://dl.min.io/server/minio/release/linux-amd64/archive/minio_20231101183725.0.0_amd64.deb -O minio.deb
sudo dpkg -i minio.deb
```

```shell
mkdir ~/minio
minio server ~/minio --console-address :9090
```

**Spark**:
```shell
sudo apt update
```

```shell
sudo apt install default-jdk
```

```shell
wget https://dlcdn.apache.org/spark/spark-3.4.0/spark-3.4.0-bin-hadoop3.tgz
```

```shell
tar xvf spark-3.4.0-bin-hadoop3.tgz
```

```shell
sudo mv spark-3.4.0-bin-hadoop3 /opt/spark
```

```shell
nano ~/.bashrc
```

```shell
export SPARK_HOME=/opt/spark
export PATH=$PATH:$SPARK_HOME/bin
```
```shell
source ~/.bashrc
```

**Maven**:

Baixar os arquivos JARs de acordo com a versão do Spark que foi baixada anteriormente. São dois arquivos JARs que precisam ser baixados:
- O primeiro você encontra na linha de **files**.
- O segundo fica disponivel na parte de **Compile Dependencies (1)**.

Após o download desses dois arquivos, eles devem ser incluidos dentro da pasta de JARs do **spark**.

4. **Execução do Projeto**

A execução do projeto é realizada via spark-submit no terminal de consulta. Pelo seguinte código:
```shell
spark-submit --name job1-app job-spark.py --verbose
```
O arquivo do projeto (job-spark.py) deve estar na sua pasta Home ou vai precisar passar o caminho que o arquivo do projeto vai estar salvo na sua máquina. 
    
6. **Estrutura do Projeto**

```bash
|-- data/                # Dados de exemplo
|-- job-spark.py/        # Arquivo do Projeto em Python
|-- README.md            # Este arquivo
```

8. **Links Úteis**

- *Base no Kaggle*: https://www.kaggle.com/datasets/nhs/general-practice-prescribing-data
- *MinIO*: https://min.io/download#/kubernetes
- *Spark*: https://spark.apache.org/downloads.html | https://www.virtono.com/community/tutorial-how-to/how-to-install-apache-spark-on-ubuntu-22-04-and-centos/
- *Maven hadoop-aws*: https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-aws

## Exemplo de Uso

```shell
print ("\nImprime os dados lidos da lading:")
print (df.show())
```
![image](https://github.com/ssantosfer/minio-spark/assets/105020346/8db0991b-3989-46e0-b079-df2736d44f1f)

```shell
print ("\nImprime o schema do dataframe lido da raw:")
print (df.printSchema())
```

![image](https://github.com/ssantosfer/minio-spark/assets/105020346/add0d295-0c6c-444e-a41f-f121ab8425a3)

```shell
print ("\n ========= Imprime o resultado do dataframe processado =========\n")
print (df_result.show())
```
![image](https://github.com/ssantosfer/minio-spark/assets/105020346/8bade416-bb9d-41f6-9748-53d197cfe748)
