**Comment avoir des alias dans son shell? **

Par défaut, les fichiers .bash_profile et .bashrc n'existent pas. 

Dans le .bash_profile
	
	[ -r ~/.bashrc ] && source ~/.bashrc

Dans le .bashrc (par exemple)

	alias git="/usr/local/git/bin/git"
	alias ll="ls -al"
	export CLICOLOR=1
	
À noter que le système cherche dans cet ordre : 

	/etc/profile
	~/.bash_profile
	~/.bash_login
	~/.profile
