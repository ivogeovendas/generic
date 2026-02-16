# Plataforma recomendada para migrar Vaadin 6 para React + Java

## Objetivo
Criar uma plataforma de modernização gradual (sem “big bang”) para substituir telas Vaadin 6 por React, mantendo backend Java e reduzindo risco operacional.

## Stack alvo (recomendada)
- **Frontend**: React + TypeScript + Vite + Material UI (ou Ant Design).
- **Backend**: Java 17+ com Spring Boot 3 (REST APIs).
- **Segurança**: Spring Security + OAuth2/OIDC (Keycloak, Auth0 ou Azure AD).
- **Banco**: manter o banco atual inicialmente; evoluir schema com Flyway/Liquibase.
- **Integração/API**: OpenAPI (Swagger) para contrato entre frontend e backend.
- **Observabilidade**: Micrometer + Prometheus + Grafana + logs centralizados.
- **Entrega**: Docker + CI/CD (GitHub Actions/GitLab CI/Jenkins) + Kubernetes (opcional).

## Estratégia de migração (prática)
### 1) Descoberta e inventário (2–4 semanas)
- Mapear módulos Vaadin 6, fluxos críticos, regras de negócio e dependências.
- Classificar telas em: simples, média complexidade e crítica.
- Definir baseline de performance e indicadores (erro, SLA, lead time).

### 2) Arquitetura-alvo e fundação (2–3 semanas)
- Criar monorepo ou multi-repo com:
  - `frontend-react`
  - `backend-api`
  - `shared-contracts` (OpenAPI / DTOs)
- Padronizar autenticação, autorização e tratamento global de erros.
- Criar design system mínimo para acelerar migração de telas.

### 3) Migração incremental (Strangler Pattern)
- Publicar um **gateway** (Nginx/Spring Cloud Gateway) para rotear:
  - rotas antigas → Vaadin 6
  - rotas novas → React
- Migrar primeiro jornadas de menor risco para validar arquitetura.
- Extrair regras do Vaadin para serviços Java reutilizáveis (evitar lógica em UI).

### 4) Paralelismo controlado e corte final
- Rodar Vaadin 6 e React em paralelo com feature flags.
- Medir uso e erros por tela migrada.
- Desativar gradualmente módulos Vaadin quando equivalentes estiverem estáveis.

## Plataforma técnica de suporte à migração
- **API-first**: OpenAPI + geração de client TypeScript.
- **Testes**:
  - Frontend: Vitest + React Testing Library + Playwright.
  - Backend: JUnit + Testcontainers.
  - Contrato: testes de contrato (consumer-driven, se necessário).
- **Qualidade**: SonarQube + SAST + dependency scanning.
- **Automação de refatoração Java**: OpenRewrite (opcional, útil para modernizar código legado).

## Roadmap sugerido (90 dias)
- **Dias 1–30**: inventário, arquitetura alvo, pipeline CI/CD, autenticação e primeira API.
- **Dias 31–60**: migração de 20–30% das telas (baixa/média complexidade), operação em paralelo.
- **Dias 61–90**: migração de fluxos críticos, hardening, plano de desligamento do Vaadin 6.

## Equipe mínima
- 1 Arquiteto(a) / Tech Lead
- 2 Devs Backend Java
- 2 Devs Frontend React
- 1 QA automação
- 1 DevOps parcial

## Riscos comuns e mitigação
- **Lógica presa na UI Vaadin** → extrair para serviços backend antes da migração da tela.
- **Escopo grande demais** → fatiar por domínio/jornada, não por tecnologia.
- **Quebra de contrato API** → versionamento e testes de contrato.
- **Resistência operacional** → feature flags, deploy canário e métricas de adoção.

## Recomendação objetiva
Se você quer reduzir risco e acelerar entrega, a melhor “plataforma” é:
1. **Spring Boot 3 (backend)** + **React/TypeScript (frontend)**
2. Migração **incremental com Strangler Pattern**
3. **OpenAPI + CI/CD + observabilidade** desde o início

Esse conjunto permite modernizar sem parar o negócio e sem reescrever tudo de uma vez.
