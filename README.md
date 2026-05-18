## Bootcamp Santander Cibersegurança #2 · DIO

![Screenshot_20241213-174339](https://github.com/user-attachments/assets/4b5db1cb-8ee3-467b-84c9-872c37c1c033)

---

> ⚠️ **Aviso Legal:** Este projeto foi desenvolvido exclusivamente em ambiente controlado e isolado, com fins educacionais, como parte de bootcamp certificado de cibersegurança. Simular ataques de phishing sem autorização expressa é crime previsto na Lei nº 12.737/2012 (Lei Carolina Dieckmann) e no Marco Civil da Internet. Nenhum dado real foi capturado, nenhum sistema externo foi acessado, e nenhum usuário real foi envolvido. Todo conteúdo aqui documentado destina-se ao entendimento de como ataques funcionam para que possam ser prevenidos.

---

# Simulação de Phishing com Kali Linux — Engenharia Social na Prática

---

## 1. Problema de Negócio

Phishing é responsável por mais de 80% das violações de segurança reportadas globalmente. Mesmo com investimentos crescentes em firewalls, EDRs e SOCs, o vetor humano permanece o mais explorado — e o menos treinado.

O problema central para equipes de segurança não é apenas bloquear ataques. É entender como eles são construídos antes que atacantes reais os usem. Profissionais que nunca simularam um ataque de engenharia social têm um ponto cego operacional crítico: não sabem exatamente o que estão defendendo.

O desafio técnico deste projeto é executar uma simulação completa de phishing — da configuração do ambiente isolado à clonagem de página e captura de credenciais — documentando cada etapa do pipeline de ataque com o rigor de um relatório de pentest real. O entregável não é o ataque: é o conhecimento operacional que habilita a construção de defesas calibradas para vetores reais.

---

## 2. Contexto

O projeto foi desenvolvido como desafio prático do **Bootcamp Santander Cibersegurança #2** na DIO. O objetivo era simular um ataque de phishing do início ao fim: configurar o ambiente de testes em máquina virtual isolada, usar o SET (Social-Engineer Toolkit) para clonar uma página de login, capturar credenciais submetidas e preservar os logs da operação no formato de evidência auditável.

A escolha de VirtualBox como ambiente de isolamento não foi acidental — é um requisito de segurança operacional. Qualquer simulação de ataque deve ocorrer em rede isolada, sem exposição à internet pública, para garantir que o servidor de phishing e as credenciais capturadas não saiam do perímetro controlado. Essa disciplina é o que diferencia um teste de penetração ético de uma atividade ilícita — e é exatamente o que qualquer engajamento profissional de red team exige como primeiro controle.

![Screenshot_20241213-174549](https://github.com/user-attachments/assets/1477545d-011a-440f-97c5-fd16fe2912b4)

---

## 3. Premissas

- Todo o experimento foi executado em **rede local isolada** (LAN interna da VM, modo Host-Only), sem acesso à internet pública e sem envolvimento de usuários reais;
- As credenciais capturadas nos logs (`usuario_exemplo`, `senha_exemplo`) são **dados fictícios** inseridos pelo próprio executante para validar o funcionamento do pipeline — não há dados reais em nenhum arquivo deste repositório;
- O SET foi utilizado na modalidade **Credential Harvester + Site Cloner**, que hospeda o servidor de phishing localmente na porta 80 — nenhum dado trafegou fora da máquina virtual;
- O ambiente foi destruído ao final do exercício, conforme boas práticas de segurança operacional em laboratórios de pentesting;
- A documentação dos logs serve como evidência de execução para fins de aprendizado — o mesmo formato de relatório produzido em testes de penetração profissionais autorizados.

---

## 4. Estratégia da Solução

A simulação seguiu o pipeline completo de um ataque de phishing por engenharia social, organizado em cinco etapas:

**Etapa 1 — Configuração do ambiente isolado (VirtualBox + Kali Linux)**
Criação de VM com Kali Linux em modo Host-Only, garantindo isolamento total de rede. Recursos mínimos alocados: 2 GB RAM, 20 GB disco. O modo Host-Only foi escolhido em vez de NAT para garantir que nenhum tráfego do servidor SET pudesse vazar para a rede do host.

**Etapa 2 — Atualização e verificação do SET**
O Social-Engineer Toolkit vem pré-instalado no Kali Linux. Após `sudo apt update && sudo apt upgrade -y`, o SET foi iniciado via `sudo setoolkit` para confirmação de funcionamento e versão.

**Etapa 3 — Configuração do ataque via SET**
Navegação pelo menu interativo em quatro níveis: `Social-Engineering Attacks` → `Website Attack Vectors` → `Credential Harvester Attack Method` → `Site Cloner`. O IP local da VM foi obtido via `ifconfig` e inserido como endereço do servidor de captura.

**Etapa 4 — Execução e captura de credenciais**
O SET iniciou um servidor HTTP local na porta 80. Ao acessar `http://192.168.x.x` em outro dispositivo da mesma rede isolada, a página clonada foi exibida. Credenciais inseridas no formulário foram capturadas e exibidas no terminal do SET em tempo real, no formato `POST DATA: username=...&password=...`.

**Etapa 5 — Preservação e análise de logs**
Os logs do SET foram redirecionados para arquivo e analisados com ferramentas de linha de comando. Essa etapa replica a fase de evidências de um pentest real — capturar e preservar logs auditáveis é parte obrigatória de qualquer engajamento de segurança ofensiva profissional.

```bash
# Redirecionar saída para arquivo de log
sudo setoolkit > logs.txt

# Monitorar capturas em tempo real
tail -f /var/log/setoolkit/harvester.log

# Filtrar credenciais capturadas
grep "Captured" /var/log/setoolkit/harvester.log

# Compactar logs para arquivamento
tar -czvf logs_auditoria.tar.gz /var/log/setoolkit/
```

---

## 5. Decisões Técnicas e Trade-offs

**Por que Host-Only em vez de NAT no VirtualBox?**
O modo NAT permite acesso à internet a partir da VM, mas também significa que o servidor HTTP do SET poderia, teoricamente, responder a requisições externas dependendo da configuração do host. O modo Host-Only cria uma rede completamente privada entre VM e host, eliminando qualquer possibilidade de vazamento do servidor de phishing para fora do perímetro de laboratório. É a diferença entre "provavelmente isolado" e "garantidamente isolado" — e em segurança, a segunda categoria é o único padrão aceitável.

**Por que documentar os logs no formato de evidência auditável?**
A maioria dos tutoriais de phishing mostra o ataque e para por aí. A decisão de preservar logs com `grep`, `tail` e `tar` e documentá-los no repositório replica o entregável real de um pentest: a evidência de execução. Em engajamentos profissionais, o relatório com logs auditáveis é tão importante quanto o ataque em si — sem ele, o cliente não tem como validar o que foi testado. Praticar essa disciplina desde o laboratório é o que forma a diferença entre um estudante de segurança e um profissional de pentest.

**Por que SET (Social-Engineer Toolkit) e não uma ferramenta customizada?**
O SET é o padrão de mercado para simulações de engenharia social em ambientes de laboratório porque é auditável, documentado e amplamente conhecido pelas equipes de defesa. Uma ferramenta customizada poderia ser mais sofisticada, mas comprometeria a reproducibilidade do laboratório e a clareza do aprendizado. O objetivo não era evadir detecção — era entender o mecanismo do ataque na sua forma mais legível.

**Por que o pipeline começa pela configuração do isolamento, e não pelo ataque?**
Porque a fronteira ética e legal de qualquer teste de penetração é definida antes da primeira linha de comando. Documentar a configuração do isolamento como Etapa 1 — não como pré-requisito implícito — reflete a realidade de engajamentos profissionais: o scope e o ambiente controlado são definidos contratualmente e tecnicamente antes de qualquer ação ofensiva. Essa ordem não é apenas didática; é operacional.

---

## 6. Resultados

O laboratório entregou:

- **Pipeline completo executado com sucesso** em ambiente isolado: VM Host-Only → clonagem de página → servidor HTTP local → captura de credenciais via POST → análise de logs
- **Registro auditável do comportamento do SET** durante captura, incluindo User-Agent, IP de origem e dados POST submetidos — no mesmo formato de evidência utilizado em relatórios de pentest profissional

```
[*] Credential Harvester is running on http://192.168.1.100
[*] We got a hit! IP: 192.168.1.101
[*] User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) ...
[*] Data captured:
    username=usuario_exemplo
    password=senha_exemplo
```

- **Compreensão operacional da anatomia do ataque** — da isca à extração de dados — aplicável diretamente em treinamentos de Security Awareness e na definição de controles técnicos (MFA obrigatório, inspeção de tráfego HTTP interno, simulações periódicas de phishing)
- **Base prática para argumentar controles de segurança**: tendo executado o ataque, é possível dimensionar com precisão o esforço do atacante e o valor de cada camada defensiva — WAF, proxy reverso com inspeção de conteúdo, monitoramento de DNS interno

---

## 7. Próximos Passos

- **Fechar o ciclo ataque → defesa:** configurar um proxy reverso com inspeção de conteúdo para detectar e bloquear servidores de phishing internos — transformar o laboratório ofensivo em um laboratório de hardening;
- **Evoluir para Spear Phishing simulado:** ataques direcionados com personalização de mensagem por perfil, avaliando como a taxa de engajamento muda com o nível de personalização — dado relevante para calibrar treinamentos de conscientização;
- **Integrar com SIEM local:** enviar os logs do SET para uma instância local de Elastic Stack e construir alertas automáticos para submissão de credenciais em páginas suspeitas, simulando a detecção em um SOC real;
- **Estruturar relatório de pentest formal:** organizar os resultados no formato profissional — Executive Summary, Findings, Risk Rating (CVSS), Remediation Recommendations — que é o entregável esperado em engajamentos comerciais de segurança ofensiva.

---

## Ferramentas Utilizadas

| Ferramenta | Papel no Projeto |
|---|---|
| **VirtualBox (Host-Only)** | Fronteira de isolamento — garantia de que o servidor de phishing não sai do perímetro controlado |
| **Kali Linux** | Sistema operacional de pentesting com SET pré-instalado e ferramentas de análise de logs |
| **SET — Social-Engineer Toolkit** | Clonagem de página, servidor HTTP de captura de credenciais, geração de logs auditáveis |
| **ifconfig / grep / tail / tar** | Coleta, filtragem e preservação de evidências operacionais |
| **Git / GitHub** | Controle de versão e documentação do projeto para portfólio |

---

### Kali Linux
![Screenshot_20241213-173508](https://github.com/user-attachments/assets/99247379-2986-4ec8-953b-7c956cca5c0e)

### SET (Social-Engineer Toolkit)
![Screenshot_20241213-173823](https://github.com/user-attachments/assets/21f5d895-be66-4138-bf13-0459839d5306)

### Log de Captura
![Screenshot_20241213-204010](https://github.com/user-attachments/assets/3b23148e-e961-4536-aeac-77e9a3aad73a)

---

> *"O mercado não contrata ferramenta. Contrata quem resolve problemas."*
> — Meigarom Lopes

---

[![Portfólio Sérgio Santos](https://img.shields.io/badge/Portfólio-Sérgio_Santos-111827?style=for-the-badge&logo=githubpages&logoColor=00eaff)](https://portfoliosantossergio.vercel.app)
[![LinkedIn Sérgio Santos](https://img.shields.io/badge/LinkedIn-Sérgio_Santos-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/santossergioluiz)
