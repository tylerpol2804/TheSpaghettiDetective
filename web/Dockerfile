FROM python:3

RUN apt-get update && apt-get install -y vim
RUN pip install --upgrade pip

ADD . /app
WORKDIR /app
RUN pip install -r requirements.txt

EXPOSE 3333

# Required env vars and their default values

# Don't set DEBUG to True, otherwise the static files will be cached in browser until hard-refresh
ENV DEBUG=False

ENV SITE_USES_HTTPS=False
# ENV SITE_USES_HTTPS=True
ENV SOCIAL_LOGIN=False
# ENV SOCIAL_LOGIN=True
ENV ALERT_P_THRESHOLD=0.45

ENV REDIS_URL=redis://redis:6379
ENV DATABASE_URL=sqlite:////app/db.sqlite3
ENV INTERNAL_MEDIA_HOST=http://web:3334
ENV ML_API_HOST=http://ml_api:3333

# Optional env vars

# AZURE_STORAGE_CONNECTION_STRING=
# ENALBE_HISTORY=False
# EMAIL_HOST=
# EMAIL_HOST_USER=
# EMAIL_HOST_PASSWORD=
# EMAIL_PORT=
# EMAIL_USE_TLS=True
# DEFAULT_FROM_EMAIL=changeme@example.com
# ML_API_TOKEN=
# SENTRY_DSN=

RUN python manage.py collectstatic --noinput -c

CMD ["sh", "-c", "python manage.py migrate && newrelic-admin run-program gunicorn config.wsgi:application --workers 3 --bind 0.0.0.0:3334"]
