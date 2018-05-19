FROM node:9

RUN git clone https://github.com/cam-inc/viron.git

WORKDIR viron
RUN yarn

ENTRYPOINT ["npm", "start"]
