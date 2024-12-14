# Criação de um Phishing com o Kali Linux 

![Screenshot_20241213-202704](https://github.com/user-attachments/assets/29de3f18-6a62-445d-9ba6-dcf5c46b05a9)


# Vamos aos passos para executar o desafio


# Ferramentas Utilizadas

# 1. VirtualBox:

   ![Screenshot_20241213-192022](https://github.com/user-attachments/assets/a83e3495-fc60-4b20-9ed6-d0721e5aefb1)


Ferramenta de virtualização que permite executar sistemas operacionais em uma máquina virtual (VM).

Objetivo no projeto: Instalar e configurar o Kali Linux para criar o ambiente controlado de testes.


# 2. Kali Linux:

   ![Screenshot_20241213-192515](https://github.com/user-attachments/assets/70df1348-5b19-4549-9f06-a34e34ac681a)

Sistema operacional baseado em Debian, projetado para testes de segurança e hacking ético.

Objetivo no projeto: Usar ferramentas de segurança para simular um ataque de phishing.


# 3. SET (Social-Engineer Toolkit) 
![Screenshot_20241213-193058](https://github.com/user-attachments/assets/14ea72bb-c029-4735-8582-b2047c7f1227)


Ferramenta de código aberto incluída no Kali Linux. Focada em engenharia social, permite criar páginas falsas para testes de segurança.

Objetivo no projeto: Clonar páginas e capturar credenciais.


# 4. Git/GitHub

![Screenshot_20241213-193346](https://github.com/user-attachments/assets/b44e2ce7-daf0-498b-ae65-c3f8dd3ec9f4)


Ferramenta para controle de versão e repositório online.

Objetivo no projeto: Versionar o código e compartilhar o projeto.


# Passo a Passo Detalhado

# Etapa 1: Configurar a Máquina Virtual no VirtualBox

1. Baixar o VirtualBox:

Faça o download no site oficial do VirtualBox.

Instale no seu sistema operacional principal.


# 2. Baixar a imagem do Kali Linux:

Baixe a versão ISO do Kali Linux aqui.

Escolha a opção "VMs" para facilitar a integração com o VirtualBox.


# 3. Criar uma máquina virtual no VirtualBox:

Abra o VirtualBox e clique em Novo.

Insira o nome como Kali Linux e configure:

Tipo: Linux.

Versão: Debian 64-bit.


Aloque recursos:

RAM: 2 GB ou mais.

Disco: 20 GB ou mais.


Selecione a imagem ISO do Kali Linux como disco de inicialização.


# 4. Iniciar o Kali Linux:

Inicie a VM e conclua a instalação do Kali Linux. Use as credenciais padrão:

Usuário: kali

Senha: kali 


# Etapa 2: Atualizar o Kali Linux e Instalar Ferramentas

1. Atualizar o sistema:
Abra o terminal no Kali Linux e execute:

sudo apt update && sudo apt upgrade -y


# 2. Verificar o SET:
O SET já está pré-instalado no Kali Linux. Verifique executando:

setoolkit


# 3. Instalar Git (se necessário):

sudo apt install git -y


# Etapa 3: Configurar o Phishing com o SET

1. Iniciar o SET:
No terminal, execute:

sudo setoolkit

# 2. Escolher o tipo de ataque:
No menu interativo, escolha:

1) Social-Engineering Attacks.


# 3. Selecionar o vetor de ataque:
Escolha:

2) Website Attack Vectors.


4. Método de captura de credenciais:
Escolha:

3) Credential Harvester Attack Method.


5. Método específico:
Escolha:

2) Site Cloner.


6. Configurar o IP e a URL:

# Obtenha o endereço IP da máquina virtual:

ifconfig

Use o IP mostrado na interface ativa, como 192.168.x.x.

Insira o IP no SET.

# Insira o endereço da página a ser clonada (exemplo: http://www.facebook.com).


7. O SET criará um servidor local que hospedará a página clonada.


# Etapa 4: Testar o Phishing

# 1. Acessar a página clonada:

No navegador de qualquer máquina na mesma rede, acesse:

http://192.168.x.x

A página clonada será exibida.

# 2. Inserir credenciais:

Quando alguém insere informações na página clonada, os dados são capturados pelo SET.


# 3. Visualizar os resultados no terminal:
O SET exibirá algo assim:

[*] WE GOT A HIT! User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)
[*] POST DATA: username=usuario_exemplo&password=senha_exemplo
[*] CREDENTIALS HARVESTED: username: usuario_exemplo, password: senha_exemplo



# Log: 

![Screenshot_20241213-204010](https://github.com/user-attachments/assets/3b23148e-e961-4536-aeac-77e9a3aad73a)


O log dos resultados gerados pelo SET (Social-Engineer Toolkit) será exibido diretamente no terminal do Kali Linux enquanto o ataque de phishing está ativo.


# 1. Acesso à página clonada:

Quando alguém acessa a página falsa, o SET registra a atividade no terminal:

[*] Credential Harvester is running on http://192.168.1.100
[*] Waiting for data to be submitted...
[*] We got a hit! IP: 192.168.1.101
[*] User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.88 Safari/537.36


# 2. Captura de credenciais:

![Screenshot_20241213-203711](https://github.com/user-attachments/assets/c38141b1-0711-4149-8c12-dd01caf521e3)


Quando um usuário insere dados na página falsa (como nome de usuário e senha), o SET exibe algo assim:

[*] POST request received from: 192.168.1.101
[*] Data captured:
username=usuario_exemplo
password=senha_exemplo


# 3. Logs com vários acessos:

Se houver múltiplos acessos, o SET registrará todas as tentativas:

[*] We got a hit! IP: 192.168.1.102
[*] User-Agent: Mozilla/5.0 (Windows NT 10.0; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Firefox/84.0
[*] POST request received from: 192.168.1.102
[*] Data captured:
username=teste_user
password=senha123

[*] We got a hit! IP: 192.168.1.103
[*] User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Safari/537.36
[*] POST request received from: 192.168.1.103
[*] Data captured:
username=usuario_teste
password=senha_secreta 


# Como Salvar o Log

O log normalmente aparece apenas no terminal. Para salvá-lo em um arquivo, você pode redirecionar a saída do terminal para um arquivo de texto.

# 1. Execute o SET redirecionando a saída:

 sudo setoolkit > logs.txt


# 2. Todos os resultados exibidos no terminal serão salvos no arquivo logs.txt.



# Comandos úteis para gerenciar logs

# Visualizar em tempo real (log ativo):

tail -f /var/log/setoolkit/harvester.log


# Filtrar informações específicas:

grep "Captured" /var/log/setoolkit/harvester.log


# Compactar logs antigos:

tar -czvf logs_antigos.tar.gz /var/log/setoolkit/



# Etapa 5: Publicar no GitHub

# 1. Criar um repositório no GitHub:

Nomeie como DesafioDioPhishing 










