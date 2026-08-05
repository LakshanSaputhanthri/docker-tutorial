FROM node:20.16.0-alpine3.20
RUN addgroup app && adduser -S -G app app
USER app
WORKDIR /app
COPY --chown=app:app . .
RUN npm install
ENV BACKEND_URL=http://localhost:8000
EXPOSE 3000
CMD ["npm", "run" ,"dev"]