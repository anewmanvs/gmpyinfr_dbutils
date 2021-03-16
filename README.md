# gmpyinfr_dbutils

Módulo de funções e métodos úteis de banco de dados para o dia-a-dia de uma equipe de Ciência de Dados.

## Instalação

**Não há cobertura para utilização no Windows**. Este pacote e o passo-a-passo de instalação tem funcionamento garantido nas seguintes distros:

**Debian**

- 8 (jessie)
- 9 (stretch)
- 10 (buster)

**Ubuntu**

- 20.04 (focal)
- 19.04 (disco)
- 18.04 (bioni)
- 16.04 (xenial)
- 14.04 (trusty)

Demais distribuições linux devem funcionar sem problemas mas têm comandos e processo de instalação diferentes. Caso este seja o seu caso, por favor verifique a documentação do [`turbodbc`](https://turbodbc.readthedocs.io/en/latest/pages/getting_started.html), [`Microsoft SQL Server`](https://docs.microsoft.com/en-us/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server?view=sql-server-ver15) e [`PostgreSQL`](https://www.postgresql.org/download/linux/).

### C++ Packages para Debian/GNU Linux, Ubuntu e CentOS

Execute os comandos abaixo, na ordem fornecida

```bash
sudo apt update
sudo apt install -y ca-certificates lsb-release wget libboost-all-dev unixodbc-dev python-dev unixodbc

wget https://apache.bintray.com/arrow/$(lsb_release --id --short | tr 'A-Z' 'a-z')/apache-arrow-archive-keyring-latest-$(lsb_release --codename --short).deb

sudo apt install -y ./apache-arrow-archive-keyring-latest-$(lsb_release --codename --short).deb
sudo apt update 
sudo apt install -y libarrow-dev libarrow-dataset-dev libarrow-python-dev
```

### Microsoft SQL Server drivers

Apenas após a finalização com sucesso dos passos acima, execute os seguintes comandos. Para mais detalhes específicos da sua distro e passo a passo mais detalhado, visite a [página oficial da Microsoft](https://docs.microsoft.com/en-us/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server?view=sql-server-ver15).

```bash
sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl gnupg2 lsb-release

sudo su

version=$(lsb_release -d --short | sed "s@GNU/Linux@@" | tr 'A-Z' 'a-z' | grep -Po '([a-z]+\ *[0-9]+(?:\.[0-9]+)?)' | sed -E "s@\s+@ @" | tr ' ' '/')
regex="([a-z]+)/([0-9]+[.0-9]*)"
version=$(if [[ $version =~ $regex ]]; then if [ "${BASH_REMATCH[1]}" == "debian" ]; then echo "$version" | grep -Po '([a-z]+/[0-9]+)'; else echo "$version"; fi; fi)

curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/$version/prod.list > /etc/apt/sources.list.d/mssql-release.list

exit

sudo apt update
sudo ACCEPT_EULA=Y apt -y install msodbcsql17
```

### PostgreSQL drivers

A instalação dos drivers do PostgreSQL é um pouco mais simples, embora alguns erros possam ocorrer. Siga o passo-a-passo abaixo para instalação dos drivers:

```bash
sudo apt update
sudo apt install -y odbc-postgresql libpq-dev
```

#### Troubleshooting

Em algumas situações, durante a utilização, este tipo de erro pode ocorrer:

```bash
[unixODBC][Driver Manager]Can't open lib 'psqlodbcw.so' : file not found (0) (SQLDriverConnect)
```

Favor verifique o conteúdo do arquivo `odbcinst.ini` que pode ser encontrado normalmente em `/etc/odbcinst.ini` ou em `$HOME/.odbcinst.ini`. Caso neste arquivo a linha que indica o local do driver esteja preenchida sem o *fullpath* faça a correção inserindo o caminho completo da lib.

### Instalação do pacote

Após a instalação das dependências acima, pode-se instalar o pacote através do pip, pelo comando

```bash
pip install gmpyinfr-dbutils
```

Há um projeto mantido no PyPi para facilitar o acesso ao pacote. Pode-se visualizar através [deste link](https://pypi.org/project/gmpyinfr-dbutils/).

Se pretende instalar a partir do source (este repositório), deve-se criar as wheels e instalar a partir do pip através do seguinte comando:

```bash
python setup.py bdist_wheel
pip install dist/*.whl
rm -rf build/ gmpyinfr_dbutils.egg-info/ dist/
```