# Certbot cheatsheet

## Create a ssl certificate

- Generate a new CSR and copy it to a new text file, e.g. cert.csr

`$ sudo certbot certonly --manual --csr cert.csr`

- Follow the prompts. It will tell you to place a file containing specific data in a specific location of your web site hosted on your domain.
- Upon confirmation, it will generate the cert, which you then install on your web server.
