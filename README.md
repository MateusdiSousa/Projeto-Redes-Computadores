<div align="center">

# ADS04_ISO200

Repositório para apresentar o projeto desenvolvido para a disciplina Sistemas Operacionais II.

## <a href="https://www.linkedin.com/in/fabiano-sabha-8661b4/" target="Sabha"> Professor Fabiano Sabha Walczak </a>



</div>

# :rocket: Tecnologias Utilizadas

Para o desenvolvimento deste projeto foram utilizadas as seguintes ferramentas e tecnologias:

<table>
  <thead>
    <th>
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/typescript/typescript-original.svg" alt="Alt text" title="TypeScript" style="display: inline-block; margin: 0 auto; width: 60px">
    </th>
    <th>
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/react/react-original.svg" alt="Alt text" title="React" style="display: inline-block; margin: 0 auto; width: 60px"></th>
    <th>
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/nestjs/nestjs-original.svg" alt="Alt text" title="NestJs" style="display: inline-block; margin: 0 auto; width: 60px" />
    <th>
        <img src="https://www.debian.org/logos/openlogo-nd.svg" alt="Alt text" title="Debian" style="display: inline-block; margin: 0 auto; width: 60px">
    </th>
    <th>
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/mysql/mysql-original-wordmark.svg" alt="Alt text" title="React" style="display: inline-block; margin: 0 auto; width: 60px" />
    </th>
    <th>
        <img src="https://github.com/apiFatec/API-3-Semestre-Ionic/assets/112169639/8f7699b6-4ee3-4bfb-a761-f79faa45049d" alt="Alt text" title="Tailwind" style="display: inline-block; margin: 0 auto; width: 60px">
    </th>
    <th>
        <img src="https://img.daisyui.com/images/daisyui-logo/daisyui-logotype.svg" alt="Alt text" title="React" style="display: inline-block; margin: 0 auto; width: 60px">
    </th>
    <th>
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/docker/docker-original.svg" alt="Alt text" title="Docker" style="display: inline-block; margin: 0 auto; width: 60px">
    </th>
    <th>
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/nginx/nginx-original.svg" alt="Alt text" title="Nginx" style="display: inline-block; margin: 0 auto; width: 60px">
    </th>
    <th>
        <img src="https://www.svgrepo.com/show/349466/openvpn.svg" alt="Alt text" title="OpenVPN" style="display: inline-block; margin: 0 auto; width: 60px">
    </th>
  </thead>

  <tbody>
    <td>Typescript</td>
    <td>React</td>
    <td>NestJs</td>
    <td>Debian</td>
    <td>MySQL</td>
    <td>Tailwind</td>
    <td>DaisyUI</td>
    <td>Docker</td>
    <td>Nginx</td>
    <td>OpenVPN</td>
  </tbody>
</table>

# :rocket: Relatório do Projeto de Sistemas Operacionais II

## 1. Introdução

Nesta relatório explicarei o passo-a-passo para a realização do projeto de Redes de Computadores, atendendo os requisitos obrigatórios e hospedando a aplicação web em uma instância EC2 da AWS com o Debian, implementando loadbalancer, VPN e utilizando Docker no desenvolvimento. 

## 2. Configuração da Máquina Virtual na AWS

Para começar, criei 3 instâncias elegíveis para o nível gratuito da AWS utilizando o sistema operacional Debian. A configuração inicial envolveu os seguintes passos:

**1. Criação da Instância:**
   - Através do console da AWS, criei uma nova instância EC2 selecionando o sistema operacional Debian.

**2. Configuração de Acesso:**
   - Configurei a segurança da instância para permitir acessos SSH e HTTP.
   - Gereci as chaves SSH para garantir a segurança do acesso remoto.
  
**3. Grupo de segurança:**
    <p>Nas configurações do grupo de segurança permiti o acesso as seguintes portas:</p>
    - Máquina 1 (Backend)
    <ul>
        <li>3000: Porta que será utilizada pelo backend</li>
    </ul>
    - Máquina 2 (Frontend, podendo ser mais máquinas de acordo com a necessidade)
    <ul>
        <li>80: Porta que será utilizado pelo frontend para hospedar o site</li>
    </ul>
    - Máquina 3 (Loadbalancer e Proxy Reverso)
    <ul>
        <li>1194: Porta que será utilizada para acessar a rede VPN</li>
    </ul>

## 3. Instalação de Serviços Essenciais

