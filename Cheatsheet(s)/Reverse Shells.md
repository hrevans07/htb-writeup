* If you're in a netcat reverse shell using zsh as your terminal locally, use these commands to give proper shell
	* python -c 'import pty;pty.spawn("/bin/bash")'
	* \^z
	* stty raw -echo; fg
	* enter