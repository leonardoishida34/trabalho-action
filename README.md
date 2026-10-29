# 🚀 trabalho-action

Repositório de **DevOps** com dois trabalhos que usam o **GitHub Actions**, aplicados em um mesmo projeto: um CRUD de tarefas feito em um único arquivo HTML (`index.html`), onde é possível **criar, listar, editar e excluir** tarefas.

👤 **Aluno:** Leonardo Hitoshi Ishida

| Parte | Tema |
|---|---|
| 1 | Changelog automático e Release (zip) |
| 2 | Deploy automático em VM com Runner local |

---

## 📜 Parte 1: Changelog e Release

Duas Actions automatizam tarefas do repositório. Elas ficam na pasta `.github/workflows`.

### Changelog (`changelog.yml`)

- **Quando roda:** a cada push na branch `main`.
- **O que faz:** lê o histórico de commits e escreve a lista no arquivo `CHANGELOG.md`.
- **Resultado:** o changelog sempre fica atualizado sozinho, sem precisar editar à mão.

### Release (`release.yml`)

- **Quando roda:** quando uma tag de versão é enviada, por exemplo `v1.0.0`.
- **O que faz:** cria um arquivo `.zip` com o projeto e publica uma Release no GitHub.
- **Resultado:** o zip fica disponível para download na aba **Releases**.

### Como gerar uma nova versão

Depois de fazer seus commits e esperar o changelog atualizar, crie a tag:

```bash
git pull
git tag v1.0.0
git push origin v1.0.0
```

A Action de release roda em seguida e publica o zip.

### Como baixar

1. Abra a aba **Releases** do repositório.
2. Escolha a versão desejada.
3. Em **Assets**, baixe o arquivo `.zip`.
4. Extraia e abra o `index.html` no navegador.

---

## 🖥️ Parte 2: Deploy automático em VM

A aplicação roda em uma **máquina virtual Ubuntu Server**, dentro de um container Docker com Nginx, e é atualizada automaticamente a cada push no repositório.

### Tecnologias

- HTML e JavaScript
- Docker e Docker Compose (imagem `nginx:alpine`)
- Ubuntu Server em máquina virtual (VirtualBox)
- GitHub Actions com Runner local (self-hosted)

### Runner local

O Runner é um programa instalado dentro da VM e registrado neste repositório. Ele roda como serviço e fica conectado ao GitHub, esperando por tarefas. Quando um workflow usa `runs-on: self-hosted`, o job não roda nos servidores do GitHub: ele roda **dentro da VM**, com acesso ao Docker dela.

Na página **Settings → Actions → Runners** do repositório, ele aparece como **Idle** quando está online e pronto.

### Deploy automático (`deploy.yml`)

O workflow roda a cada push na branch `main`:

1. O GitHub avisa o Runner que há um job novo.
2. O Runner (na VM) baixa o código atualizado do repositório.
3. Executa `docker compose up -d --build`, que reconstrói a imagem e recria o container.
4. A aplicação na VM passa a mostrar a versão nova, sem deploy manual.

### Como acessar a aplicação

Com a VM ligada, na rede do laboratório:

```
http://192.168.100.37:8080
```

O IP da VM é entregue por DHCP e pode mudar. Para conferir, rode `ip a` dentro da VM (endereço da interface `enp0s3`).

### Executar em outra máquina com Docker

```bash
git clone https://github.com/leonardoishida34/trabalho-action.git
cd trabalho-action
docker compose up -d --build
```

Depois acesse `http://localhost:8080`.

### Como testar o deploy

1. Altere algo no `index.html` (por exemplo, o título).
2. Faça `git add .`, `git commit` e `git push`.
3. Acompanhe o workflow **Deploy** na aba **Actions**.
4. Atualize a página no navegador (Ctrl+F5) e veja a nova versão.