### 3.1 Com a instância do FRONTEND configurada e acessível, instalei os seguintes serviços necessários:

   - **Servidor WWW – Apache:**
   ``` bash
   sudo apt update
   sudo apt install apache2
   ```
   - Verifiquei o funcionamento acessando o endereço IPv4 público da instância.
   - **Git:** Para controle de versão e deploy da aplicação.
     ```bash
     sudo apt install git
     ```
   - **NodeJs:** Para rodar o javascript no backend.
     ```bash
     sudo apt install nodejs
     ```
   - **npm:** Para o gerenciamento de pacotes do nodejs.
     ```bash
     sudo apt install npm
     ```
### 3.2 Com a instância do BACKEND configurada e acessível, instalei os seguintes serviços necessários:
- **Git:** Para controle de versão e deploy da aplicação.
     ```bash
     sudo apt-get update
     sudo apt install git
     ```
- **Docker e Docker Compose:** Criação de containers.
     ```bash
     sudo apt install docker.io -y
     sudo apt install docker-compose
     ```

### 3.3 Com a instância do LoadBalancer configurada e acessível, instalei os seguintes serviços necessários:
- **Git:** Para controle de versão e deploy da aplicação.
     ```bash
     sudo apt-get update
     sudo apt install git
     ```
- **OpenVPN:** Criação de VPN.
     ```bash
     sudo apt install openvpn
     ```

- **Nginx:** Servidor WWW.
     ```bash
     sudo apt install nginx
     ```

## :rocket: 4. Rodando o Projeto

### 4.1 Rodando o frontend

```cmd
git clone https://github.com/MateusdiSousa/Projeto-de-Sistemas-Operacionais.git
```

### Configurando o frontend
```cmd
sudo nano Projeto-de-Sistemas-Operacionais/frontend/src/services/api.tsx
```
- E certificar que a url da variável do api esteja correspondente com a url pública da instância do backend:
```cmd
import axios from "axios";

const api = axios.create({baseURL : 'http://{IPV4-INSTÂNCIA DO BACKEND}:3000/'});

export default api
```
- Logo em seguida instale as dependências do frontend.
```cmd
cd Projeto-de-Sistemas-Operacionais/frontend
npm install
```
- Modifique o seguinte arquivo:
```cmd
nano node_modules/jwt-decode/build/esm/index.d.ts
```
- Adicione o seguinte campo:
```cmd
export interface JwtPayload {
    iss?: string;
    sub?: string;
    aud?: string[] | string;
    exp?: number;
    nbf?: number;
    iat?: number;
    jti?: string;
    role?: any; #adicione o campo role.
}
```
Libere o módulo de reescrita
```
a2enmod rewrite
```

após isso realize o build
```
npm run build
```

- Após isso você deverá configurar o apache retornar a página do frontend

```cmd
sudo mv Projeto-de-Sistemas-Operacionais/frontend/dist /var/www/
sudo nano /etc/apache2/sites-available/000-default.conf
```
- Edite o arquivo para que fique dessa forma:
```cmd
    <VirtualHost *:80>
      ServerAdmin webmaster@localhost
      DocumentRoot /var/www/html/seu-app-react/build

      <Directory /var/www/dist>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted

        # Ativando a reescrita
        RewriteEngine On

        # Se o arquivo solicitado não existir, redireciona para o index.html
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteCond %{REQUEST_FILENAME} !-d
        RewriteRule ^ /index.html [L]
    </Directory>
  </VirtualHost>

```

Logo após isso crie o arquivo .htaccess na pasta /var/www/dist
```cmd
    <IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteBase /

    # Se o arquivo solicitado não existir, redireciona para o index.html
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^ /index.html [L]
</IfModule>
```

### 4.2 Rodando o Backend
- para rodar o backend do projeto, você deve usar os comandos:
```cmd
 git clone https://github.com/MateusdiSousa/Projeto-de-Sistemas-Operacionais.git
```
- crie o arquivo .env no projeto 
```cmd
cd Projeto-de-Sistemas-Operacionais/backend
cat .env-example.txt > .env
```
- depois disso edite o arquivo .env com os seguintes dados:
```cmd
nano .env
```
  
```cmd
DB_PORT=3306
DB_USER=root
DB_PASSWORD=fatec
DATABASE=redes
TOKEN_JWT=fatec
```
- Então na raiz do backend haverá um arquivo chamado *compose.yml*:
```cmd
version: '3.8'

services:
  backend:
    build: .
    ports:
      - "3000:3000"
    networks:
      - my-network
    environment:
      DB_HOST: "mysql"
      DB_PORT: 3306 
      DB_USER: "root"
      DB_PASSWORD: ${DB_PASSWORD}
      DATABASE: ${DATABASE}
      TOKEN_JWT: ${TOKEN_JWT}
    depends_on:
      - mysql
    restart: always

  mysql:
    image: mysql:8
    ports:
      - "${DB_PORT}:3306"
    networks:
      - my-network
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: ${DB_PASSWORD}
      MYSQL_DATABASE: ${DATABASE}
    volumes:
      - db_data:/var/lib/mysql

networks:
  my-network:

volumes:
  db_data:
```
- Execute o seguinte comando para subir os containers do docker. 

