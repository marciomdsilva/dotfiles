# Dotfiles

My dotfiles

## Programs needed

1. [Node.js and npm with Noode Version Manager](https://github.com/nvm-sh/nvm?tab=readme-ov-file)

- After curl the most recent repository of npm

```sh
source ~/.bashrc
nvm install --lts
```

2.Install ripgrep(`rg`) tool to search text on projects and is needed to use with fzf and telescope

```sh
sudo apt install ripgrep
```

3.Install fd-find(`fd`) tool to search files and directories on projects

```sh
sudo apt install fd-find
```

4. Install fuzzy finder(`fzf`) is a tool that make an interactive search in command line for text and files in terminal

```sh
sudo apt install fzf
```

5.Install `lazygit` a tool that is an interface(ui) for terminal for git

[jesseduffield/lazygit on Ubuntu](https://github.com/jesseduffield/lazygit?tab=readme-ov-file#debian-and-ubuntu)

```sh
LAZYGIT_VERSION=$(curl -s "https://api.github.com/repos/jesseduffield/lazygit/releases/latest" | \grep -Po '"tag_name": *"v\K[^"]*')
curl -Lo lazygit.tar.gz "https://github.com/jesseduffield/lazygit/releases/download/v${LAZYGIT_VERSION}/lazygit_${LAZYGIT_VERSION}_Linux_x86_64.tar.gz"
tar xf lazygit.tar.gz lazygit
sudo install lazygit -D -t /usr/local/bin/
```

## Create/Verify SSH Key to clone dotfiles repository

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

4. Go to GitHub → Settings → SSH and GPG keys: <https://github.com/settings/keys>

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

- If needed on the first commit to select the main branch

```sh
dotfiles push -u origin main
```
