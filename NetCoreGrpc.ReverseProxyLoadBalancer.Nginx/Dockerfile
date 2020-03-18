FROM nginx:1.16.1
WORKDIR /app
EXPOSE 443
COPY ./configuration/default.conf /etc/nginx/conf.d/default.conf
COPY ./configuration/snippets /etc/nginx/snippets
COPY ./certificates/server.crt /etc/ssl/certs/server.crt
COPY ./certificates/server.key /etc/ssl/private/server.key
CMD ["nginx", "-g", "daemon off;"]