```cmd
docker-compose up
```

### 3.4 Load Balancer

- Crie um arquivo de configuração na pasta /etc/nginx/conf.d/loadbalancer.conf com o seguinte comando:
```cmd
sudo nano /etc/nginx/conf.d/loadbalancer.conf
``` 
- E seguinda adicione a seguinte configuração
```cmd
upstream frontend{
	# adicione os endereços públicos da máquinas que estão hospedando o site
	server ip_publico1; 
	server ip_publico2;
	server ip_publico3;
}

server {
	listen 192.168.0.100; # esse será o endereço que o usuário irá acessar o loadbalancer quando estiver conectado à VPN
	
	location / {
		proxy_pass http://frontend;
	        proxy_set_header Host $host;
	        proxy_set_header X-Real-IP $remote_addr;
	        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	        proxy_set_header X-Forwarded-Proto $scheme;
	}

	        location ~* \.(css|js|png|jpg|jpeg|gif|ico)$ {
                proxy_pass http://frontend;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;
            }
}

```
### 3.5 VPN
- Após a configuração do loadbalancer o usuário deveŕa configurar a rede VPN. começando pela configuração do servidor VPN:
- Crie o arquivo server.conf com o seguinte comando:
```cmd
sudu su
cd /etc/openvpn
nano server.conf
```
- e adicione os seguintes dados:
```
dev tun # Dispositivo virtual de túnel (tun) para transporte de pacotes IP
ifconfig 192.168.0.100 192.168.0.200 # O primeiro endereço IP será atribuído ao servidor, e o segundo será atribuído ao cliente
port 1194 # Porta onde o servidor OpenVPN irá escutar conexões
proto tcp-server # Protocolo TCP em modo servidor
verb 4 # Nível de verbosidade dos logs (4 é um nível detalhado, mas sem excesso)
keepalive 10 120 # Mantém a conexão ativa: verifica a cada 10 segundos, e reconecta se não houver resposta em 120 segundos
persist-tun # Mantém a interface TUN ativa mesmo após reiniciar a conexão
persist-key # Mantém a chave privada carregada mesmo após reiniciar a conexão
comp-lzo # Ativa a compressão de dados LZO
secret /etc/openvpn/chave.key # Caminho para o arquivo de chave estática usada para autenticação e criptografia
cipher AES256 # algoritmo de criptografia utilizado
```

- após isso faça um cópia da configuração do servidor para criar a configuração do cliente:
```cmd
cat server.conf > client.conf
```

- depois modifique o arquivo client.conf:
```cmd
nano client.conf
```
```cmd
dev tun
ifconfig 192.168.0.200 192.168.0.100
remote {ENDEREÇO IP DA INSTÂNCIA ONDE ESTÁ A VPN}
port 1194
proto tcp-client
verb 4
keepalive 10 120
persist-tun
persist-key
float
comp-lzo
secret chave.key
cipher AES256
```
- Após isso deverá ser gerada a chave de acesso da vpn com o seguinte comando:
```cmd
openvpn --genkey --secret chave.key
```
- Então você deverá modificar o acesso dos arquivos *chave.key* e *client.conf*
```cmd
chmod 777 chave.key
chmod 777 client.conf
```
- Depois copie os dois arquivos para o computador que será cliente usando o comando scp por exemplo:
```cmd
scp -i /caminho/para/chave_privada usuario@host:/etc/openvpn/client.conf arquivo_destino
scp -i /caminho/para/chave_privada usuario@host:/etc/openvpn/chave.conf arquivo_destino
```
- Após isso deverá iniciar a VPN no lado do servidor com o seguinte comando:
```cmd
openvpn --config /etc/openvpn/server.conf
```
- Em seguida conecte-se a VPN no lado do cliente com o próximo comando:
```cmd
sudo openvpn --config arquivo/destino/client.conf
```

- Após realizar a conexão com a VPN, na instância do servidor inicie o nginx com o seguinte comando:
```cmd
sudo systemctl restart ngnix.service
```

#### Após realizar todos os passos todo o ambiente deverá estar configurado.


