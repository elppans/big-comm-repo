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
>Faça um backup do arquivo /etc/pacman.conf antes de instalar os pacotes:  

```bash
sudo cp -av /etc/pacman.conf /etc/pacman.conf."$(date +%Y%m%d%H%M)"
```
- [`biglinux-keyring`](https://github.com/biglinux/biglinux-keyring)

```bash
sudo ln -sf /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorcdn
temp_dir="$(mktemp -d biglinux-keyring.XXXXXXXXXX)"
cd "$temp_dir"
curl -O https://repo.biglinux.com.br/stable/x86_64/biglinux-keyring-20220827-3-any.pkg.tar.zst
curl -O https://repo.biglinux.com.br/stable/x86_64/biglinux-keyring-20220827-3-any.pkg.tar.zst.sig
key_id="$(echo "$(gpg --homedir /etc/pacman.d/gnupg --verify biglinux-keyring-20220827-3-any.pkg.tar.zst.sig biglinux-keyring-20220827-3-any.pkg.tar.zst 2>&1 || true)" | grep -E 'using|usando|usar' | grep -oiE '[0-9a-f]{8,40}')"
sudo pacman-key --recv-keys "$key_id" --keyserver keyserver.ubuntu.com
sudo pacman-key --lsign-key "$key_id"
sudo pacman -U --noconfirm biglinux-keyring-20220827-3-any.pkg.tar.zst
cd -
rm -rf "$temp_dir"
sed -i 's/SyncFirst/# SyncFirst/g' /etc/pacman.conf
```
- [`community-keyring`](https://github.com/big-comm/community-keyring)
```bash
temp_dir="$(mktemp -d community-keyring.XXXXXXXXXX)"
cd "$temp_dir"
git clone https://github.com/big-comm/community-keyring.git
cd community-keyring
makepkg -Cris
cd -
rm -rf "$temp_dir"
```

Essas keyrings são necessárias para validar as assinaturas dos pacotes vindos dos repositórios acima.


## Instalação

```bash
temp_dir="$(mktemp -d community-keyring.XXXXXXXXXX)"
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

Agradecimentos especiais a **Bruno Gonçalves**, criador do **BIGLinux**, e a **Tales A. Mendonça**, criador do **BIGLinux Community**, pelo trabalho contínuo em manter essas distribuições e repositórios disponíveis para toda a comunidade.
