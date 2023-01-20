FROM node:14

EXPOSE 3001

WORKDIR /back-end

ADD node_modules.tar.gz ./

COPY ./ ./

ENTRYPOINT ["npm", "start"]
