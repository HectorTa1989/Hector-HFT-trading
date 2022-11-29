# Hector-HFT-trading
Microservices Based Algorithmic Trading System

![image](https://user-images.githubusercontent.com/31132150/204420677-ee3ebc2f-5b34-423d-a9e5-426c3516417d.png)

Say that you have an algorithmic trading strategy idea. Whats next ? you need to code it, test it and deploy it in the market. Lets start with downloading market data as csv files and coding a rough version of the strategy to test for its effectiveness, well if the results are promising then you would need to write more code to paper trade it and eventually go live. Maybe later you decide to add Machine Learning to the mix. Before you know this would be pieces of code sticking together leading to a monolithic disaster.

Instead we can take an easier route of programming your strategy in a platform like Deltix, Algotrader, Metatrader etc… Or even better we can go the SaaS route and opt for services like Quantoconnect, Quantopian or Qunatiacs where you can start coding your strategy without being concerned about data, infrastructure or broker connections. It’s a great way to learn and start your journey, especially the community driven aspects of the SaaS platforms. Either way, most systems come at a cost and you would have to make a few sacrifices like coding up in a proprietary language or not be able to customize it enough to meet your needs and most important of all if the strategy have impressive performance would you want the strategy to be in a server that’s not yours.

Even though I have worked on multiple predictive modelling projects involving reinforcement learning (article) and alternative data (article), one thing that always troubled me was actually turning those ideas into trading strategies that I can actually backtest, paper trade and eventually go live . I have been playing around with open source backtesters like Zipline, Qstrader and Backtrader for a while, but they were still not self-sufficient to trade multiple strategies or even to use Machine Learning and lacked a lot of supporting features. But things started falling into place when I stumbled upon this amazing containerization technology called Docker. All of a sudden it opened up an array of opportunities for packaging, reproducing and productionizing software and data-science projects.

MBATS
From scratch to a functional trading infrastructure under 5 minutes and you can control all aspects of strategy development and production lifecycle using Python; that’s a one line definition of MBATS.

MBATS (Microservices Based Algorithmic Trading System) is a Docker based trading infrastructure designed with scale and interoperability in mind and one in which Machine Learning is a first class citizen. The project is opensource and all the components used in this project are opensource technologies and they have been chosen in such a way that there is a analogues managed service provided by top Cloud vendors, so that the migration to Cloud can be seamless.

Quick start
MBATS is based on Docker containers. Running your Infrastructure is as easy as running one command from your terminal. You can either run MBATS on your local machine or on the cloud using docker-compose.

Setting up the entire infrastructure can be done in 2 lines of script

Clone the github page. https://github.com/HectorTa1989/Hector-HFT-trading/
Run “docker-compose up -d — build”
Run “starter_script.bat”

Architecture
Using MBATS, you can easily create Trading Strategies in Backtrader, manage Machine Learning models with MLflow, use Postgres database with pgAdmin for storing and querying Market data. Store files and objects in Minio and use Superset to visualize performance of backtested and live strategies. And tie all the different components together and orchestrate jobs using Airflow and many more features to help you get started from idea to live trading faster and with least effort.

![image](https://user-images.githubusercontent.com/31132150/204420779-77b3b720-aa8a-4efb-946b-09d7406b887d.png)


Backtrader
Backtrader is a python based opensource event-driven trading strategy backtester with support for live trading. The reason why I choose Backtrader over other opensource backtesters like Zipline and QuantConnect is because of the amazing documentation and the community driven approach. For this project, I had to build quite a few custom Backtrader modules like Datafeeds and Analyzers, but due to the clean design and extensibility of Backtrader combined with the community support made most of it quite easy.

![image](https://user-images.githubusercontent.com/31132150/204420811-a6daf28c-caea-418f-a6ce-8cf8011cde41.png)


MLflow
Anyone who has worked in the Datascience field would have heard about Spark, well the founders of Spark have brought a similar disruptive tool to revolutionize the Machine Learning landscape and that is MLflow. Mlflow is an open source platform to manage the ML lifecycle, including experimentation, reproducibility and deployment. It currently offers four components:

MLflow Tracking
MLflow Projects
MLflow Models
MLflow Registry
There are a few other organizations that try to address this problem but what separates MLflow from the likes of Google-TFX, Facebook-FBLearner Flow and Uber-Michelangelo is that MLFlow try to address the concerns of the crowd rather than a single organization and therefore it is universal and community driven.

In this project all the Machine Learning models can be tracked, packaged and served by MLflow and the model artifacts are stored in Minio. The ML models are served using MLflow pyfunc, but we also have the option to serve the model as Rest API(code in sample jupyter notebook)

MLflow is platform, package and language agnostic whereas if you look at TFX which only supports Tensorflow. Moreover the rate at which the community is growing, it is highly likely that it will be the standard tool for Machine Learning lifecycle management in the next few months. If we decide to scale and make the switch to the Cloud for Machine Learning it would be super easy as both AWS Sagemaker and Azure Machine Learning has integration for MLflow. Also MLFlow and Spark has been developed by the Databricks team and therefore its native in their environment . So in the future if we want to train or serve the Machine Learning models of MBATS in the cloud (Databricks, Azure or AWS) it will be just few lines of code to make the switch.

Apache Airflow
Apache Airflow is an open-source workflow management platform, basically cron on steroids and it has wide array of integration with popular platforms and data stores. In this this project we use airflow for scheduling two tasks. One Task/DAG for downloading daily and minute data into the database and another dynamic DAG for scheduling live strategies.

Apache Superset
From the creators of Apache Airflow, Apache Superset is a Data Visualization tool designed and used within Airbnb and later opensourced. Superset offers interactive Data Exploration that will let you slice, dice and visualize data. Why pay for Tableau and Power-BI when you can use something that is opensource and designed for scale. It might not have all the bells and whistles like Looker but it has enough features for ensuring an amazing user experience. In this project we use Superset to visualize Backtesting and Live trading performance.

![image](https://user-images.githubusercontent.com/31132150/204420864-d8712d50-bffe-43fa-89f7-f5050627016c.png)


Minio
MinIO is pioneering high performance object storage. With READ/WRITE speeds of 55 GB/s and 35 GB/s on standard hardware, object storage can operate as the primary storage tier for a diverse set of workloads. Amazon’s S3 API is the defacto standard in the object storage world and represents the most modern storage API in the market. MinIO adopted S3 compatibiity early on and was the first to extend it to support S3 Select. Because of this S3 Compatibility by using Minio we have an upper hand of moving this object store towards cloud (AWS S3, Google Cloud Storage, Azure Storage) with minimal change to the codebase.

PostgreSQL
We have 2 Databases in our PostgreSQL

Security Master database that stores the Daily and Minute data for Forex Symbols
Risk Database to store the position information and the performance metrics.
The Databases can be managed through PgAdmin

Current Features
Infrastructure as Code — less than 5 minutes from scratch to a fully functional trading infrastructure.
Backtesting and Live trading Forex using Oanda Broker API (Can be easily be modified to accommodate Interactive Brokers for Equity).
Machine Learning model development and deployment using MLflow.
Multiple symbol strategy support.
Multiple strategy support.
Market data download and Live trading scheduling using Airflow.
Superset Dashboard for real-time monitoring of Live trading and Backtest performance results.
Easily extensible to support any kind of structured data.
Full code base in Python except for docker-compose setup.
Scaling to the Cloud
Every technology used in this project has a analogues managed service offered in the cloud. And the best part of scaling a microservices based architecture is that you can approach it in many ways to fits your need. Whether you want to move one functionality to the cloud or if you want to offload the workload of a component to the Cloud but keep all the critical parts on premise, the migration is quite easy when compared to a monolithic architecture. Moreover if the Cloud service is using the same technology then the migration is effortless. A simple example for this is GCP Cloud Composer which is built on top of Apache Airflow and Kubernetes which means that all the tasks/DAG’s that we are using in this project can be used in Cloud Composer as well. In general I have found GCP has a better strategy and technology in place for building a hybrid Cloud infrastructure and for that reason here’s an architecture if this project has to be transferred entirely into the GCP platform.

Let me know if you are interested in seeing the below architecture take form using Terraform

![image](https://user-images.githubusercontent.com/31132150/204420909-46c75f7c-c5b0-4de5-b737-8d8c2946493f.png)


MBATS is still in its early stages and need more functionalities and stress testing before it can be used by an a actual trading desk. Instead its meant for educational purpose and it was my curiosity for Machine Learning and Quantitative Trading that drove me to explore this idea and opensource this project so that it could be used by aspiring Quants.

Acknowledgments
Backtrader community

Backtest-rookies

Quantstart

Beyond Jupyter notebook — Udemy course

MBATS is still in its early stages and need more functionalities and stress testing before it can be used by an a actual trading desk. Instead its meant for educational purpose and it was my curiosity for Machine Learning and Quantitative Trading that drove me to explore this idea and opensource this project so that it could be used by aspiring Quants.

Linkedin: https://www.linkedin.com/in/hector-ta-743b0172/
