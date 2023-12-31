### Setup
1. Multi desktop
	1. Add 15+ desktops
	2. Enable desktop keyboard shortcuts
	3. Turn off mission control "Automatically rearrange" setting
	4. Turn on "switch to a Space with open windows"
	5. Change "move focus to active or next window" from ctrl-F4 to ctrl-cmd-tab
2. in Mac Finder app, add "computer" and user home directory in settings
3. always show hidden files:
	4. `defaults write com.apple.Finder AppleShowAllFiles true`
	5. `killall Finder`
4. Settings -> Control Center -> Bluetooth -> 'Show in Menu Bar'
5. bash
	1. set history to 1000
	2. 
	
### Applications
1. Sizeup
	1. https://www.irradiatedsoftware.com/downloads/?file=SizeUp.zip
2. Brew
	1. /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
3. misc
	1. brew install wget
	2. 
4. Git
	1. brew install git
5. iTerm2
	1. brew install iterm2
	2. brew tap homebrew/cask-fonts && brew install --cask font-source-code-pro
	3. import homebrew terminal colors (green)
	5. set iTerm history to 10000
	6. in Finder change default open with to iTerm (create a script then right click -> Get Info -> Open with)
	7. fix arrow nav hot keys
		1. Go to settings (⌘ Command+,)
		2. Go to tab Keys
		3. Under "Key Bindings"
		4. Change entry ⌘ Command← to Send Hex code: 0x01
		5. Change entry ⌘ Command→ to Send Hex code: 0x05
		6. Change entry option+left to escape-b
		7. Change entry option+right to escape-f
		8. Settings -> Profile -> Keys then remove profile key bindings for option-left/right
6. Sublime
	1. https://www.sublimetext.com/3
7. Obsidian
	1. https://obsidian.md/download
	2. keymaps
		1. cmd-l to search in all files (remove existing)
		2. 
8. Create new ssh key
	1. ssh-keygen -t ed25519 -c "lance.gatlin@s-mach.net"
	2. Register with github.com for lancegatlin
	3. To set SSH key per repo:  `git config core.sshCommand "ssh -i ~/.ssh/id_rsa_example -F /dev/null"`
9. Notes
	1. clone dev_notes
		1. git@github.com:lancegatlin/dev_notes.git
	2. clone other notes as needed
10. install gpg/pass
	1. brew install gpg
		1. generate new key: `gpg --full-generate-key`
		2. set gpg cache timeout to 90 minutes
			1. edit `~/.gnupg/gpg-agent.conf`:
```
default-cache-ttl 5400
max-cache-ttl 5400
```
	2. brew install pass
		1. init password store: `pass init <hex id>`
11. sdkman
	1. curl -s "https://get.sdkman.io" | bash
12. jdk
	1. sdk list java
	2. sdk install java 11.0.2-open
13. Scala 2/3
	1. sdk install scala 2.13.11
14. sbt
	1. sdk install sbt
15. intellij
	1. https://www.jetbrains.com/idea/download/?section=mac
	2. scala plugin
		1. setup scala sdk
		2. setup java sdk
	3. Turn off code folding for imports
	4. keymap
		1. Change Move Caret to Text Start to cmd-up (and remove others)
		2. Change Move Caret to Text End to cmd-down (and remove others)
		3. Change Show Type from 
16. docker/compose
	1. brew install docker
	2. brew install docker-compose
	3. install docker desktop
		1. https://www.docker.com/products/docker-desktop/
17. 
18. create `~/bin` for scripts
	1. add to `~/.bash_profile`
	2. create `upd.sh`
19. yq/jq
	1. brew install yq jq
20. install python/pip
	1. brew install python
21. aws
	1. awscli
	1. brew install awscli
	2. install localstack (as needed)
		1. brew install localstack/tap/localstack-cli
		2. pip3 install awscli-local
22. azure-cli
	1. brew install azure-cli