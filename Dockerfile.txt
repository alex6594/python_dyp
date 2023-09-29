FROM python:3.10.4-alpine
WORKDIR /code

RUN python3 -m pip install --upgrade pip
RUN apk update \
    && apk --no-cache --update add build-base
RUN apk add python3-dev
RUN apk update \
    && apk add --virtual build-deps gcc python3-dev musl-dev \
    && apk add --no-cache mariadb-dev
COPY ./requirements.txt /src/requirements.txt
RUN pip3 install --no-cache-dir --upgrade -r /src/requirements.txt


COPY . /code/

EXPOSE 8000

CMD ["python3", "manage.py", "runserver", "0.0.0.0:8000"]
