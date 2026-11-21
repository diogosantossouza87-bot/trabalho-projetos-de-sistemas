# 🏖️ Manaus Beach Point — Sistema de Gestão e Reserva de Quadras

![Status](https://shields.io)
![Stack](https://shields.io)
![Cloud](https://shields.io)

Plataforma web responsiva para agendamento online, pagamento integrado (Pix e Cartão de Crédito) e gestão operacional de quadras de *Beach Tennis* em Manaus/AM.

---

## 📐 Resumo da Arquitetura (Etapa 1)

A solução foi projetada seguindo o modelo C4 e decisões arquiteturais documentadas (`ADR-001` a `ADR-003`) para suportar picos de acesso no horário nobre sem conflito de horários (*overbooking*):

* **Frontend:** SPA responsiva em **React + TypeScript**, hospedada em **AWS S3 + CloudFront** (HTTPS/TLS 1.3).
* **Backend:** API RESTful stateless em **Node.js + TypeScript + Express**, executada em instâncias **AWS EC2** atrás de um **Application Load Balancer (ALB)** com *Auto Scaling*.
* **Banco de Dados:** **PostgreSQL 16 (AWS RDS Multi-AZ)** com garantia de integridade contra reserva dupla via restrição nativa `UNIQUE (quadra_id, data_hora_inicio)`.
* **Autenticação e Segurança:** JWT stateless (15 min) + *Refresh Token* rotativo (`ADR-002`), RBAC (`Cliente`, `Atendente`, `Administrador`) e senhas protegidas com bcrypt (`RNF02`).
* **Integrações Externas:** Gateway de Pagamento (Cartão de Crédito e Pix) e Serviço de Notificações (E-mail e SMS).

📄 *Consulte os requisitos completos (`RF01–RF10`, `RNF01–RNF06`, `RN01–RN05`), diagramas C4 e ADRs na pasta `docs/`.*

---

## 📊 Resumo Executivo do Plano de Projeto (Etapa 2)

O plano detalhado de execução, estimativas PERT, cronograma Gantt, orçamento de 3 anos e matriz de riscos está disponível integralmente em **[`PLANO.md`](./PLANO.md)**.

| Indicador de Planejamento | Valor / Meta Definida | Referência |
| :--- | :--- | :--- |
| **Equipe e Capacidade** | 1 integrante multidisciplinar (4h/dia útil — 20h/semana) | `PLANO.md` (Seção 1) |
| **Duração Estimada (PERT)** | **65,6 dias úteis** + **5 dias de buffer** = **71 dias úteis (~14,2 semanas)** | `PLANO.md` (Seção 3) |
| **Orçamento de Implantação (CAPEX)** | **R\$ 22.075,20** *(inclui R\$ 2.207,52 contingência + R\$ 1.471,68 gerencial)* | `PLANO.md` (Seção 4.1) |
| **Operação Projetada (OPEX — 3 Anos)** | **R\$ 74.020,00** *(Ano 1: 1 arena \| Ano 2: 2 arenas \| Ano 3: 3 arenas)* | `PLANO.md` (Seção 4.2) |
| **TCO Total (Implantação + 3 Anos)** | **R\$ 96.095,20** | `PLANO.md` (Seção 4.2) |
| **SLA e Performance Alvo** | Disponibilidade **99,5%** \| Latência **p95 ≤ 500 ms** \| **50 escritas/s** | `RNF01`, `RNF03`, `RNF06` |

---

## 📁 Estrutura do Repositório

```text
manaus-beach-point/
├── README.md                          # Visão geral do projeto, arquitetura e resumo executivo
├── PLANO.md                           # Plano de Projeto completo da Etapa 2 (EAP, PERT, Gantt, TCO e Riscos)
├── docs/                              # Documentação da Etapa 1 (Requisitos, Diagramas C4 e ADRs)
│   └── Trabalho_Pratico.pdf           # Especificação completa de Requisitos e Arquitetura
├── frontend/                          # Cliente SPA (React + TypeScript) [Planejado - EAP 1.4]
│   └── src/
├── backend/                           # Servidor API REST (Node.js + TypeScript + Express) [Planejado - EAP 1.3]
│   └── src/
│       ├── routes/                    # Camada de Apresentação e Middlewares (JWT / RBAC)
│       ├── controllers/               # Regras de Negócio (Auth, Reservas, Pagamentos, Relatórios)
│       └── repositories/              # Camada de Dados (Acesso PostgreSQL via pg)
└── infra/                             # Scripts de provisionamento AWS (VPC, EC2, RDS, S3) e SQL [Planejado - EAP 1.2]
```
