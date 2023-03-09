# Instalação NGINX no Ubuntu

https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-20-04

# Arquivos importantes e sua localidade

**nginx.conf**

O **nginx.conf** é o principal arquivo de configuração do NGINX, e geralmente pode ser encontrado em `/etc/nginx/nginx.conf`. Caso não esteja lá, é possível usar o comando `sudo find / -iname "nginx.conf"`

# Servindo um arquivo HTML simples

Iremos criar um arquivo HTML simples, em $HOME/index.html

```html
<html>
    <head>
        <title>Welcome to this simple website!</title>
    </head>
    <body>
        <h1>This is being served via NGINX.</h1>
    </body>
</html>
```

Podemos redefinir o arquivo **nginx.conf** para ter as seguintes configurações:

```nginx
http {
	server {
		listen 8080; ############# Ao acessar <IP>/8080, seremos redirecionados para o index.html localizado |
		root /tmp; # <-------------------------------------------------------------------------- nessa pasta -
	}

	server {
		listen 80; ############### O mesmo princípio aqui...
		root /tmp/404;
	}
}

events {} #################### Esse bloco é necessário, não sei o porquê.
```

# Garantindo que diferentes arquivos serão servidos com o Content Type correto

Para que o NGINX entregue o diferentes extensões de arquivos com o Content-Type adequado, devemos incluir a seguinte linha em nosso bloco `http`

```nginx

http {
	include /etc/nginx/mime.types
	
	# ...
}
```

O arquivo `/etc/nginx/mime.types` possui uma associação entre o **Content-Type** e a **extensão de arquivos**, para que diferentes extensões sejam servidas com o Content-Type correto:

![image](https://user-images.githubusercontent.com/80921933/224104779-fb457cc5-e4f3-416d-9bbc-1567b31ad2a5.png)

# Servindo conteúdo de subpastas com o bloco location

Também é possível servir conteúdo de subpastas, com a seguinte configuração:

```nginx
http {
	server {
		listen 8080;
		root /tmp;

		location /fruits {
			root /tmp; # /fruits será concatenado, e portanto, buscará o index.html de /tmp/fruits/
		}

	}
}

events {}
```

Isso significa que:
- Ao acessarmos **\<IP>:8080**, seremos direcionados para o **index.html** da pasta **/tmp**
- Ao acessarmos **\<IP>:8080/fruits**, seremos direcionados para o **index.html** da pasta **/tmp/fruits**, porque o que for passado na URL vai ser concatenado ao "root"

