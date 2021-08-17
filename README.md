# Meetup-Avalanche

Meetup de Avalanche impartido por [Andrea Vargas](https://twitter.com/andyvargtz?s=21) Consultora Blockchain de Avalanche en Español donde nos explica qué es Avalanche y qué viene a solucionar, con un posterior workshop impartido por [wimel](https://delega.io) donde explicaremos como iniciar un nodo en la red para ser validadores o simplemente para gestionar nuestros fondos.

El video del meetup se puede ver [aquí](https://odysee.com/@colmenasvq:0/meetupAvalanche:9)

## Workshop, cómo levantar un nodo en la red de Avalanche:

### Instalamos dependencias:
```sh
sudo apt install -y git make gcc build-essential
```

### Instalamos Go *(comprobar qué versión de Go es necesaria)*:
```sh
wget -c 'https://dl.google.com/go/go1.16.3.linux-amd64.tar.gz' -O go1.16.3.linux-amd64.tar.gz && sudo tar -C /usr/local/ -xzf go1.16.3.linux-amd64.tar.gz


sudo rm -Rf go1.16.3.linux-amd64.tar.gz
```

>Añadimos lo siguiente al `.bashrc` o al `.profile`:

    #Go:
    export PATH="$PATH:/usr/local/go/bin"
    export GOPATH="$HOME/go"
    export PATH="$PATH:$GOROOT/bin:$GOPATH/bin"
    export GOBIN="$GOPATH/bin"

### Clonamos repositorio y entramos en la carpeta:
```sh
git clone https://github.com/ava-labs/avalanchego.git && cd avalanchego/
```

### Seleccionamos la rama y compilamos *(comprobar la última version o la versión necesaria en [su repositorio](https://github.com/ava-labs/avalanchego/releases/))*:
```sh
git checkout v1.4.0

./scripts/build.sh
```

### Iniciamos el nodo para comprobar que funciona, el binario de Avalanche se encuentra en la carpeta `build`:
```sh
cd avalanchego/build/ && ./avalanchego
```

>Si queremos ver todas las opciones disponibles podemos usar el comando `./avalanchego --help`.

### Creamos el servicio de sistema *yo uso `vim` pero pueden usar su editor preferido ej `nano`*:
```sh
sudo vim /etc/systemd/system/ava.service
```

>Dentro añadimos *(recordad cambiar delega por el nombre de usuario en vuestro sistema)*:

```sh
[Unit]
Description=AVA node

[Service]
ExecStart=/home/delega/avalanchego/build/avalanchego --public-ip IP --plugin-dir /home/delega/avalanchego/build/plugins

User=delega
Restart=always
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
```

#### Si el nodo lo tenemos en casa sin IP fija, podemos usar el siguiente flag ([issue](https://github.com/ava-labs/avalanchego/issues/246)):
```sh
--dynamic-public-ip ifconfigco
```

### Iniciamos el servicio de sistema:
```sh
sudo systemctl start ava.service
```

### Activamos el servicio desde el inicio:
```sh
sudo systemctl enable ava.service
```

### Dando un poco de seg a nuestros nodos:

>Para dar un poco de seguridad vamos a usar `ufw` más info [aquí](https://es.wikipedia.org/wiki/Uncomplicated_Firewall), el puerto `9650` sólo nos haría falta en caso de querer usar la web-wallet, aconsejable cambiar el puerto de `ssh` del `22` a cualquier otro:
```sh
ufw status
ufw allow 22/tcp
ufw allow 9651/tcp
ufw enable
ufw status numbered
```

>Archivos de los que debemos tener copias:

El `.crt` y `.key` del servidor para arrancar con el mismo node-id.

> La carpeta con la base de datos se encuentra en `.avalanchego/db/`

---

## Cómo usar nuestro nodo para manejar nuestras wallets y no depender de terceros:

[Repositorio](https://github.com/ava-labs/avalanche-wallet)

### Instalamos dependencias, Yarn, npm y node:
```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
```

### Instalamos y usamos la versión correcta de npm:
```sh
nvm install v12.14.1
```

### Instalamos yarn:
```sh
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -

echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list

sudo apt update && sudo apt install yarn
```

### Clonamos repositorio, entramos dentro de la carpeta y compilamos:
```sh
git clone https://github.com/ava-labs/avalanche-wallet.git && cd avalanche-wallet

yarn install
```

### Iniciamos el servidor de desarrollo:
```sh
yarn serve
```

>Si todo ha ido bien podemos abrir nuestro navegador en `localhost:5000` y veremos la web, recordar que si el nodo no está sincronizado no podremos usarlo al 100%.



## Enlaces:

[Canal de Telegram en español](https://t.me/avalanche_es)

[Canal de anuncios de Avalanche (en español)](https://t.me/avalanche_es_an)

[Medium](https://medium.com/avalancheavax)

[Web](https://www.avalabs.org/)

[Documentación](https://docs.avax.network/)

[Repositorio](https://github.com/ava-labs)

[Foro](https://forum.avax.network/)

[Hub Avalanche](https://community.avax.network/accounts/login/)

[Guía para delegar fondos *(en español)*](https://github.com/wimel/Delegar-fondos-en-Avalanche-AVAX)
