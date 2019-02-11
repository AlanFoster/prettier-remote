FROM node:8.15.0-alpine

EXPOSE 8080

RUN mkdir -p /code
ADD package.json yarn.lock /code/

WORKDIR /code
RUN yarn install

ADD server /code/
ENTRYPOINT ["./server"]
