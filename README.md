# Projeto Cloud - DermaPura Cosméticos (Case 3)

One-page institucional da **DermaPura Cosméticos** (Cosméticos / Higiene Pessoal), desenvolvida como exercício prático de Computação em Nuvem (LAB11).

##  Objetivo

Publicar a one-page do case DermaPura em uma instância **Amazon EC2**, com:

- Versionamento de código no **GitHub**
- Pipeline de **CI/CD** automatizado via **GitHub Actions**
- Empacotamento da aplicação com **Docker** (IaaS)

##  Arquitetura

```
Push no GitHub (branch main)
        │
        ▼
GitHub Actions (CI/CD)
        │  (SSH)
        ▼
Instância EC2 (AWS)
        │
        ▼
docker build + docker run
        │
        ▼
Nginx servindo index.html na porta 80
```

##  Estrutura do repositório

```
.
├── index.html              # One-page da DermaPura
├── Dockerfile               # Imagem com Nginx servindo a página
├── .github/workflows/
│   └── deploy.yml            # Pipeline de CI/CD (deploy automático na EC2)
└── README.md
```

##  Como executar localmente

```bash
docker build -t dermapura-onepage .
docker run -d -p 80:80 --name dermapura dermapura-onepage
```

Acesse: http://localhost

##  Deploy na AWS (EC2)

1. Instância EC2 com Docker instalado.
2. Clonar este repositório dentro da instância:
   ```bash
   git clone https://github.com/SEU_USUARIO/projeto-cloud-dermapura.git
   cd projeto-cloud-dermapura
   ```
3. Build e execução do container:
   ```bash
   docker build -t dermapura-onepage .
   docker run -d -p 80:80 --name dermapura --restart always dermapura-onepage
   ```
4. Acessar via navegador: `http://<IP_PUBLICO_DA_EC2>`

##  CI/CD (GitHub Actions)

Todo push na branch `main` dispara o workflow [`deploy.yml`](.github/workflows/deploy.yml), que:

1. Conecta via SSH na instância EC2.
2. Atualiza o código (`git pull`).
3. Reconstrói a imagem Docker (`docker build`).
4. Reinicia o container com a nova versão (`docker run`).

### Secrets necessários no repositório

Configurar em **Settings → Secrets and variables → Actions**:

| Secret | Descrição |
|---|---|
| `EC2_HOST` | IP público da instância EC2 |
| `EC2_USER` | Usuário SSH (ex.: `ec2-user`) |
| `EC2_SSH_KEY` | Chave privada (.pem) usada para acessar a EC2 |

##  Sobre o Dockerfile

A imagem é baseada em `nginx:alpine` (leve) e apenas copia o arquivo `index.html` para o diretório padrão servido pelo Nginx, expondo a porta 80.

##  Equipe

- Integrante 1 — RA: 25170645

##  Disciplina

Computação em Nuvem — LAB11 · Publicando a One-Page da sua empresa na AWS · Case 3 (DermaPura)
