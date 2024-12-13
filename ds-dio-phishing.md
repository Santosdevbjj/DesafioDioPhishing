
Vamos refazer o desafio detalhadamente, integrando o uso do VirtualBox para configurar o ambiente e executar o projeto. Aqui está um passo a passo detalhado do início ao fim, incluindo uma explicação sobre as ferramentas utilizadas.



Ferramentas Utilizadas

1. VirtualBox

Ferramenta de virtualização que permite executar sistemas operacionais em uma máquina virtual (VM).

Objetivo no projeto: Instalar e configurar o Kali Linux para criar o ambiente controlado de testes.


2. Kali Linux

Sistema operacional baseado em Debian, projetado para testes de segurança e hacking ético.

Objetivo no projeto: Usar ferramentas de segurança para simular um ataque de phishing.



3. SET (Social-Engineer Toolkit)

Ferramenta de código aberto incluída no Kali Linux. Focada em engenharia social, permite criar páginas falsas para testes de segurança.

Objetivo no projeto: Clonar páginas e capturar credenciais.



4. Git/GitHub

Ferramenta para controle de versão e repositório online.

Objetivo no projeto: Versionar o código e compartilhar o projeto.




Passo a Passo Detalhado

Etapa 1: Configurar a Máquina Virtual no VirtualBox

1. Baixar o VirtualBox:

Faça o download no site oficial do VirtualBox.

Instale no seu sistema operacional principal.


2. Baixar a imagem do Kali Linux:

Baixe a versão ISO do Kali Linux aqui.

Escolha a opção "VMs" para facilitar a integração com o VirtualBox.


3. Criar uma máquina virtual no VirtualBox:

Abra o VirtualBox e clique em Novo.

Insira o nome como Kali Linux e configure:

Tipo: Linux.

Versão: Debian 64-bit.


Aloque recursos:

RAM: 2 GB ou mais.

Disco: 20 GB ou mais.


Selecione a imagem ISO do Kali Linux como disco de inicialização.


4. Iniciar o Kali Linux:

Inicie a VM e conclua a instalação do Kali Linux. Use as credenciais padrão:

Usuário: kali

Senha: kali




Etapa 2: Atualizar o Kali Linux e Instalar Ferramentas

1. Atualizar o sistema:
Abra o terminal no Kali Linux e execute:

sudo apt update && sudo apt upgrade -y


2. Verificar o SET:
O SET já está pré-instalado no Kali Linux. Verifique executando:

setoolkit


3. Instalar Git (se necessário):

sudo apt install git -y




Etapa 3: Configurar o Phishing com o SET

1. Iniciar o SET:
No terminal, execute:

sudo setoolkit


2. Escolher o tipo de ataque:
No menu interativo, escolha:

1) Social-Engineering Attacks.



3. Selecionar o vetor de ataque:
Escolha:

2) Website Attack Vectors.



4. Método de captura de credenciais:
Escolha:

3) Credential Harvester Attack Method.



5. Método específico:
Escolha:

2) Site Cloner.



6. Configurar o IP e a URL:

Obtenha o endereço IP da máquina virtual:

ifconfig

Use o IP mostrado na interface ativa, como 192.168.x.x.

Insira o IP no SET.

Insira o endereço da página a ser clonada (exemplo: http://www.facebook.com).



7. O SET criará um servidor local que hospedará a página clonada.




Etapa 4: Testar o Phishing

1. Acessar a página clonada:

No navegador de qualquer máquina na mesma rede, acesse:

http://192.168.x.x

A página clonada será exibida.


2. Inserir credenciais:

Quando alguém insere informações na página clonada, os dados são capturados pelo SET.



3. Visualizar os resultados no terminal:
O SET exibirá algo assim:

[*] WE GOT A HIT! User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)
[*] POST DATA: username=usuario_exemplo&password=senha_exemplo
[*] CREDENTIALS HARVESTED: username: usuario_exemplo, password: senha_exemplo




Etapa 5: Publicar no GitHub

1. Criar um repositório no GitHub:

Nomeie como desafio-ciberseguranca.

Adicione uma descrição explicando que o projeto é para fins educacionais.


2. Clonar o repositório no Kali Linux:

git clone https://github.com/seu-usuario/desafio-ciberseguranca.git


3. Adicionar arquivos ao repositório:

Inclua os seguintes itens:

Capturas de tela do SET em funcionamento.

Logs de resultados (exemplo do terminal).

Um arquivo README.md explicando o projeto.


4. Versionar e enviar ao GitHub:

git add .
git commit -m "Adicionando desafio de Phishing"
git push origin main



Estrutura do Repositório no GitHub

Seu repositório deve ter a seguinte estrutura:

desafio-ciberseguranca/
│
├── screenshots/
│   └── captura1.png
│   └── captura2.png
│
├── logs/
│   └── resultados.txt
│
└── README.md

No README.md, inclua:

Uma introdução ao projeto.

Passos para reproduzir o ambiente.

Explicação ética sobre a simulação.



