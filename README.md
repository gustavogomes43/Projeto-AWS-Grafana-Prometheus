# 📊 AWS Observability Stack: Prometheus & Grafana com Docker

![Status](https://img.shields.io/badge/Status-Concluído-success)
![AWS](https://img.shields.io/badge/AWS-EC2-orange)
![Docker](https://img.shields.io/badge/Docker-Containers-blue)
![Grafana](https://img.shields.io/badge/Grafana-Monitoring-orange)

## 🎯 Por que este projeto?
Em ambientes de missão crítica, "quem não mede, não gerencia". Este projeto nasceu da necessidade de transformar um servidor "caixa-preta" em um ambiente totalmente transparente. O objetivo foi implementar uma camada de **Observabilidade Profissional** para monitorar a saúde de instâncias EC2 na AWS, permitindo antecipar falhas antes que elas afetem o usuário final.

## 🏗️ Arquitetura do Projeto
A arquitetura foi desenhada para ser leve, escalável e de rápida implementação via containers:

```text
[ Usuário / Admin ] 
      │
      ▼
[ AWS Cloud ]
      │
  ┌───┴────────────────────────────────────────┐
  │  EC2 Instance (Amazon Linux)               │
  │                                            │
  │   ┌──────────────┐      ┌──────────────┐   │
  │   │   Grafana    │◄─────┤  Prometheus  │   │
  │   │ (Dashboard)  │      │  (Database)  │   │
  │   └──────┬───────┘      └──────▲───────┘   │
  │          │                     │           │
  │          │              ┌──────┴───────┐   │
  │          └─────────────►│ Node Exporter│   │
  │                         │ (Metrics)    │   │
  │                         └──────────────┘   │
  └────────────────────────────────────────────┘
```

## 🛠️ Ferramentas Utilizadas
* **AWS EC2:** Escolhida pela flexibilidade e por ser o padrão de mercado para computação em nuvem.
* **Docker & Docker Compose:** Utilizados para garantir que o ambiente de monitoramento seja idêntico em qualquer máquina, facilitando o deploy e a portabilidade.
* **Prometheus:** A melhor opção para métricas de séries temporais devido ao seu modelo de *pull*, que é altamente eficiente.
* **Grafana:** Líder de mercado em visualização de dados, permitindo criar dashboards que facilitam a tomada de decisão executiva.
* **Node Exporter:** Um agente leve que expõe métricas de hardware e do kernel Linux diretamente para o coletor.

## 🚀 Como foi criado (Passo a Passo)
1. **Provisionamento Automatizado:** Utilizei scripts de **User Data** na AWS para que a instância já nascesse com Docker e Docker Compose instalados.
2. **Infraestrutura como Código (Mentalidade):** Desenvolvi um arquivo `docker-compose.yml` que orquestra os três containers (Prometheus, Grafana e Node Exporter) em uma rede isolada.
3. **Configuração de Redes (Security Groups):** Configurei regras de entrada específicas para as portas `3000` (Grafana) e `9090` (Prometheus), mantendo o princípio do privilégio mínimo.
4. **Data Source Integration:** Conectei o Prometheus ao Grafana via rede interna do Docker, otimizando o tráfego.

## 🧠 Desafios e Soluções
Durante o projeto, surgiram desafios reais que exercitaram meu raciocínio lógico:

* **Desafio 1: Comunicação entre Containers.**
    * *Problema:* O Grafana não conseguia encontrar o Prometheus usando `localhost`.
    * *Solução:* Entendi o funcionamento das **Docker Networks**. Configurei o acesso via nome do serviço definido no Compose (`http://prometheus:9090`), permitindo que o DNS interno do Docker resolvesse o endereço.
* **Desafio 2: Erros de cache e extensões no navegador.**
    * *Problema:* A interface do Grafana apresentava pop-ups de erro inesperados.
    * *Solução:* Realizei o isolamento do problema testando em modo incógnito e diagnosticando que extensões de terceiros estavam conflitando com o JavaScript da ferramenta.

## 💰 Benefícios e Redução de Custos
A implementação desta solução em uma empresa gera impactos diretos no faturamento:
* **Redução de MTTR (Mean Time To Repair):** Com alertas e gráficos, a equipe identifica a causa raiz de um travamento em minutos, não horas.
* **Right-sizing de Instâncias:** Ao observar que a CPU raramente passa de 10%, a empresa pode fazer o *downgrade* da instância, reduzindo custos de infraestrutura em até **60%**.
* **Prevenção de Downtime:** Monitorar o uso de disco evita que o servidor pare por falta de espaço, o que evita prejuízos financeiros.

## 📈 Resultados e Testes de Stress

### Validação em Tempo Real
Para validar a precisão da coleta, realizei um teste de stress na CPU através do terminal.

![Comando de Stress](screenshots/Stress.png)
> **Legenda:** Execução do comando de stress via SSH na EC2.

![Pico no Grafana](screenshots/DashI.png)
> **Legenda:** Resposta imediata no Grafana mostrando o pico de processamento.

---
**Desenvolvido por Gustavo - Foco em DevOps e Cloud Computing.**
