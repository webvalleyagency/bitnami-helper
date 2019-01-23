# Let's Encrypt certificates on bitnami

## Prerequsities

### Lego

Install lego let's encrypt wrapper following this tutorial https://docs.bitnami.com/aws/how-to/generate-install-lets-encrypt-ssl/

### First request

Run this command:

```
sudo lego --email="notifications@webvalley.cz" --domains="domain.com" --domains="www.domain.com"  --webroot="/opt/bitnami/apps/domain-com/htdocs/www/" --path="/etc/lego" --accept-tos run
```

Add this to httpd-vhosts.conf

```
SSLEngine on
SSLCertificateFile /etc/lego/certificates/domain.com.crt
SSLCertificateKeyFile /etc/lego/certificates/domain.com.key
```

## Automatic Renewal

Add following code to root crontab.

```
MAILTO="notifications@webvalley.cz"
0 3 */15 * * lego --email="notifications@webvalley.cz" --domains="domain.com" --domains="www.domain.com"  --webroot="/opt/bitnami/apps/domain-com/htdocs/www/" --path="/etc/lego" --accept-tos renew --days 30 && /opt/bitnami/apache2/bin/apachectl -k graceful
0 3 */15 * * lego --email="notifications@webvalley.cz" --domains="2domain.net" --webroot="/opt/bitnami/apps/2domain-net/htdocs/web/" --path="/etc/lego" --accept-tos renew --days 30 && /opt/bitnami/apache2/bin/apachectl -k graceful
```


