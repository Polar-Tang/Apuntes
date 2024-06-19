Si la versión que tenés no es la última
```
apt remove docker-compose
```
```
curl -L "https://github.com/docker/compose/releases/download/v2.16.0/docker-compose-$(uname -s)-$(uname -m)" -o docker-compose
```
```
chmod +x docker-compose
```
Asegurate que este en la carpeta correcta para ser ejecutado como binario:
```
mv ./docker-compose /usr/local/bin/docker-compose
```
