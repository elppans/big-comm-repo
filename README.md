# BIG Community Repository

Pacote para Arch Linux que adiciona (e mantém atualizados) os repositórios oficiais do **BIGLinux** e do **BIGLinux Community**, permitindo instalar pacotes dessas comunidades diretamente com o `pacman`/`paru`/`yay`.

## O que este pacote faz

Ao instalar o `big-comm-repo`, os seguintes repositórios são adicionados ao `/etc/pacman.conf`:

```ini
[biglinux-update-stable]
SigLevel = PackageRequired
Server = https://repo.biglinux.com.br/update-stable/$arch

[community-stable]
SigLevel = PackageRequired
Server = https://repo.communitybig.org/stable/$arch

[community-extra]
SigLevel = PackageRequired
Server = https://repo.communitybig.org/extra/$arch

[biglinux-stable]
SigLevel = PackageRequired
Server = https://repo.biglinux.com.br/stable/$arch
```

- **Instalação/atualização** (`post_install` / `post_upgrade`):
  - Cria um backup do `/etc/pacman.conf` (`pacman.conf.backup_AAAAMMDDHHMMSS`) antes de qualquer alteração.
  - Importa e assina localmente as chaves dos repositórios BigLinux/BigCommunity.
  - Verifica cada repositório no `pacman.conf`:
    - Se já existir e estiver correto → não altera nada.
    - Se existir mas estiver diferente → atualiza para a configuração correta.
    - Se não existir → adiciona.
- **Remoção** (`post_remove`):
  - Remove os 4 blocos de repositório do `pacman.conf`.
  - Remove as chaves importadas do keyring do pacman.
- **Backups**: os arquivos `pacman.conf.backup_*` não são removidos automaticamente e ficam em `/etc`, caso você queira restaurar a configuração anterior.

## Instalação

```bash
temp_dir="$(mktemp -d big-comm-repo.XXXXXXXXXX)"
cd "$temp_dir"
git clone https://github.com/elppans/big-comm-repo.git
cd big-comm-repo
makepkg -si
cd -
rm -rf "$temp_dir"
```

Após a instalação, atualize as bases de dados do pacman:

```bash
sudo pacman -Syy
```

## Remoção

```bash
sudo pacman -R big-comm-repo
```

Os repositórios adicionados por este pacote serão removidos automaticamente do `pacman.conf`.

## Licença

[MIT](https://github.com/elppans/big-comm-repo/blob/main/LICENSE)

## Agradecimentos

Agradecimentos especiais a **Bruno Gonçalves**, criador do [**BIGLinux**](https://www.biglinux.com.br/), e a **Tales A. Mendonça**, criador do [**BIGLinux Community**](https://communitybig.org/), pelo trabalho contínuo em manter essas distribuições e repositórios disponíveis para toda a comunidade.
