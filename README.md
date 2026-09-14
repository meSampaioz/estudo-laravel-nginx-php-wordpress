# 🐳 Multi-App Orchestration Environment (Laravel + WordPress + Nginx Proxy)

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white) ![Nginx](https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white) ![Laravel](https://img.shields.io/badge/Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white) ![WordPress](https://img.shields.io/badge/WordPress-21759B?style=for-the-badge&logo=wordpress&logoColor=white) ![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)

Uma arquitetura de desenvolvimento local modular orquestrada via **Docker Compose**, integrando três aplicações isoladas (`app-admin`, `app-services` e `app-cms`) através de um **Nginx Reverse Proxy Gateway** centralizado com suporte a domínios virtuais customizados (`.local`).

## 🏗️ Arquitetura da Solução

```
                          ┌─────────────────────────────┐
                          │    Nginx Reverse Proxy      │
                          │   (Gateway - Porta :80)     │
                          └──────────────┬──────────────┘
                                         │
        ┌────────────────────────────────┼────────────────────────────────┐
        │                                │                                │
        ▼                                ▼                                ▼
┌───────────────┐                ┌───────────────┐                ┌───────────────┐
│   App Admin   │                │ App Services  │                │    App CMS    │
│ (Laravel 10)  │                │ (Laravel 10)  │                │  (WordPress)  │
│  admin.local  │                │services.local │                │   cms.local   │
└───────┬───────┘                └───────┬───────┘                └───────┬───────┘
        │                                │                                │
        ▼                                ▼                                ▼
┌───────────────┐                ┌───────────────┐                ┌───────────────┐
│ MySQL Database│                │ MySQL Database│                │ MySQL Database│
│ (Porta 3306)  │                │ (Porta 3307)  │                │ (Porta 3308)  │
└───────────────┘                └───────────────┘                └───────────────┘
```

## 🛠️ Recursos e Tecnologias

- **Isolamento de Redes:** Redes bridge customizadas gerenciadas pelo Docker.
- **Roteamento Centralizado:** Gateway Nginx manipulando instâncias PHP 8.3 FPM e Apache WordPress.
- **Serviços de Banco de Dados:** Três contêineres MySQL 8.0 independentes com mapeamento de portas locais exclusivas.
- **Higienização de Código:** Estrutura preparada para versionamento sem exposição de chaves privadas, credenciais ou IPs de rede interna.

## 🗄️ Matriz de Portas e Conexões

| Serviço | Aplicação | Host Local | Porta Externa | Banco de Dados |
|---|---|---|---|---|
| admin_mysql | App Admin (Laravel) | 127.0.0.1 | 3306 | admin_db |
| services_mysql | App Services (Laravel) | 127.0.0.1 | 3307 | services_db |
| cms_mysql | App CMS (WordPress) | 127.0.0.1 | 3308 | cms_db |

## 🚀 Como Executar o Ambiente Localmente

### 1. Configurar os Domínios Locais

Adicione os domínios de desenvolvimento ao arquivo `/etc/hosts` da sua máquina:

```bash
sudo nano /etc/hosts
```

Adicione as linhas:

```
127.0.0.1    admin.local
127.0.0.1    services.local
127.0.0.1    cms.local
```

### 2. Subir a Infraestrutura Docker

```bash
docker compose -f docker-compose.yml up -d
docker compose -f docker-compose.admin.yml up -d
docker compose -f docker-compose.services.yml up -d
docker compose -f docker-compose.cms.yml up -d
```

### 3. Setup das Aplicações Laravel

```bash
cp apps/app-admin/.env.example apps/app-admin/.env
docker exec -it app_admin php artisan key:generate

cp apps/app-services/.env.example apps/app-services/.env
docker exec -it app_services php artisan key:generate
```

## 🌐 Endereços de Acesso

- **App Admin (Laravel):** http://admin.local
- **App Services (Laravel):** http://services.local
- **App CMS (WordPress):** http://cms.local

## 📄 Licença

Este projeto está sob a licença MIT.
