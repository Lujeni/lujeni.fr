FROM nginxinc/nginx-unprivileged:1.18-alpine as run-stage
COPY public/ /usr/share/nginx/html/
EXPOSE 8080
CMD ["nginx", "-g", "daemon off;"]
