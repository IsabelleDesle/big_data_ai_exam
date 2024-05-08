# FROM python:3.12.0-alpine

# WORKDIR /app

# dockerfile in frontend folder: use alpine instead of slim and use multistage build
# for more cache and image with more relevant content image

FROM python:3.12.2-alpine3.19 as build-image

WORKDIR '/app'

RUN apk add --no-cache linux-headers g++

COPY requirements.txt ./

RUN pip wheel --wheel-dir=/root/wheels -r requirements.txt



FROM python:3.12.2-alpine3.19 as production-image

WORKDIR '/app'

COPY --from=build-image /root/wheels /root/wheels

COPY --from=build-image /app/requirements.txt ./

RUN addgroup -S uwsgi && adduser -S uwsgi -G uwsgi

RUN pip install --no-index --find-links=/root/wheels -r requirements.txt

COPY ./ ./

USER uwsgi

CMD ["python", "app.py"]