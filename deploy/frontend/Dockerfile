FROM node:18-alpine

ARG AWS_ACCESS_KEY_ID
ARG AWS_DEFAULT_REGION
ARG AWS_SECRET_ACCESS_KEY

RUN echo "region $AWS_DEFAULT_REGION"

RUN apk add --no-cache python3 py3-pip

RUN python3 -m venv /venv
ENV PATH="/venv/bin:$PATH"

RUN pip install awscli

WORKDIR /app

COPY budget-tracker-vue/package*.json ./
RUN npm install

COPY budget-tracker-vue ./

RUN npm run build

RUN aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
RUN aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
RUN aws configure set default.region $AWS_DEFAULT_REGION
RUN aws --endpoint-url=https://storage.yandexcloud.net s3 rm --recursive s3://frontend-budget-tracker
RUN aws --endpoint-url=https://storage.yandexcloud.net s3 cp --recursive dist/ s3://frontend-budget-tracker/
