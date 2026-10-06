# Respostas · Avaliação Prática de Docker · ViaSerra Transportes (Turma C)

Nome: Lucas Siqueira Apolinario
Matrícula: 26175364
Usuário do GitHub: Apolinario-coder
Usuário do Docker Hub: lukass2

Responda com as suas palavras e com o que aconteceu na SUA máquina. Resposta curta e certa vale mais
do que texto longo copiado. Resposta que contradiz o seu próprio Dockerfile vale zero.

## Parte 1 · Dockerfile do portal

1. Qual imagem base você usou e qual o tamanho final da imagem do portal (saída de `docker images`)?
`nginx:1.27-alpine`. O tamanho final da imagem é 21MB (tamanho de conteúdo) / 73.6MB (espaço em disco).

2. Em qual pasta do container o Nginx procura os arquivos do site? Mostre o comando que você usou para
   conferir que o `index.html` está lá dentro.
Na pasta `/usr/share/nginx/html`.
Comando utilizado para conferir:
`docker exec atividade-portal-1 ls -la /usr/share/nginx/html`

## Parte 2 · Docker Hub

3. Nome completo da imagem publicada e link público do repositório no Docker Hub.
Nome completo: `lukass2/viaserra-portal:1.0-26175364`
Link público: https://hub.docker.com/r/lukass2/viaserra-portal

4. Se você mudar o HTML, quais comandos precisa rodar para que a versão nova chegue ao Docker Hub?
`docker build -t lukass2/viaserra-portal:1.0-26175364 ./portal`
`docker push lukass2/viaserra-portal:1.0-26175364`

## Parte 3 · Página de manutenção

5. Preencha uma linha por defeito encontrado. Defeito inexistente listado aqui desconta pontos.

| # | Instrução | O que estava errado | O que você viu acontecer | Como corrigiu |
|---|---|---|---|---|
| 1 | `COPY pagina/ .` | O diretório de origem no host é `site/` e não `pagina/` | Falha no `docker build`: erro informando que o diretório de origem `/pagina` não foi encontrado | Alterado para `COPY site/ .` |
| 2 | `CMD ["nginx"]` | O Nginx por padrão entra em segundo plano (daemon) e encerra o processo principal (PID 1) | O container finalizou logo após iniciar (`Exited (0)` no `docker ps -a`) | Alterado para `CMD ["nginx", "-g", "daemon off;"]` |
| 3 | `WORKDIR /usr/share/nginx` | O Nginx serve arquivos por padrão em `/usr/share/nginx/html` e não `/usr/share/nginx` | O container subiu, mas exibia a página padrão "Welcome to nginx!" em vez da página de manutenção | Alterado o WORKDIR para `/usr/share/nginx/html` |

6. Qual a diferença entre `-p 7042:80` e `-p 80:7042` no `docker run`? Qual dos dois números é a porta do container?
A sintaxe do parâmetro `-p` é `<porta-do-host>:<porta-do-container>`.
- Em `-p 7042:80`, a porta 7042 da máquina host é mapeada para a porta 80 do container.
- Em `-p 80:7042`, a porta 80 da máquina host é mapeada para a porta 7042 do container.
O número após os dois pontos é a porta do container (em `-p 7042:80`, é a porta 80; em `-p 80:7042`, é a 7042).

## Parte 4 · Primeiro docker-compose

7. Escreva os dois comandos `docker run` que fariam o mesmo que o seu `docker-compose.yml`.
```bash
docker run -d --name portal -p 8064:80 --restart unless-stopped lukass2/viaserra-portal:1.0-26175364
docker run -d --name manutencao -p 7064:80 --restart unless-stopped atividade-manutencao
```

8. Qual comando derruba os dois containers de uma vez?
`docker compose down`

## Verificador

9. Código de conclusão impresso pelo verificador:

```
VIASERRA-26175364-4B938587
```
