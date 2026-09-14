# 🐳 Multi-App Orchestration Environment (Laravel + WordPress + Nginx Proxy)

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white)
![Laravel](https://img.shields.io/badge/Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)
![WordPress](https://img.shields.io/badge/WordPress-21759B?style=for-the-badge&logo=wordpress&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)

Uma arquitetura de desenvolvimento local modular orquestrada via **Docker Compose**, integrando três aplicações isoladas (`app-admin`, `app-services` e `app-cms`) através de um **Nginx Reverse Proxy Gateway** centralizado com suporte a domínios virtuais customizados (`.local`).
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
Recursos e Tecnologias

    Isolamento de Redes: Redes bridge customizadas gerenciadas pelo Docker.

    Roteamento Centralizado: Gateway Nginx manipulando instâncias PHP 8.3 FPM e Apache WordPress.

    Serviços de Banco de Dados: Três contêineres MySQL 8.0 independentes com mapeamento de portas locais exclusivas.

    Higienização de Código: Estrutura preparada para versionamento sem exposição de chaves privadas, credenciais ou IPs de rede interna.
Matriz de Portas e ConexõesPara conexões externas via SGDB (DBeaver, TablePlus, VS Code Client):ServiçoAplicaçãoHost LocalPorta ExternaBanco de Dadosadmin_mysqlApp Admin (Laravel)127.0.0.13306admin_dbservices_mysqlApp Services (Laravel)127.0.0.13307services_dbcms_mysqlApp CMS (WordPress)127.0.0.13308
🚀 Como Executar o Ambiente Locamente
1. Configurar os Domínios Locais

Adicione os domínios de desenvolvimento ao arquivo /etc/hosts da sua máquina:
Bash

sudo nano /etc/hosts

Adicione as linhas:
Plaintext

127.0.0.1    admin.local
127.0.0.1    services.local
127.0.0.1    cms.local

2. Subir a Infraestrutura Docker
Bash

# 1. Subir o Gateway Proxy
docker compose -f docker-compose.yml up -d

# 2. Subir os Contêineres de Aplicação
docker compose -f docker-compose.admin.yml up -d
docker compose -f docker-compose.services.yml up -d
docker compose -f docker-compose.cms.yml up -d

3. Setup das Aplicações Laravel
Bash

# App Admin
cp apps/app-admin/.env.example apps/app-admin/.env
docker exec -it app_admin php artisan key:generate

# App Services
cp apps/app-services/.env.example apps/app-services/.env
docker exec -it app_services php artisan key:generate

🌐 Endereços de Acesso

    App Admin (Laravel): http://admin.local

    App Services (Laravel): http://services.local

    App CMS (WordPress): http://cms.local

📄 Licença

Este projeto está sob a licença MIT.
