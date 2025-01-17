FROM python:3.9-slim AS base

ENV PATH "/opt/venv/bin:$PATH"
ENV PYTHONDONTWRITEBYTECODE True
ENV PYTHONUNBUFFERED True

FROM base AS builder
RUN python -m venv /opt/venv
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
RUN apt-get update && apt-get install -y wget \
    && wget -O wait-for-it.sh https://raw.githubusercontent.com/vishnubob/wait-for-it/master/wait-for-it.sh \
    && chmod +x wait-for-it.sh

FROM base
WORKDIR /app
COPY --from=builder wait-for-it.sh /usr/local/bin/
COPY --from=builder /opt/venv /opt/venv
COPY docker-entrypoint.sh /usr/local/bin/
COPY main.py .

ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["python", "main.py"]