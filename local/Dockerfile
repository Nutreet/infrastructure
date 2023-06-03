FROM openjdk:21-bullseye

WORKDIR /app

# Install Node.js
RUN apt-get update -y && \
    apt-get install -y apt-transport-https curl software-properties-common && \
    curl -sL https://deb.nodesource.com/setup_16.x | bash - && \
    apt-get install -y nodejs

# Install the Firebase CLI
RUN npm i -g firebase-tools

# Download all emulator JAR files
RUN firebase setup:emulators:database && \
    firebase setup:emulators:firestore && \
    firebase setup:emulators:pubsub && \
    firebase setup:emulators:storage && \
    firebase setup:emulators:ui

# Redirect logs to Docker stdout
RUN ln -sf /dev/stdout firestore-debug.log && \
    ln -sf /dev/stdout pubsub-debug.log && \
    ln -sf /dev/stdout ui-debug.log

# Copy Firebase configuration files
COPY firebase.json ./
COPY .firebaserc ./

CMD firebase emulators:start --import=/data/firebase-export --export-on-exit
