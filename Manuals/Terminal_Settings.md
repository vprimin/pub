![thb](https://github.com/vprimin/pub/blob/main/Manuals/images/iterm.png)

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
Если это виртуалка UTM можно переложить в `sudo mv micro /opt/homebrew/bin`

Если будет выходить ошибка 'zsh: bad CPU type in executable:', то запустить команду 

`softwareupdate --install-rosetta`

Oh-my-zsh
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
Редактируем настройки:
`micro ~/.zshrc` 

`plugins=(sudo macos sublime)`
`ZSH_THEME='dst'`  
*(Еще нравятся:  'mh' и 'bureau' )*
[Полный каталог тем](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes#gallifrey)

Далее ставим
Fig+iTerm2+Warp
```
brew install --cask fig
brew install --cask iterm2
brew install --cask warp
```

*** 
еще я меняю иконку для iTerm2, но это уже 
Вот [тут](https://github.com/jasonlong/iterm2-icons) инструкция
И устанавливаю шрифт для монохромного текста [Source Code Pro](https://fonts.google.com/specimen/Source+Code+Pro) и в настройках iterm2 указываю использовать этот шрифт