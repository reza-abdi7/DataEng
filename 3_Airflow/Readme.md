## Running apachi airflow 2.0 in python environment

1. create project folder
2. Create dags, dags_local, logs, plugins folder inside the project directory

using powershell in vscode or bash:
```powershell
md ./dags, ./dags_local, ./logs, ./plugins
```
or
```
mkdir ./dags ./dags_local ./logs ./plugins
```
Set Airflow Home (optional):

Airflow requires a home directory, and uses ~/airflow by default, but you can set a different location if you prefer. The AIRFLOW_HOME environment variable is used to inform Airflow of the desired location. This step of setting the environment variable should be done before installing Airflow so that the installation process knows where to store the necessary files.
so if your project folder is named 'airflow' you do not need to set this, but if the folder is named something else you need to run this command each time opening a terminal.

```bash
export AIRFLOW_HOME=~/airflow
```

3. install apachi airflow using pip

remember to match the dependency to your python version.
```
pip install 'apache-airflow==2.9.1' \
 --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-2.9.1/constraints-3.8.txt"
```
you can get the latest version from Apache Airflow github at:

- [Airflow github](https://github.com/apache/airflow)

then initialize database with command:
```
airflow db init
```
Note that this command works on unix like os, so if you are using windows you can use via WSL (Windows Subsystem for linux) or via containers.

now you need to create a user password for webserver. you can do it by command:

```
airflow users create --username admin --firstname firstname --lastname lastname --role Admin --email admin@domain.com
```
set the password and next step.

Next start airflow webserver by command:

```
airflow webserver -p 8080
```
then open the linke in the browser (http://0.0.0.0:8080)
and loging with the credential you already set

if scheduler is not running:

```bash
airflow scheduler
```
and then refresh the page in the browser.

## Running apache airflow 2.0 in docker local

1. Create project folder
2. Create dags, dags_local, logs, plugins folder inside the project directory

```bash
mkdir ./dags ./logs ./plugins
```

Install Docker desktop application if you do not have it running on your system:

- [Download Docker Desktop Mac OS](https://hub.docker.com/editions/community/docker-ce-desktop-mac)
- [Download Docker Desktop Windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows)

after installing, make sure that it is running by checking if the docker icon is in the menu bar. check the status if the docker is running.

you can check the version of docker and docker-compose in terminal:
```bash
docker --version
docker-compose --version
```
if you see a version output, it means you have docker and docker-compose running

Now, in the Airflow documentation, you can find docker-compose.yaml file to download:
-[docker-compose.yaml file download](https://airflow.apache.org/docs/apache-airflow/stable/howto/docker-compose/index.html#fetching-docker-compose-yaml)

```bash
curl -LfO 'https://airflow.apache.org/docs/apache-airflow/2.9.1/docker-compose.yaml'
```
if finished successfully you can see docker-compose.yaml file in project directory
open it and check it. there are many services, by default it defines airflow with CeleryExecutor. if you do not need it you can modify and use local executor by /
changing the value of AIRFLOW_CORE_EXECUTOR to "LocalExecutor" and then you do not need AIRFLOW_CELERY_RESULT_BACKEND and AIRFLOW_CELERY_BROKER_URL and can delete them.

Now to initialize the Database, run:

```bash
docker-compose up airflow-init
```
it is going to download the necessary docker images and set up an admin user as "airflow" as the username and password.
once you see "airflow-init_1" exited by code 0, it means the database initialization is complete.

Next, running airflow with command:
```bash
docker-compose up -d   #in detached mode, running conatiner in background
```
you can check running containers by command:
```bash
docker ps
```
you can check running containers in desktop Docker Dashboard too.

