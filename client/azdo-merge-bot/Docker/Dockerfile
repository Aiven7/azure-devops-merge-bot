# Etap buildowania aplikacji Azure DevOps Extension (Node.js/webpack)
FROM node:20 AS build
WORKDIR /app

# Kopiuj package.json i package-lock.json (jeśli istnieje) osobno dla lepszego cache
COPY ../package.json ./
COPY ../package-lock.json ./

# Instaluj zależności - dodaj build tools dla kompatybilności
RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip \
    python3-dev \
    build-essential \
    make \
    g++ \
    && rm -rf /var/lib/apt/lists/*

# Konfiguruj Python dla node-gyp
RUN ln -sf /usr/bin/python3 /usr/bin/python
RUN npm install -g node-gyp@latest

# Instaluj zależności npm z poprawną konfiguracją Python
ENV PYTHON=/usr/bin/python3
RUN npm install

# Kopiuj resztę kodu
COPY .. .

# Buduj aplikację (webpack)
RUN npm run build || npx webpack --config webpack.config.js


# Etap eksportu builda do folderu hosta
FROM build AS export
WORKDIR /app

# Kopiuj wynik builda do folderu docelowego na hostcie (client/build)
# Użycie polecenia docker cp po zbudowaniu obrazu:
# docker build -t azdo-merge-bot .
# docker create --name temp azdo-merge-bot
# docker cp temp:/app/dist ../build
# docker rm temp

# Ten etap nie uruchamia serwera, tylko przygotowuje artefakty builda
