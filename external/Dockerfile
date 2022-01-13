FROM node
COPY . /tmp/external/
WORKDIR /tmp/external
EXPOSE 8080
RUN ["npm","install"]
CMD ["npm","start"]
