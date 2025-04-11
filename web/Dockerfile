FROM nginx:1.21.6

# Expose port 80 (or 8080 if you've updated the config)
EXPOSE 80

# Environment variables used for substitution in templates
ENV CATALOGUE_HOST=catalogue \
    USER_HOST=user \
    CART_HOST=cart \
    SHIPPING_HOST=shipping \
    PAYMENT_HOST=payment \
    RATINGS_HOST=ratings

# Copy files
COPY entrypoint.sh /root/
RUN chmod +x /root/entrypoint.sh

COPY default.conf.template /etc/nginx/conf.d/default.conf.template
COPY static /usr/share/nginx/html

# Set entrypoint
ENTRYPOINT ["/root/entrypoint.sh"]
