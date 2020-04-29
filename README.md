# Dockerfiles

Collection of useful dockerfiles

## Docker Compose Usage 

```
docker-compose up -d --build
```

## Moodle

```
git clone -b MOODLE_38_STABLE git://git.moodle.org/moodle.git
```

## Development Certificates

Using mkcert you can generate the client development certificates and update your hosts file.

```sh
mkcert -key-file certificate.key -cert-file certificate.crt elevatelearning.com "*.elevatelearning.com" elevatelearning.test localhost 127.0.0.1 ::1
```
