
Jogo Multiplayer de senha

Este projeto é um jogo multiplayer simples rodando em containers Docker. O jogador deve adivinhar uma senha criada aleatoriamente, e o sistema fornecerá feedback sobre o número de letras corretas e suas respectivas posições.

## Requisitos

Antes de começar, certifique-se de que você tem as seguintes ferramentas instaladas:

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Funcionalidades

Utilizamos **Docker Compose** para criar múltiplos containers de backend para balanceamento. O objetivo é distribuir as requisições entre diferentes instâncias do backend.

## Instalação

Siga os passos abaixo para instalar e rodar o projeto em sua máquina:

### 1. Clone o repositório

```bash
git clone https://github.com/maurizb/trabalhodocker2.git
cd trabalhodocker2
```

### 2. Verifique e ajuste as configurações

O arquivo `docker-compose.yml` e o arquivo de configuração do Nginx (`nginx.conf`) já estão configurados para o balanceamento de carga (**NÃO IMPLEMENTADO**).

- O `docker-compose.yml` define dois containers de backend (`backend1` e `backend2`) rodando a mesma aplicação. (**NO MOMENTO APENAS UM**).
- O arquivo `nginx.conf` define a configuração do proxy reverso para distribuir as requisições entre os containers.

Se quiser, ajuste as configurações de portas, número de containers, ou outras variáveis.

### 3. Rodar o projeto

Para rodar o projeto, utilize o seguinte comando:

```bash
docker-compose up --build
```

Isso irá construir as imagens e subir os containers do jogo. A aplicação estará disponível em [http://localhost](http://localhost).

### 4. Escalar o backend (opcional)

Se você quiser aumentar o número de containers backend rodando o jogo, pode usar o seguinte comando para escalar:

```bash
docker-compose up --scale backend1=3 --scale backend2=3
```

Isso criará mais instâncias dos containers de backend, e o Nginx irá balancear a carga entre eles.

## Como Jogar

1. Abra o navegador e acesse [http://localhost](http://localhost). Digite uma frase secreta, envie, e salve o game-id.

2. Para adivinhar a senha, entre com o game_id que foi gerado no passo acima e tente adivinhar.
3. Toda vez que você interagir com o jogo, o Nginx irá redirecionar sua requisição para um dos containers de backend rodando no ambiente.
   
### Testar o balanceamento de carga

- Cada requisição ao backend é balanceada entre múltiplos containers. Se você abrir a aba do desenvolvedor no seu navegador (normalmente pressionando `F12`), poderá observar o tráfego de rede sendo distribuído.
- O comportamento pode ser testado ao abrir várias abas ou acessando repetidamente a aplicação.

## Encerrando o Jogo

Para parar todos os containers e excluir os volumes, executar:

```bash
docker compose down --volumes
```

Isso irá desligar e remover todos os containers criados.