FROM node:20-alpine

# Creation a sustem useer, want the users to run app and not root users
RUN addgroup app && adduser -S app -G app app

USER app

WORKDIR /app

COPY package*.json ./

USER root

RUN chown -R app:app /app
USER app

RUN npm install

COPY . .

EXPOSE 5173

CMD ["npm", "run", "dev"]


