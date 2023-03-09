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

```
http {
	server {
		listen 8080; ############# Quando o usuário acessar <IP>/8080, será redirecionado para o index.html localizado |
		root /tmp; <------------------------------------------------------------------------------------ nessa pasta ---
	}

	server {
		listen 80; ############### O mesmo princípio aqui...
		root /tmp/404;
	}
}

events {} #################### Esse bloco é necessário, não sei o porquê.
```

