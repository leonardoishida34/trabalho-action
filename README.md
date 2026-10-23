📝 trabalho-action

Trabalho de DevOps que usa o GitHub Actions para automatizar tarefas do repositório.

O projeto é um CRUD de tarefas feito em um único arquivo HTML (index.html), onde é possível criar, listar, editar e excluir tarefas.

⚙️ Como funciona

O repositório tem duas Actions principais, que ficam na pasta .github/workflows.

📜 Changelog
Quando roda: a cada push na branch main.
O que faz: lê o histórico de commits e escreve a lista no arquivo CHANGELOG.md.
Resultado: o changelog sempre fica atualizado sozinho, sem precisar editar à mão.
📦 Release
Quando roda: quando uma tag de versão é enviada, por exemplo v1.0.0.
O que faz: cria um arquivo .zip com o projeto e publica uma Release no GitHub.
Resultado: o zip fica disponível para download na aba Releases.
🚀 Como gerar uma nova versão

Depois de fazer seus commits e esperar o changelog atualizar, crie a tag:

bash
git pull
git tag v1.0.0
git push origin v1.0.0

A Action de release roda em seguida e publica o zip.

📥 Como baixar
Abra a aba Releases do repositório.
Escolha a versão desejada.
Em Assets, baixe o arquivo .zip.
Extraia e abra o index.html no navegador.
🛠️ Tecnologias

HTML, JavaScript, Git e GitHub Actions.