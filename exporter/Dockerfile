FROM node:16 as builder

# install FoundationDB client library
RUN wget -q https://github.com/apple/foundationdb/releases/download/7.1.61/foundationdb-clients_7.1.61-1_amd64.deb \
  && dpkg -i foundationdb-clients_7.1.61-1_amd64.deb \
  && rm foundationdb-clients_7.1.61-1_amd64.deb

WORKDIR /app

COPY . .
RUN npm install
Run npm run build

FROM node:16

# install FoundationDB client library
RUN wget -q https://github.com/apple/foundationdb/releases/download/7.1.61/foundationdb-clients_7.1.61-1_amd64.deb \
  && dpkg -i foundationdb-clients_7.1.61-1_amd64.deb \
  && rm foundationdb-clients_7.1.61-1_amd64.deb

# set up multi-version FoundationDB client
ENV FDB_NETWORK_OPTION_EXTERNAL_CLIENT_DIRECTORY /usr/lib/foundationdb/multiversion

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm install

COPY --from=builder /app/build /app/build

ENTRYPOINT ["node", "build/index.js"]
