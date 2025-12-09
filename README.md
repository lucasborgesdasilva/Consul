# Consul

## O que é Consul?

Consul é uma ferramenta de malha de serviços (service mesh) desenvolvida pela HashiCorp que fornece descoberta de serviços, configuração centralizada e segmentação de rede.

## Principais Funcionalidades

- **Service Discovery**: Registro automático e descoberta de serviços
- **Health Checking**: Monitoramento contínuo da saúde dos serviços
- **Key-Value Store**: Armazenamento de configurações distribuído
- **DNS Interface**: Interface DNS integrada para resolução de serviços

### Pré-requisitos
- Docker instalado e em execução

### Comando para Iniciar

```bash
docker compose up -d
```

### Acessar o Servidor Consul

```bash
docker exec -it <id-ou-nome-do-container> sh  
```

### Verificar o ip para a criação do server

```bash
ifconfig
```

### Criar e executar o server 

```bash
  consul agent -server -bootstrap-expect=3 -node=consulserver01 -bind=172.22.0.2 -data-dir=/var/lib/consul -config-dir=/etc/consul.d
```
- **-bootstrap-expect=** (Indica quantos servers eu estou esperando)
- **-node=**             (Nome do nosso server)
- **-bind=**             (Informar o ip consultado anteriormente)
- **-data-dir=**         (Diretório onde o consul guarda os arquivos dele)
- **-config-dir=**       (Diretório onde ficam os arquivos de configuração)

**"Dica importante"**
- Antes de rodar esse comando é necessário ter os diretórios informados criados. Rode primeiro o mkdir antes do consul.

```bash
  mkdir /etc/consul.d
  mkdir /var/lib/consul
```

### Verificar se o server esta de pé

```bash
  consul members
```

### Criar o Cluster

- Após Criar cada um dos servidores, para criar o cluster é necessáro fazer o join desses servers.
  Basta passar o ip dos outros servers que ele adiciona, e ao consultar novamente os servidores estarão sendo exibidos.

```bash
  consul join <ip-do-servidor>
```

### Criar um client
```bash
  consul agent -bind=172.22.0.4 -data-dir=/var/lib/consul -config-dir=/etc/consul.d
```

### Informar onde o serviço está registrado
```bash
consul catalog nodes -service <nome-do-serviço>
```

### retry-join
- Serve para quando subirmos o nosso client, automaticamente ele adiciona no cluster. Podemos passar quantos retry-joins
  nos quisermos.
```bash
  consul agent -bind=172.22.0.4 -data-dir=/var/lib/consul -config-dir=/etc/consul.d -retry-join=172.22.0.2
```
