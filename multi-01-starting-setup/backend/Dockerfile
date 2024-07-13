FROM node

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

ENV MONGODB_USER=mongoadmin
ENV MONGODB_PWD=secret

CMD [ "npm", "start" ]