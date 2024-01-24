FROM node

WORKDIR /nodeapp

COPY package.json /nodeapp

RUN npm install

COPY . ./

EXPOSE 80

CMD ["node", "server.js"]


