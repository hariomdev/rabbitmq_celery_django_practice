# RabbitMQ and Celery Implementaion in django

Celery is an asynchronous task queue based on distributed message passing. Task queues are used as a strategy to distribute the workload between threads/machines. In this tutorial I will explain how to install and setup Celery + RabbitMQ to execute asynchronous in a Django application.

To work with Celery, we also need to install RabbitMQ because Celery requires an external solution to send and receive messages. Those solutions are called message brokers. Currently, Celery supports RabbitMQ, Redis, and Amazon SQS as message broker solutions.

## Installation
The easiest way to install Celery is using pip:
```bash
pip install Celery
```

To install it on a newer Ubuntu version is very straightforward:

```bash
apt-get install -y erlang
apt-get install rabbitmq-server
```
Then enable and start the RabbitMQ service

```bash
sudo systemctl enable rabbitmq-server
systemctl start rabbitmq-server
```

Check the status to make sure everything is running smooth:

```bash
systemctl status rabbitmq-server
```

## Celery Basic Setup

First, consider the following Django project named mysite with an app named core:
```bash
mysite/
 |-- mysite/
 |    |-- core/
 |    |    |-- migrations/
 |    |    |-- templates/
 |    |    |-- apps.py
 |    |    |-- models.py
 |    |    +-- views.py
 |    |-- templates/
 |    |-- __init__.py
 |    |-- settings.py
 |    |-- urls.py
 |    +-- wsgi.py
 |-- manage.py
 +-- requirements.txt
```

Add CELERY_BROKER_URL in configuration to the settings.py file
```bash
CELERY_BROKER_URL = 'amqp://localhost'
```

Alongside with the settings.py and urls.py files, let’s create a new file named celery.py.

```bash
import os
from celery import Celery

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'mysite.settings')

app = Celery('mysite')
app.config_from_object('django.conf:settings', namespace='CELERY')
app.autodiscover_tasks()
```

Now edit the __init__.py file in the project root:

```bash
from .celery import app as celery_app

__all__ = ['celery_app']
```
