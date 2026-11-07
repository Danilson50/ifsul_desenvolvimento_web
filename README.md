# Laboratório cliente-servidor ASKAM

O Nginx serve os arquivos do site. O navegador dentro do LinuxServer Webtop atua como cliente. Os dois containers usam a mesma rede Docker; nela, o nome `nginx` identifica o servidor.

## Iniciar

Na sua maquina, inicie o Docker usando containers Linux. Abra um terminal nesta pasta e execute:

```bash
docker compose up -d
docker compose ps
```

A primeira execução baixa as imagens e pode demorar, especialmente a do Webtop.

1. No navegador da sua maquina, abra **https://localhost:3001** para acessar o desktop do Webtop. O certificado é autoassinado; aceite a exceção para este ambiente local.
2. Dentro do Webtop, abra o navegador e acesse **http://nginx**. Esta é a comunicação cliente-servidor entre os containers.
3. Para comparar, acesse **http://localhost:8080** diretamente no navegador do Windows.

## Alterações e teste

HTML, CSS, JavaScript e imagens são montados no Nginx em modo de leitura. Depois de editar os arquivos locais, atualize o navegador com Ctrl + F5; não é necessário reconstruir uma imagem.

Para acompanhar as requisições recebidas pelo servidor:

```bash
docker compose logs -f nginx
```

Abra o site no navegador do Webtop e observe as requisições GET no terminal. Ctrl + C encerra apenas a visualização dos logs.

Para conferir o acesso a partir do cliente pelo terminal do Webtop:

```bash
curl -I http://nginx
```

O resultado esperado é `HTTP/1.1 200 OK`. No navegador, a aba Network/Rede das ferramentas de desenvolvedor também mostra as requisições de HTML, CSS e imagens.

As fontes do Google usadas pelo site precisam de acesso à internet; sem ele, o CSS usa as fontes alternativas já configuradas.

## Parar e diagnosticar

```bash
docker compose down
```

Isso preserva o volume com as configurações do Webtop. Para consultar falhas de inicialização:

```bash
docker compose logs --tail=100 webtop
docker compose logs --tail=100 nginx
```

Se uma porta estiver ocupada, altere apenas o número publicado no Compose (8080 ou 3001, antes dos dois-pontos) e use esse novo número no navegador do Windows.
