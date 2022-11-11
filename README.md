---
Projeto: ansible-syspass
Descrição: Esse projeto visa instalar e configurar o gerenciador de senha sysPass Será instalado o Apache, MariaDB e PHP. Assim, proteger o MariaDB e criar o banco de dados para o sysPass, clonar o projeto sysPass, executar as configurações e instalar o Composer. Para finalizar, criar a configuração https autoassinado para o host virtual e ativar o mesmo.
Autor: Glauber GF (mcnd2)
Data: 2022-11-03
---

# Instalar e Configurar o sysPass com Ansible

![Image](https://github.com/glaubergf/ansible-syspass/blob/main/pictures/login_syspass.png)

O [**sysPass**](https://syspass.org/en) é uma poderosa aplicação web que oferece gerenciamento de senhas de forma segura e colaborativa.

Ela possui muitas opções para facilitar e reforçar a segurança no compartilhamento de senhas entre equipes, departamentos ou clientes, incluindo ACLs, perfis, campos personalizados, valores predefinidos ou links públicos, entre muitos outros.

A configuração é gerenciada por meio de uma interface web intuitiva e permite definir opções como autenticação LDAP, correio, auditoria, backup, importação/exportação, etc.

Embora você possa usar a interface web para configurar o sysPass, os valores são armazenados no formato XML, portanto, uma maneira fácil de modificá-los sem qualquer interface do usuário.

O **Ansible** trabalha com os conceitos de inventário (_lista de máquinas que serão gerenciadas_), playbooks (_comandos ou passo-a-passo a ser executado_) e roles (_modularização do código_). Atualmente o Ansible pertence a **Red Hat**.

## Comandos

* Para checar ou executar uma role sozinha, execute o Ansible com o parâmetro "-t" (--tags) seguido do nome da role e "-C" (--check) para apenas checar sem de fato executar.

Exemplo para a role 'install-webserver':
```
ansible-playbook -i hosts main.yml -t install-webserver -C
```

* Executando o projeto por completo:

```
ansible-playbook -i hosts main.yml
```

## A Configuração

Uma breve descrição das tasks dentro de cada role:

Na role **install-webserver** é executado a instalação do Apache, MariaDB e PHP, seguido de alterações em alguns parâmetros do php.ini .

* tasks:

```
Adicionando o nome do Server no /etc/hosts
Modificando o arquivo de configuração modt (Message Of The Day)
Inserindo configuração no '/etc/profile' para o motd
Executando o 'apt-get update'
Instalando os servidor web Apache, banco de dados MariaDB e o PHP
Editando alguns parâmetros no arquivo de configuração 'php.ini'
Reiniciando o serviço Apache
```

Na role **create-syspassdb** primeiro se faz a configuração necessária para proteger o MariaDB e em seguida criar o banco de dados sysPass.

* tasks:

```
Iniciando e habilitando o serviço MariaDB
Conectando ao servidor usando 'login_unix_socket'
Removendo contas de usuários 'anônimos' para localhost
Removendo o MySQL 'test database'
Privilégios de liberação para usuário 'root'
Criando banco de dados com nome 'syspassdb'
Criando usuário 'syspassuser' para o banco de dados 'syspassdb'
```

Na role **install-syspass** se faz necessário clonar o projeto sysPass e executar as configurações. Em seguida será instalado o Composer.

* tasks:

```
Clonando a versão mais recente do sysPass do repositório no Github
Movendo o diretório 'syspass' para raiz do Apache (/var/www/html)
Definindo a propriedade de dono e group recursivamente para o diretório 'syspass'
Definindo permissões para outros diretórios (config,backup)
Baixando o Composer
Movendo o Composer Globalmente
Setando permissões no Composer
Instalando/atualizando requerimentos do 'composer.json'
```

Na role **config-host-virtual** será criado (copiado) o arquivo de configuração https autoassinado para o host virtual e ativando o mesmo.

* tasks:

```
Criando configuração Host Virtual Apache para o sysPass
Ativando o Host Virtual Apache
Reiniciando o serviço Apache
```

## Licença

**GNU General Public License** (_Licença Pública Geral GNU_), **GNU GPL** ou simplesmente **GPL**.

[GPLv3](https://www.gnu.org/licenses/gpl-3.0.html)

------

Copyright (c) 2022 Glauber GF (mcnd2)

Este programa é um software livre: você pode redistribuí-lo e/ou modificar
sob os termos da GNU General Public License conforme publicada por
a Free Software Foundation, seja a versão 3 da Licença, ou
(à sua escolha) qualquer versão posterior.

Este programa é distribuído na esperança de ser útil,
mas SEM QUALQUER GARANTIA; sem mesmo a garantia implícita de
COMERCIALIZAÇÃO ou ADEQUAÇÃO A UM DETERMINADO FIM. Veja o
GNU General Public License para mais detalhes.

Você deve ter recebido uma cópia da Licença Pública Geral GNU
junto com este programa. Caso contrário, consulte <https://www.gnu.org/licenses/>.

*

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>