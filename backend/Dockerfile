# Build Stage
FROM python:3.12.1-bookworm AS builder

WORKDIR /backend

RUN apt-get update && apt-get -y upgrade

ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

COPY ./requirements.txt /backend/requirements.txt

COPY ./src /backend/app

# Installing dependencies in a temporary directory
RUN mkdir /install && pip install --no-cache-dir --upgrade -r /backend/requirements.txt --target /install

# Production Stage
FROM python:3.12.1-bookworm

WORKDIR /backend

RUN pip install uvicorn

# Copying only the necessary files from the builder stage
COPY --from=builder /install /usr/local/lib/python3.12/site-packages
COPY --from=builder /backend/ /backend/


COPY ./src /backend/app

CMD [ "/bin/bash", "-c", "python3 /backend/app/migrate.py && uvicorn app.main:app --host 0.0.0.0 --port 4000"]
