FROM python:3.10-slim-bookworm
ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /
RUN useradd --uid 1000 --create-home app && \
  apt-get update && \
  apt-get install --no-install-recommends --yes \
  build-essential \
  libpcre3 \
  libpcre3-dev \
  libffi-dev \
  openssl \
  && rm -rf /var/lib/apt/lists/* && \
  pip install --no-cache-dir pipenv==2023.2.18

RUN mkdir -p /var/www/app && mkdir /app

COPY Pipfile* /app
RUN cd /app && pipenv sync --system

COPY . /app

CMD ["uwsgi", "--http", ":5000", "--master", "--enable-threads", "-s", "/var/www/app/app.sock", "--manage-script-name", "--mount", "/=app:app"]