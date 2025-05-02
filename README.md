# Dotfiles

My dotfiles

## Create/Verify SSH Key

1. Verify if SSH exist

```bash
ls ~/.ssh/id_rsa.pub
```

2. If don't exist create one

```sh
ssh-keygen -t ed25519 -C "your@email.com"
```

3. Copy the key

```sh
cat ~/.ssh/id_ed25519.pub
```

4. Go to GitHub → Settings → SSH and GPG keys: https://github.com/settings/keys

5. Click on "New SSH Key"

6. Change the origin to use SSH instead of HTTPS (if needed)

```sh
dotfiles remote set-url origin git@github.com:marciomdsilva/dotfiles.git
```

## Clone the repository directly in home (~/) using a "bare" repository

[Stow or Bare repo by chezmoi](https://www.atlassian.com/git/tutorials/dotfiles)

1. Create the variables

```sh
GIT_REPO="git@github.com:marciomdsilva/dotfiles.git"
DOTFILES_DIR="$HOME/.dotfiles"
```

2. Clone the repository like bare

```sh
git clone --bare "$GIT_REPO" "$DOTFILES_DIR"
```

3. Define temporary alias

```sh
alias dotfiles='/usr/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'
```

4. Ensures we won't see unversioned files in the status

```sh
dotfiles config --local status.showUntrackedFiles no
```

5. Try to apply archives in home

```sh
dotfiles checkout
```

6. Define permanent alias

```sh
SHELL_RC="$HOME/.bashrc"
if [ -n "$ZSH_VERSION" ]; then
  SHELL_RC="$HOME/.zshrc"
fi

if ! grep -q "alias dotfiles=" "$SHELL_RC"; then
  echo "Added permanente alias to $SHELL_RC..."
  echo "alias dotfiles='/usr/bin/git --git-dir=\$HOME/.dotfiles/ --work-tree=\$HOME'" >> "$SHELL_RC"
else
  echo "Alias already exist in $SHELL_RC"
fi
```
