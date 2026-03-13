## Bootcamp Santander Cibersegurança #2 · DIO

![Screenshot_20241213-174339](https://github.com/user-attachments/assets/4b5db1cb-8ee3-467b-84c9-872c37c1c033)

---

> ⚠️ **Aviso Legal:** Este projeto foi desenvolvido exclusivamente em ambiente controlado e isolado, com fins educacionais, como parte de bootcamp certificado de cibersegurança. Simular ataques de phishing sem autorização expressa é crime previsto na Lei nº 12.737/2012 (Lei Carolina Dieckmann) e no Marco Civil da Internet. Todo o conteúdo aqui documentado destina-se ao entendimento de como ataques funcionam para que possam ser prevenidos.

---

# 🔐 Simulação de Phishing com Kali Linux — Engenharia Social na Prática

## 1. Problema de Negócio

Phishing é responsável por mais de 80% das violações de segurança reportadas globalmente. O problema central para equipes de segurança não é apenas bloquear ataques — é **entender como eles são construídos** para identificar vetores de vulnerabilidade antes que atacantes reais os explorem.

Profissionais de cibersegurança que nunca simularam um ataque de engenharia social têm um ponto cego crítico: não sabem o que estão defendendo. O desafio técnico é executar uma simulação completa de phishing — da clonagem da página à captura de credenciais — em ambiente isolado, documentando cada etapa do pipeline de ataque para uso em treinamentos de conscientização e testes de penetração autorizados.

---

## 2. Contexto

O projeto foi desenvolvido como desafio prático do **Bootcamp Santander Cibersegurança #2** na DIO. O objetivo era simular um ataque de phishing real do início ao fim: configurar o ambiente de testes em máquina virtual, usar o SET (Social-Engineer Toolkit) para clonar uma página de login, capturar credenciais submetidas e registrar os logs da operação.

A escolha de VirtualBox como ambiente de isolamento não foi acidental — é um requisito de segurança operacional. Qualquer simulação de ataque deve ocorrer em rede isolada, sem exposição à internet pública, para garantir que credenciais capturadas e o servidor de phishing não saiam do perímetro controlado. Essa disciplina é o que diferencia um teste de penetração ético de uma atividade ilícita.

![Screenshot_20241213-174549](https://github.com/user-attachments/assets/1477545d-011a-440f-97c5-fd16fe2912b4)

---

## 3. Premissas

- Todo o experimento foi executado em **rede local isolada** (LAN interna da VM), sem acesso à internet pública e sem envolvimento de usuários reais.
- As credenciais capturadas nos logs (`usuario_exemplo`, `senha_exemplo`) são **dados fictícios** inseridos pelo próprio executante do teste para validar o funcionamento do pipeline.
- O SET foi utilizado na modalidade **Credential Harvester + Site Cloner**, que hospeda o servidor de phishing localmente — nenhum dado trafegou fora da máquina virtual.
- O ambiente foi destruído ao final do exercício, conforme boas práticas de segurança operacional em laboratórios de pentesting.
- A documentação dos logs serve como evidência de execução para fins de aprendizado — o mesmo tipo de relatório produzido em testes de penetração reais.

---

## 4. Estratégia da Solução

A simulação seguiu o pipeline completo de um ataque de phishing por engenharia social, dividido em cinco etapas:

**Etapa 1 — Configuração do ambiente isolado (VirtualBox + Kali Linux):** criação de máquina virtual com Kali Linux em rede NAT ou Host-Only, garantindo isolamento total. Recursos mínimos alocados: 2 GB RAM, 20 GB disco. Credenciais padrão (`kali/kali`) utilizadas exclusivamente para o laboratório.

**Etapa 2 — Atualização e verificação do SET:** o Social-Engineer Toolkit já vem pré-instalado no Kali Linux. Após `sudo apt update && sudo apt upgrade -y`, o SET foi iniciado via `sudo setoolkit` para confirmação de funcionamento.

**Etapa 3 — Configuração do ataque via SET:** navegação pelo menu interativo em quatro níveis — `Social-Engineering Attacks` → `Website Attack Vectors` → `Credential Harvester Attack Method` → `Site Cloner`. O IP local da VM foi obtido via `ifconfig` e inserido como endereço do servidor. A URL-alvo para clonagem foi configurada na rede interna.

**Etapa 4 — Execução e captura de credenciais:** o SET iniciou um servidor HTTP local na porta 80. Ao acessar `http://192.168.x.x` em outro dispositivo da mesma rede isolada, a página clonada foi exibida. Credenciais inseridas no formulário foram capturadas e exibidas no terminal do SET em tempo real, no formato `POST DATA: username=...&password=...`.

**Etapa 5 — Registro e análise de logs:** os logs do SET foram redirecionados para arquivo via `sudo setoolkit > logs.txt` e analisados com `grep`, `tail -f` e compactação via `tar`. Essa etapa simula a fase de relatório em um pentest real — capturar evidências é tão importante quanto executar o ataque.

---

## 5. Insights Técnicos

A execução do laboratório revelou aspectos da engenharia social que não são visíveis apenas na teoria:

- **A eficácia do phishing não depende de sofisticação técnica:** o SET clona páginas em segundos, sem nenhuma habilidade de desenvolvimento web. O vetor de ataque mais poderoso não é o código — é a credibilidade visual da página e o senso de urgência na mensagem. Isso explica por que campanhas de phishing continuam funcionando mesmo com usuários tecnicamente experientes.

- **O isolamento de rede é a fronteira entre laboratório e crime:** a diferença técnica entre este experimento e um ataque real é o scope — a rede isolada da VM versus a internet pública. Essa distinção, invisível no código, é o que define legalidade e ética em qualquer teste de penetração.

- **Logs estruturados são evidência auditável:** o formato de saída do SET (`[*] We got a hit! IP: x.x.x.x`, `[*] Data captured: username=...`) segue o mesmo padrão de relatórios de pentest profissional. Saber filtrar e preservar esses logs com `grep`, `tail` e `tar` é uma competência operacional real — não apenas uma etapa do tutorial.

- **O `Credential Harvester` documenta a anatomia do ataque:** ver o POST data chegando no terminal em tempo real torna concreto o mecanismo que usuários finais nunca enxergam. Esse entendimento é o que habilita a construção de defesas efetivas — WAFs, MFA obrigatório, treinamentos de conscientização calibrados para os vetores reais de engajamento.

---

## 6. Resultados

O laboratório entregou:

- Pipeline completo de phishing executado com sucesso em ambiente isolado: configuração de VM → clonagem de página → captura de credenciais → análise de logs
- Registro documentado do comportamento do SET durante captura, incluindo User-Agent, IP de origem e dados POST submetidos
- Compreensão operacional da anatomia de um ataque de engenharia social — da isca à extração de dados — aplicável diretamente em treinamentos de conscientização e definição de controles de segurança
- Base prática para argumentar a necessidade de controles como MFA, monitoramento de tráfego HTTP interno e simulações periódicas de phishing como parte de programas de Security Awareness

---

## 7. Próximos Passos

- **Implementar defesas contra o ataque simulado:** configurar um proxy reverso com inspeção de conteúdo para detectar e bloquear servidores de phishing internos — fechar o ciclo ataque → defesa.
- **Evoluir para Spear Phishing:** simular ataques direcionados com personalização de mensagem por perfil de vítima, avaliando como a taxa de engajamento muda com o nível de personalização.
- **Integrar com SIEM:** enviar os logs do SET para uma instância local de Elastic Stack ou Splunk e construir alertas automáticos para submissão de credenciais em páginas suspeitas.
- **Documentar relatório de pentest formal:** estruturar os resultados no formato padrão de relatório de teste de penetração (Executive Summary, Findings, Risk Rating, Remediation), que é o entregável esperado em engajamentos profissionais de segurança.

---

## 🛠️ Ferramentas Utilizadas

| Ferramenta | Papel no Projeto |
|------------|-----------------|
| **VirtualBox** | Ambiente isolado de virtualização — fronteira de segurança do laboratório |
| **Kali Linux** | Sistema operacional de pentesting com SET pré-instalado |
| **SET (Social-Engineer Toolkit)** | Clonagem de página, servidor de phishing local e captura de credenciais |
| **ifconfig / grep / tail / tar** | Coleta, filtragem e preservação de logs operacionais |
| **Git / GitHub** | Controle de versão e documentação do projeto |

---

## ▶️ Como Reproduzir (Somente em Ambiente Controlado)

```bash
# 1. Instalar VirtualBox e criar VM com Kali Linux (rede Host-Only ou NAT)
# 2. Iniciar o Kali e atualizar o sistema
sudo apt update && sudo apt upgrade -y

# 3. Iniciar o SET
sudo setoolkit
# Menu: 1) Social-Engineering Attacks
#       2) Website Attack Vectors
#       3) Credential Harvester Attack Method
#       2) Site Cloner

# 4. Obter o IP da VM para configurar o servidor
ifconfig

# 5. Executar com log em arquivo
sudo setoolkit > logs.txt

# 6. Monitorar capturas em tempo real
tail -f /var/log/setoolkit/harvester.log

# 7. Filtrar credenciais capturadas
grep "Captured" /var/log/setoolkit/harvester.log
```

> ⚠️ Execute apenas em rede interna isolada. Nunca exponha o servidor SET à internet pública.

---

### Kali Linux
![Screenshot_20241213-173508](https://github.com/user-attachments/assets/99247379-2986-4ec8-953b-7c956cca5c0e)

### SET (Social-Engineer Toolkit)
![Screenshot_20241213-173823](https://github.com/user-attachments/assets/21f5d895-be66-4138-bf13-0459839d5306)

### Log de Captura
![Screenshot_20241213-204010](https://github.com/user-attachments/assets/3b23148e-e961-4536-aeac-77e9a3aad73a)

---

**Contato:**

[![Portfólio Sérgio Santos](https://img.shields.io/badge/Portfólio-Sérgio_Santos-111827?style=for-the-badge&logo=githubpages&logoColor=00eaff)](https://portfoliosantossergio.vercel.app)

[![LinkedIn Sérgio Santos](https://img.shields.io/badge/LinkedIn-Sérgio_Santos-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/santossergioluiz)


---
