# Teleport com Docker – Guia Rápido

## Subindo o ambiente

```bash
docker-compose up --build
```

Acesse o container do servidor:

```bash
docker exec -it server bash
```

## Criando o usuário administrador

Dentro do container, execute:

```bash
tctl users add admin --roles=editor,access --logins=root,admin
```

Após executar o comando acima, será exibida uma saída semelhante a esta:

```text
2026-01-16T01:12:56Z WARN Starting Teleport with a self-signed TLS certificate, this is not safe for production clusters. Using a self-signed certificate opens Teleport users to Man-in-the-Middle attacks. config/configuration.go:1119
User "admin" has been created but requires a password. Share this URL with the user to complete user setup, link is valid for 1h:
https://debian-server:443/web/invite/c0d23b99e84f625213e9ce97b583e10f

NOTE: Make sure debian-server:443 points at a Teleport proxy which users can access.
```

### Ajustando a URL

Altere o endereço `debian-server` para `localhost` na URL gerada:

```text
https://localhost:443/web/invite/c0d23b99e84f625213e9ce97b583e10f
```

Ao acessar esse link, será possível criar a senha de acesso para o usuário **admin**.

## Acessando o Teleport

Após definir a senha, acesse a interface web do Teleport em:

```
https://localhost/
```

## Adicionando nodes ao Teleport

Para gerar um token de inclusão de novos nodes, execute:

```bash
docker exec -it server tctl tokens add --type=node
```

O comando retornará algo semelhante a:

```bash
teleport start \
  --roles=node \
  --token=ba34b8809e96611983178d10d9568967 \
  --ca-pin=sha256:51d0b08d465c60a3204baac23eb14a36ad0cb7c928586d7a241c1aedc13ee974 \
  --auth-server=172.18.0.2:3025
```

### Registrando o node

**no cluster que será adicionando, deverá está instalando o agente do teleport**

exemplo para almalinux:9

```bash
dnf config-manager --add-repo https://rpm.releases.teleport.dev/teleport.repo
dnf repolist
dnf install teleport
```

Execute o comando acima **dentro do node que será adicionado** ao cluster Teleport.

## Fontes

- https://linuxgenie.net/install-teleport-on-ubuntu-22-04/
- https://goteleport.com/docs/installation/linux/
