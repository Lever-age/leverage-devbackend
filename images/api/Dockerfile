FROM python:3.6.2

EXPOSE 5000
WORKDIR /api
RUN useradd -U -u 1000 -d /api leverage

ADD app/requirements.txt /requirements.txt
RUN pip install -r /requirements.txt
ADD app_config.py /api/leverageapi/app_config.py
ADD app /api

RUN chown -R 1000:1000 /api
USER leverage

ENTRYPOINT ["/usr/local/bin/python3.6"]
CMD ["runserver.py"]
