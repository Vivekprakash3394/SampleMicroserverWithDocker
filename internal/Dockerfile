FROM node
COPY . /tmp/internal/
WORKDIR /tmp/internal
EXPOSE 8082
RUN ["npm","install"]
CMD ["node","server.js"]
