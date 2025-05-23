# DevOps - Sistema de Gerenciamento de Estudantes

## Aplicação escolhida:
A aplicação foi originalmente demonstrada em aula, contendo apenas uma versão em terminal(Python) para gerenciamento de estudantes. Para criação de mais containers, desenvolvi uma versão web(Java/Tomcat), que oferece as mesmas funcionalidades, porém com uma interface gráfica
Ambas as versões compartilham o mesmo banco de dados MongoDB, permitindo que as alterações feitas em uma interface sejam refletidas imediatamente na outra. Isso garante consistência nos dados e flexibilidade no uso.

## Containers e tecnologias usadas:
### MongoDB - Banco de dados:
- **Função**: Armazenamento dos registros dos estudantes
- **Importância**: Serviço central que provê a persistência dos dados para toda a aplicação
- **Configurações**:
  - Usada a imagem oficial do MongoDB
  - Utiliza autenticação com usuário e senha root
  - Volume dedicado, para que os dados não sejam perdidos quando reiniciar o container
  - Expoẽ a porta padrão apenas para outros containers
- **Relações**:
  - É acessado pelos containers tomcat e terminal
  - Não depende de outros serviços

### Tomcat - versão web
- **Funcão**: Prover a interface gráfica web para gerenciamento de estudantes
- **Importância**: Permite acesso via navegador, essencial para usuários finais em produção
- **Configurações**:
  - Dockerfile:
    - Primeiro o Maven compila o código Java e gera o arquivo WAR, depois o Tomcat recebe só o arquivo pronto
    - O arquivo WAR é colocado na pasra webapps do Tomcat com o nome ROOT.war, ele é automaticamente implantado como aplicação principal e acessível pela URL base
  - A porta 8080 é exposta para acesso externo à aplicação web 
- **Relações**:
  - Conecta-se ao mongodb usando o nome do serviço
  - Não possui dependências diretas além do banco de dados  

### Terminal - versão em python
- **Função**: Prover uma interface via linha de comando
- **Importância**: Útil para ambientes sem interface gráfica
- **Configurações**:
  - Dockerfile:
    - Baseado em imagem Python
    - Utiliza PyMongo para conexão com o banco
    - Executa o script Python
- **Relações**:
  - Conecta-se ao mongodb usando o nome do serviço
  - Opera de modo independênte da aplicação web 

## Comunicação entre containers
- Containers estão conectados a rede `app_network`, permitindo que os containers se comuniquem pelos nomes, sem expor portas desnecessárias ou usar localhost

## Manual de instalação
- Necessário ter Docker e Docker Compose instalados
  
### Passos:
- Para iniciar todos os containers, executar na pasta Estudantes
  - `docker-compose build --no-cache && docker-compose up -d`
- Acessar as interfaces:
  - Na web: http://localhost:8080/students
  - No terminal: `docker exec -it terminal python ./main.py`


![image](https://github.com/user-attachments/assets/628d2282-9cd0-4d49-b8e1-d132eecf67f4)


