FROM nginx:stable

COPY build/web /usr/share/nginx/html

# RUN rm /etc/nginx/conf.d/default.conf
# COPY nginx.conf /etc/nginx/conf.d

CMD nginx -g "daemon off;"