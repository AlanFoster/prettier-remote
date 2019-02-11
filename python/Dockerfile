FROM labhackercd/alpine-python3-nodejs:latest

EXPOSE 8080

RUN mkdir -p /code
ADD package.json yarn.lock /code/

WORKDIR /code
RUN yarn install

ADD server /code/
ENTRYPOINT ["./server"]
