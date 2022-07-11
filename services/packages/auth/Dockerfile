FROM node:14-alpine
ENV APP_DIR /var/www/
ENV BUILD_DIR /home/www/
WORKDIR ${BUILD_DIR}
RUN set -ex && \
    apk add --no-cache --virtual build-deps alpine-sdk python3
COPY ./services/packages/auth/ ${BUILD_DIR}/packages/auth/
COPY ./services/packages/shared/ ${BUILD_DIR}/packages/shared/
COPY ./services/packages/db/ ${BUILD_DIR}/packages/db/
COPY ./services/*.json ${BUILD_DIR}
COPY ./services/*.js ${BUILD_DIR}
COPY ./services/yarn.lock ${BUILD_DIR}
WORKDIR ${BUILD_DIR}
RUN set -ex && \
    rm -rf ./packages/shared/dist/ && \
    rm -rf ./packages/db/dist/ && \
    echo "setting up server..." && \
    yarn install && \
    yarn workspace @san/shared run build && \
    yarn workspace @san/db run build && \
    yarn workspace auth run build && \
    cp -r ./packages/auth/dist/ ${APP_DIR} && \
    rm -rf ./node_modules/@san/shared && \
    rm -rf ./node_modules/@san/db && \
    cp -r ./node_modules ${APP_DIR}
WORKDIR ${BUILD_DIR}packages
RUN set -ex && \
    mkdir ${APP_DIR}node_modules/@san/shared && \
    cp -r ./shared/dist/* ${APP_DIR}node_modules/@san/shared/ && \ 
    mkdir ${APP_DIR}node_modules/@san/db && \
    cp -r ./db/dist/* ${APP_DIR}node_modules/@san/db/



# WORKDIR /usr/src/app
# COPY ./package*.json /usr/src/app/
# RUN npm install
# COPY ./ /usr/src/app/
# EXPOSE 8889
CMD [ "node", "index.js" ]