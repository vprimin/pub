Ставим Homebrew командой
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
или еще проще, скачиваем .pkg файл и запускаем
https://github.com/Homebrew/brew/releases 
Далее добавляем brew в PATH
```
(echo; echo 'eval "$(/opt/homebrew/bin/brew shellenv)"') >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```
далее ставим Midnight Commander
```
brew install mc
```
Далее ссылку на iCloud
```
ln -s ~/Library/Mobile\ Documents/com\~apple\~CloudDocs ~/iCloud
```
Текстовый редактор Micro
```
curl https://getmic.ro | bash

sudo mv micro /usr/local/bin 
```
Oh-my-zsh
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
`micro ~/.zshrc` 
Theme='**bureau**' %% 'mh', 'dst' %%
[Каталог тем](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes#gallifrey)
`plugins=(sudo macos sublime)`
fig.io
```
brew install --cask warp
```
Warp