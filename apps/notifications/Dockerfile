FROM node:alpine As development

WORKDIR /usr/src/app

COPY package.json ./
COPY yarn.lock ./
COPY tsconfig.json tsconfig.json
COPY nest-cli.json nest-cli.json

RUN npm install yarn

COPY apps/notifications apps/notifications
COPY libs libs

RUN yarn install -r

RUN yarn build notifications

FROM node:alpine as production

ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}

WORKDIR /usr/src/app

COPY package.json ./
COPY yarn.lock ./

RUN npm install yarn

RUN yarn install --production --ignore-engines

COPY --from=development /usr/src/app/dist ./dist

CMD ["node", "dist/apps/notifications/main"]