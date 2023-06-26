### Setup
1. Multi desktop
	1. Add 15+ desktops
	2. Enable desktop keyboard shortcuts
	3. Turn off mission control "Automatically rearrange" setting
	4. Turn on "switch to a Space with open windows"
	5. Change "move focus to active or next window" from ctrl-F4 to ctrl-cmd-tab
2. bash
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
	4. set iTerm history to 10000
6. Sublime
	1. https://www.sublimetext.com/3
7. Obsidian
	1. https://obsidian.md/download
8. Create new ssh key
	1. ssh-keygen -t ed25519 -c "lance.gatlin@s-mach.net"
	2. Register with github.com for lancegatlin
	3. To set SSH key per repo:  `git config core.sshCommand "ssh -i ~/.ssh/id_rsa_example -F /dev/null"`
9. Notes
	1. clone dev_notes
		1. git@github.com:lancegatlin/dev_notes.git
	2. clone other notes as needed
10. sdkman
	1. curl -s "https://get.sdkman.io" | bash
11. jdk
	1. sdk list java
	2. sdk install java 11.0.2-open
12. Scala 2/3
	1. sdk install scala 2.13.11
13. sbt
	1. sdk install sbt
14. intellij
	1. https://www.jetbrains.com/idea/download/?section=mac
	2. scala plugin
		1. setup scala sdk
		2. setup java sdk
	3. Turn off code folding for imports
	4. 
15. docker
	1. brew install docker