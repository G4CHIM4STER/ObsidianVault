___
Теги: #idea 
Дата создания: 2025-08-05 11:20 
Последнее изменение: вторник 5-го августа 2025 11:20:33
<< [[2025-08-04]] | [[2025-08-06]] >> 
___
# Короткий, но очень важный забег по технологиям
---


### 1. GitLab CI/CD

- **Что освоить:**
    - Как работает `.gitlab-ci.yml` (stages, jobs, variables, artifacts, cache).
    - Разные варианты запуска окружений: `shell`, `docker`, `docker-in-docker (dind)`, `docker-out-of-docker (dood)`.
    - Secrets & Variables (как хранить токены, пароли).
    - Триггеры, manual jobs, schedules.
    - Артефакты и деплой.
- **Материалы:**
    
    - Документация: [CI/CD Pipelines](https://docs.gitlab.com/ee/ci/pipelines/)
    - Бесплатный курс: [GitLab CI/CD Fundamentals](https://gitlab.edcast.com/pathways/gitlab-ci-cd-fundamentals)
    - Видео: [GitLab CI/CD Crash Course](https://www.youtube.com/watch?v=oxuRxtrO2Ag)
### 2. **Bash скриптинг**

- **Что освоить:**
    
    - Основы (циклы, условия, функции, переменные).
    - Работа с файлами, stdin/stdout/stderr, пайплайны.
    - trap и обработка ошибок.
    - Писать скрипты для автоматизации (чисто и модульно).
- **Материалы:**
    - Книга: _Classic Shell Scripting_ (Arnold Robbins).
    - Онлайн: [Bash Guide](https://mywiki.wooledge.org/BashGuide)
    - Практика: [exercises](https://www.shellcheck.net/) + [learnshell.org](https://www.learnshell.org/)

### 3. **Работа с облаками (Yandex Cloud + boto3)**

- **Что освоить:**
    
    - Подключение через `boto3` (AWS SDK).
    - Работа с Object Storage (S3 API): upload, download, list, delete.
    - Работа с IAM (токены, сервисные аккаунты).
    - Организация пайплайнов с облаком (например, выгрузка артефактов).        
- **Материалы:**
    
    - Документация: [Yandex Cloud + boto3](https://cloud.yandex.ru/docs/storage/tools/boto3)
    - AWS S3 + boto3 (аналогия): [boto3 docs](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/s3.html)
    - Примерный туториал: [Python + S3 with boto3](https://realpython.com/python-boto3-aws-s3/)        

---

# Глобальный и не менее важный заход в DevOps и архитектуру
## 1. Теория построения систем
Цель: понять как устроены **масштабируемые**, **отказоустойчивые**, **распределённые**
системы.
Основные ресурсы:
- [ ] Designing Data-Intensive Applications (Martin Kleppmann) — MUST READ
- [ ] System Design Primer (GitHub)
- [ ] Talks от Gregor Hohpe, Randy Shoup (GOTO Conf)
- [ ] Software Engineering at Google (abseil.io)
Ключевые темы:
- [ ] CAP теорема, BASE vs ACID, Consistency tradeoffs
- [ ] CQRS, Event Sourcing, Pub/Sub
- [ ] Stateless vs Stateful systems
- [ ] Load balancing, replication, sharding
- [ ] Fallacies of distributed computing
## 2. Лучшие DevOps практики (гибридные)
Цель: овладеть современными DevOps практиками, включая гибридные
развертывания.
Ресурсы:
- [ ] DevOps Handbook (Gene Kim)
- [ ] Site Reliability Engineering (Google SRE Book)
- [ ] Awesome DevOps GitHub Repo
Гибридные подходы:
- [ ] IaC: Ansible (best practices), Terraform (HashiCorp Learn)
- [ ] CI/CD: GitLab + on-prem runners + staging in cloud
- [ ] Monitoring: Prometheus, Alertmanager, Grafana, Zabbix, SNMP
- [ ] Secrets: HashiCorp Vault, Zero Trust, PKI
## 3. Kubernetes — от базы до продакшена
Цель: уверенно разворачивать, конфигурировать и поддерживать production-grade
Kubernetes.
Основы:
- [ ] FreeCodeCamp 4h YouTube Intro
- [ ] Labs: Play with Kubernetes, Katacoda
Продвинутые темы:
- [ ] Helm, Bitnami charts, custom values
- [ ] Ingress: NGINX, Traefik
- [ ] RBAC, NetworkPolicy, Cilium
- [ ] Prometheus Operator, kube-state-metrics
- [ ] GitOps: ArgoCD, Flux
Практика:
- [ ] k3s/kind кластер
- [ ] GitLab CI → Docker build → Helm deploy
- [ ] Canary releases, rollbacks, scaling
## 4. MLOps: следующий шаг
Цель: понять экосистему MLOps и быть готовым к интеграции ML моделей в
инфраструктуру.
Общие ресурсы:
- [ ] ml-ops.org, mlops.community
- [ ] Видео: “How Spotify does MLOps”, “MLOps explained”
- [ ] Инструменты: MLflow, Kubeflow, DVC, BentoML, ClearML, Feast
Ключевые этапы:
- [ ] Подготовка данных, пайплайны, мониторинг моделей
- [ ] CI/CD для моделей
- [ ] Drift detection, governance, versioning
## 5. Примерный учебный план на 12 недель
Примерный таймлайн (12 недель):
- [ ] 1–2: Архитектура систем (Kleppmann + system-design-primer)
- [ ] 3–4: DevOps практика (DevOps Handbook + Terraform/Ansible)
- [ ] 5–6: Kubernetes Core (FreeCodeCamp + Helm + k3s)
- [ ] 7–8: GitOps, CI/CD (GitLab → Helm deploy → ArgoCD)
- [ ] 9–10: Monitoring & Observability (Prometheus stack, Alertmanager)
- [ ] 11–12: Production-ready thinking (SLO, SLA, анти-паттерны, security)
+ далее постепенный переход в MLOps

___
### Zero-Links
- [[00 DevOps]]

### Links
- 
