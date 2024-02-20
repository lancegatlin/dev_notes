### Setup
1. Multi desktop
	1. Add 15+ desktops
	3. Mission control
		1. Turn off mission control "Automatically rearrange" setting
		2. Turn on "switch to a Space with open windows"
		3. Turn off all hot corners (`Hot Corners` button)
	4. Keyboard shortcuts
		1. Enable desktop keyboard shortcuts under Keyboard shortcuts -> Mission Control (expand down caret on left)
		2. Change all "Switch to Desktop X" where X >= 10  to "shift-ctrl-X"
		3. Change "move focus to active or next window" from ctrl-F4 to ctrl-cmd-tab under Keyboard Shortcuts -> Keyboard
2. in Mac Finder app, add "computer" and user home directory in settings
3. always show hidden files:
	4. `defaults write com.apple.Finder AppleShowAllFiles true`
	5. `killall Finder`
4. Settings -> Control Center -> Bluetooth -> 'Show in Menu Bar' (or search bluetooth)
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
	2. black/green color scheme
		1. brew tap homebrew/cask-fonts && brew install --cask font-source-code-pro
		2. download https://iterm2colorschemes.com homebrew scheme
		3. Settings -> Profiles (Default) -> Colors
			1. ClickbBottom right dropdown "Color Presets..."
			2. import homebrew terminal colors (green)
	3. Settings -> Profiles (default) -> Terminal -> Scrollback Buffer
		1. set iTerm history to 10000
	4. in Finder change default open with to iTerm (create a script then right click -> Get Info -> Open with)
	5. fix arrow nav hot keys
		3. Fix being/end of line nav
			1. Under Settings -> General -> Keys -> Key Bindings
				1. Change entry ⌘ Command← to Send Hex code: 0x01
				2. Change entry ⌘ Command→ to Send Hex code: 0x05
		4. Fix word nav
			1. Remove existing: Settings -> Profiles -> Keys -> Key Mappings then remove profile key bindings for option-left/right (using minus sign)
			2. [ ] Under Settings -> General -> Keys -> Key Bindings add:
				1. Change entry option+left to escape-b 
				2. Change entry option+right to escape-f
6. Sublime
	1. https://www.sublimetext.com/3
7. Obsidian
	1. https://obsidian.md/download
	2. keymaps
		1. cmd-l to search in all files (remove existing) (todo:fix me)
8. Create new ssh key
	1. ssh-keygen -t ed25519 -c "lance.gatlin@s-mach.net"
	2. Register with github.com for lancegatlin
	3. Note: if possible generate NEW ssh key and add to lancegatlin and corp user
		1. If using different SSH keys:
			1. To set SSH key per repo:  `git config core.sshCommand "ssh -i ~/.ssh/id_rsa_example -F /dev/null"`
9. Notes
	1. clone dev_notes
		1. git@github.com:lancegatlin/dev_notes.git
	2. clone other notes as needed
10. install gpg/pass
	1. brew install gpg
		1. generate new key: `gpg --full-generate-key`
			1. default kind of key (sign/encrypt)
			2. default curve
			3. default does not expire
		2. set gpg cache timeout to 90 minutes
			1. edit `~/.gnupg/gpg-agent.conf`:
```
default-cache-ttl 5400
max-cache-ttl 5400
```
	2. brew install pass
		1. init password store: `pass init <gpg key hex id>`
11. sdkman
	1. `curl -s "https://get.sdkman.io" | bash`
12. jdk
	1. `sdk list java`
	2. `sdk install 11.0.22-zulu`
		1. note: sbt doesn't work with Java >17 yet
			1. https://stackoverflow.com/questions/76151072/error-during-sbt-launcher-java-lang-unsupportedoperationexception-the-security
13. Scala 2/3
	1. `sdk install scala 2.13.11`
14. sbt
	1. `sdk install sbt`
15. intellij
	1. https://www.jetbrains.com/idea/download/?section=mac
	2. scala plugin
		1. setup scala sdk
		2. setup java sdk
	3. Turn off code folding for imports
	4. keymap
		1. Change Move Caret to Text Start to cmd-up (and remove others)
		2. Change Move Caret to Text End to cmd-down (and remove others)
		3. Change Show Type from ctrl-shift-P to ctrl-F (?)
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
	1. brew install python (installs python3)
21. aws
	1. awscli
	1. brew install awscli
	2. install localstack (as needed)
		1. brew install localstack/tap/localstack-cli
		2. pip3 install awscli-local
22. azure-cli
	1. brew install azure-cli