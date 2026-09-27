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

- **Instalação/atualização** (`post_install` / `post_upgrade`): verifica cada repositório no `pacman.conf`.
  - Se já existir e estiver correto → não altera nada.
  - Se existir mas estiver diferente → atualiza para a configuração correta.
  - Se não existir → adiciona.
- **Remoção** (`post_remove`): remove os 4 blocos de repositório do `pacman.conf`, deixando o sistema como estava antes da instalação.

## Dependências

- [`biglinux-keyring`](https://github.com/biglinux/biglinux-keyring)
```bash
https://github.com/biglinux/biglinux-keyring.git
cd biglinux-keyring || exit 1
makepkg -Cris
```
- [`community-keyring`](https://github.com/big-comm/community-keyring)
```bash
https://github.com/big-comm/community-keyring.git
cd community-keyring || exit 1
makepkg -Cris
```

Essas keyrings são necessárias para validar as assinaturas dos pacotes vindos dos repositórios acima.

## Instalação

```bash
git clone https://github.com/elppans/big-comm-repo.git
cd big-comm-repo
makepkg -si
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

MIT

## Agradecimentos

Agradecimentos especiais a **Bruno Gonçalves**, criador do **BIGLinux**, e a **Tales A. Mendonça**, criador do **BIGLinux Community**, pelo trabalho contínuo em manter essas distribuições e repositórios disponíveis para toda a comunidade.
