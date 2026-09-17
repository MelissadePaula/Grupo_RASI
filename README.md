# Grupo_RASI
Trabalho avaliativo referente ao 3º Bimestre da matéria RASI do ano de  2026 


PARTE A — Configurando a rede
1. Configurando o modo Bridge

Primeiramente, configure a placa de rede da máquina virtual em modo Bridge.

Na configuração da placa de rede:

Ligado a: Placa em modo Bridge
Nome: Realtek PCIe GbE Family Controller
Tipo de Placa: Intel PRO/1000 MT Desktop (82540EM)
Promiscuous Mode: Recusar
Virtual Cable Connected: ativado
2. Descobrindo o próprio IP

Abra o terminal e execute:

hostname -I

O resultado apresentado foi:

10.125.131.156 172.17.0.1

Utilize o endereço:

10.125.131.156
3. Conectando ao servidor da máquina hospedeira

No computador hospedeiro, abra o terminal e execute:

ssh aluno@10.125.131.156

Quando aparecer a pergunta:

Are you sure you want to continue connecting (yes/no/[fingerprint])?

Digite:

yes

Em seguida, informe a senha do usuário.

Após a autenticação, a conexão com o servidor será estabelecida.

PARTE B — Docker
4. Atualizando o sistema

No terminal, execute:

sudo apt update

Aguarde a atualização dos pacotes.

5. Verificando a versão do Docker

Execute:

docker -version

O terminal apresentará a versão ou as informações relacionadas ao Docker.

6. Testando o Docker

Execute:

docker run hello-world

O Docker iniciará o container de teste e apresentará a mensagem:

Hello from Docker!
This message shows that your installation appears to be working correctly.

PARTE C — Criando o projeto Flask
7. Instalando a biblioteca tree

Execute:

sudo apt install tree
8. Criando a pasta do projeto

Crie a pasta principal:

mkdir projeto-flask

Depois, crie a pasta da aplicação:

mkdir projeto-flask/app
9. Criando os arquivos

Crie o Dockerfile:

touch projeto-flask/Dockerfile

Crie o arquivo da aplicação Flask:

touch projeto-flask/app/app.py

Crie o arquivo de dependências:

touch projeto-flask/app/requirements.txt

A estrutura do projeto ficará:

projeto-flask/
├── Dockerfile
└── app/
    ├── app.py
    └── requirements.txt

10. Configurando o Dockerfile

Abra o arquivo Dockerfile e escreva:

FROM python:3.14-slim
WORKDIR /app
COPY app/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY app/ .
EXPOSE 5000
CMD ["python", "app.py"]

Salve o arquivo.

11. Construindo a imagem

Entre na pasta do projeto:

cd projeto-flask

Depois execute:

docker build -t flask-app .

A imagem flask-app será construída a partir do Dockerfile.

12. Mapeando a porta

Execute:

docker run -d -p 5000:5000 --name meu-flask flask-app

O Docker iniciará o container utilizando a porta 5000.

13. Acessando a aplicação Flask

Abra o navegador e acesse:

10.125.131.156:5000

A aplicação Flask será exibida no navegador com a mensagem:

Minha primeira aplicação Flask

Aplicação executando dentro de um container Docker

14. Verificando o container

No terminal, execute:

docker ps

O comando exibirá os containers em execução e o mapeamento da porta:

0.0.0.0:5000->5000/tcp
15. Verificando os logs do Flask

Execute:

docker logs meu-flask

Os logs mostrarão a execução do Flask:

* Serving Flask app 'app'
* Debug mode: off
* Running on all addresses (0.0.0.0)
* Running on http://127.0.0.1:5000
* Running on http://172.17.0.2:5000

Também serão registrados os acessos realizados pelo navegador:

GET / HTTP/1.1 200
GET /favicon.ico HTTP/1.1 404
