FROM nginx:latest

# Remove default nginx page
RUN rm -rf /usr/share/nginx/html/*

# Copy our HTML
COPY index.html /usr/share/nginx/html/

EXPOSE 80