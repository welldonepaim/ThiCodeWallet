# Deploy em VM com Reverse Proxy (HTTPS)

Este projeto agora tem uma stack de producao separada em `docker-compose.prod.yml`.

## 1) O que essa stack faz

- `proxy` (Caddy): recebe trafego publico na porta 80/443.
- `api`: fica privada na rede Docker, sem exposicao publica.
- `mongo`: banco privado na rede Docker.
- Certificado HTTPS: emitido e renovado automaticamente pelo Caddy (Let's Encrypt).

## 2) Pre-requisitos da VM

- Docker + Docker Compose instalados.
- Dominio configurado no DNS com registro `A` apontando para o IP publico da VM.
- Firewall liberando portas `80` e `443`.

## 3) Configurar variaveis de producao

Crie o arquivo `.env.prod`:

```bash
cp .env.prod.example .env.prod
```

Edite com seu dominio real:

```env
DOMAIN=wallet.seudominio.com
LETSENCRYPT_EMAIL=seu-email@dominio.com
```

## 4) Subir em producao

Se voce ja estiver com a stack local em execucao, pare antes para liberar as portas 80/443:

```bash
docker compose down
```

```bash
docker compose --env-file .env.prod -f docker-compose.prod.yml up -d --build
```

## 5) Verificar

```bash
docker compose --env-file .env.prod -f docker-compose.prod.yml ps
docker compose --env-file .env.prod -f docker-compose.prod.yml logs -f proxy
```

Acesse:

- `https://SEU_DOMINIO`

## 6) Como o reverse proxy funciona aqui

- Frontend chama `VITE_API_BASE_URL=/api`.
- Navegador acessa `https://SEU_DOMINIO/api/...`.
- O proxy recebe e encaminha internamente para `api:5002`.
- Isso evita expor a API diretamente e centraliza seguranca/HTTPS no proxy.

## 7) Atualizar aplicacao

```bash
git pull
docker compose --env-file .env.prod -f docker-compose.prod.yml up -d --build
```
