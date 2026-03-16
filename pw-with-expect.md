# Passwords with expect command in Script {#passwords_with_expect_command_in_script}

This is not advised! You should never set passwords in a script. But if
for some reason you need to, here are some really good examples:
<https://www.bitbull.ch/wiki/index.php/Passw%C3%B6rter_mit_expect_einf%C3%BCttern>

## Code Example {#code_example}

In this code example we connect to the *DestinationServer* with the User
*AccessUser* and we want to run our commands as root with the argument
*1*. After the commands we have a newly added User *ThisIsTheUser* on
the *DestinationServer* with an ssh key and he is in the sudoers list.

**Info**: You need to press Enter twice after the command in order to
run the script. I could not figure out why though. I think it is a
timing problem (fault can not always be reproduced). **Wait 2 Seconds
with the first Enter!** Otherwise the commands will not be executed.

**Caution**: Change *su -* to *sudo su -* if needed.

Prerequisites

`sudo dnf install expect`

Command

`chmod +x ssh_expect_script.exp`\
`./Scriptname.exp ``<server>`{=html}` ``<user>`{=html}` ``<sudo>`{=html}\
\
`#Example`\
`./Scriptname.exp DestinationServer.your.domain AccessUser 1`

Code

`#!/usr/bin/expect -f`\
\
`# Set the timeout value for expect commands`\
`set timeout 20`\
\
`# Set the TERM environment variable`\
`set env(TERM)`\
\
`# Retrieve command-line arguments`\
`set server [lindex $argv 0]`\
`set user [lindex $argv 1]`\
`set sudo [lindex $argv 2]`\
\
`# Define the commands to be executed`\
`set cmd1 "id"`\
`set cmd2 "useradd -d /home/ThisIsTheUser -m ThisIsTheUser"`\
`set cmd3 "mkdir /home/ThisIsTheUser/.ssh"`\
`set cmd4 "chown ThisIsTheUser. /home/ThisIsTheUser/.ssh"`\
`set cmd5 "chmod 700 /home/ThisIsTheUser/.ssh"`\
`set cmd6 {echo 'from="10.115.5.10" ssh-rsa ThisIsTheKey= ThisIsTheUser@ThisIsTheServer.your.domain' > /home/ThisIsTheUser/.ssh/authorized_keys} `\
`set cmd7 "chmod 600 /home/ThisIsTheUser/.ssh/authorized_keys"`\
`set cmd8 "chown ThisIsTheUser. /home/ThisIsTheUser/.ssh/authorized_keys"`\
`set cmd9 {grep -q ThisIsTheUser /etc/sudoers || echo 'ThisIsTheUser ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers}`\
`set cmd10 {grep -v '^#' /etc/security/access.conf | grep admin && usermod -a -G root ThisIsTheUser}`\
`set cmd11 "chmod -R go-rwx /home/ThisIsTheUser"`\
`set cmd12 {grep -e ^AllowGroups -e ^AllowUsers /etc/ssh/sshd_config >/dev/null 2>&1 && echo ssh_restrict}`\
`set cmd13 {grep -q ThisIsTheUser /etc/sudoers && grep -q ThisIsTheUser /home/ThisIsTheUser/.ssh/authorized_keys && echo all-don}`\
\
`# Define passwords and prompt`\
`set initial_password "ThisIsTheFirstPW"`\
`set second_password "ThisIsTheSecondPW"`\
`set prompt "`$$.*\@.* .*$$`\$"`\
\
`# Spawn the SSH session`\
`spawn ssh $user@$server`\
`expect {`\
`   "(yes/no)? " {`\
`      send "yes\r"`\
`      expect {`\
`         "assword: " {`\
`            send "$initial_password\r"`\
`            interact -nobuffer -re "(.*)\r" return`\
`         }`\
`         "$prompt" {}`\
`      }`\
`   }`\
`   "assword: " {`\
`      send "$initial_password\r"`\
`      interact -nobuffer -re "(.*)\r" return`\
`   }`\
`   "$prompt" {}`\
`}`\
\
`# Press Enter key to proceed`\
`send "\r"`\
\
`# Check if sudo is required`\
`if { $sudo == "1" } {`\
`   send "su -\r"`\
`   expect {`\
`      "assword: " {`\
`         send "$second_password\r"`\
`         expect "$prompt"`\
`         send "$cmd1\r"`\
`         expect "$prompt"`\
`         send "$cmd2\r"`\
`         expect "$prompt"`\
`         send "$cmd3\r"`\
`         expect "$prompt"`\
`         send "$cmd4\r"`\
`         expect "$prompt"`\
`         send "$cmd5\r"`\
`         expect "$prompt"`\
`         send "$cmd6\r"`\
`         expect "$prompt"`\
`         send "$cmd7\r"`\
`         expect "$prompt"`\
`         send "$cmd8\r"`\
`         expect "$prompt"`\
`         send "$cmd9\r"`\
`         expect "$prompt"`\
`         send "$cmd10\r"`\
`         expect "$prompt"`\
`         send "$cmd11\r"`\
`         expect "$prompt"`\
`         send "$cmd12\r"`\
`         expect "$prompt"`\
`         send "$cmd13\r"`\
`         expect "$prompt"`\
`      }`\
`      "$prompt" {`\
`         send "$cmd1\r"`\
`         expect "$prompt"`\
`         send "$cmd2\r"`\
`         expect "$prompt"`\
`         send "$cmd3\r"`\
`         expect "$prompt"`\
`         send "$cmd4\r"`\
`         expect "$prompt"`\
`         send "$cmd5\r"`\
`         expect "$prompt"`\
`         send "$cmd6\r"`\
`         expect "$prompt"`\
`         send "$cmd7\r"`\
`         expect "$prompt"`\
`         send "$cmd8\r"`\
`         expect "$prompt"`\
`         send "$cmd9\r"`\
`         expect "$prompt"`\
`         send "$cmd10\r"`\
`         expect "$prompt"`\
`         send "$cmd11\r"`\
`         expect "$prompt"`\
`         send "$cmd12\r"`\
`         expect "$prompt"`\
`         send "$cmd13\r"`\
`         expect "$prompt"`\
`      }`\
`   }`\
`}`\
\
`# If sudo is not required`\
`if { $sudo == "" || $sudo == "0" } {`\
`   send "$cmd1\r"`\
`   expect "$prompt"`\
`   send "$cmd2\r"`\
`   expect "$prompt"`\
`   send "$cmd3\r"`\
`   expect "$prompt"`\
`   send "$cmd4\r"`\
`   expect "$prompt"`\
`   send "$cmd5\r"`\
`   expect "$prompt"`\
`   send "$cmd6\r"`\
`   expect "$prompt"`\
`   send "$cmd7\r"`\
`   expect "$prompt"`\
`   send "$cmd8\r"`\
`   expect "$prompt"`\
`   send "$cmd9\r"`\
`   expect "$prompt"`\
`   send "$cmd10\r"`\
`   expect "$prompt"`\
`   send "$cmd11\r"`\
`   expect "$prompt"`\
`   send "$cmd12\r"`\
`   expect "$prompt"`\
`   send "$cmd13\r"`\
`   expect "$prompt"`\
`}`\
\
`# Exit the session`\
`send "exit\r"`\
`send "exit\r"`\
\
`# Wait for the session to close`\
`expect eof`
