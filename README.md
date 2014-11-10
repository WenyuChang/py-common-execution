Advanced Python Based Common Execution Utility
============
Features Supported

1. Commands Parsing
2. Commands Modify after initialized
3. Single Commands Executing
4. Multipy Commands Executing
5. Commands Ordering Change
6. Foreground commands execution
7. Background commands execution
8. Stdout/Stderr redirection
9. Commands execution timeout
10. Local Commands execution
11. Remote Commands execution
12. User Pre-Defined Commands

"Command Module" Detail Usage
============
```python
from Command import Command

command = Command('ls -al')
command = Command('ls')
command.add_argument('-a -l')
command.add_argument('-s')

command = Command('echo ${text}', {'text':'aaaaaa'})
command = Command('echo')
command.add_argument('${text}')
command.add_argument('${text1}')
command.set_substitution_dict({'text':'aaa'}, text1='bbb', text2='ccc')
print command
```

"LocalExec Module" Detail Usage
============
``` python
from LocalExecutor import LocalExec
from Modifier import FG, BG

# Initial local_exec
local_exec = LocalExec()
local_exec = LocalExec('ls -al')
local_exec = LocalExec(['ls -al', 'echo text'])
local_exec = LocalExec(command)
local_exec = LocalExec([command1, command2])

# Show commands and modify local_exec by adding/removing/slicing commands
print len(local_exec)
local_exec.show_commands()
local_exec.remove_command(command)
local_exec.insert_command_at(1, 'ls -al')
local_exec.insert_command_at(1, ['ls -al', 'echo text'])
local_exec.insert_command_at(1, command)
local_exec.insert_command_at(1, [command1, command2])
local_exec = local_exec1 + local_exec2 + local_exec3
local_exec += local_exec1
sub_local_exec = local_exec[1:3]
sub_local_exec = local_exec[2:-1]
sub_local_exec = local_exec[2:-1:2]
sub_single_local_exec = local_exec['ls', 1] # command_name, count

# Executing local_exec in foreground
local_exec()
local_exec('-al')
local_exec >> FG() # default print stdout to sys.stdout; stderr to sys.stderr
local_exec >> FG(out='out_file', err='err_file') # print out and err to file, mode is append
local_exec >> FG(out=None) # no stdout to screen
local_exec >> FG(err=None) # no stderr to screen
local_exec >> FG(out='out_file', err='err_file', append=False) # print out and err to file, mode is write
local_exec >> FG(..., timeout=10) # set timeout
local_exec >> FG(..., timeout=-1) # no timeout

# Execting local_exec in background
future = local_exec >> BG() # return Future list
future = local_exec >> BG(timeout=10)
future = local_exec >> BG(timeout=-1)
future.wait()
future.stdout()
future.stderr()
future.returncode()

# Local_exec internal usage:
local_exec(bg=True/False, out=..., err=..., timeout=...)
```


"RemoteExec Module" Detail Usage
============
```python
from RemoteExecutor import RemoteExec

# Connecting to remote machine
remote_exec = RemoteExec(host='...', user='...', port='...', keyfile='...', password='...', command=..., reference=another_exec)

# Operations are same as LocalExec
```



"User Pre-Defined Module" Detail Usage
============
1. put command files unser ./common_commands/
2. user defined command need implement 'execute' method

```python
exec = LocalExec()
exec = RemoteExec(...)

# Sample ls command Usage
exec.ls('-a')
exec.ls('-al')
exec.ls('-a -l')
exec.ls('-a', '-l')

# Sample tail Command Usage
exec.tail(file_name)

# Sample _cwd_ Command Usage
exec.cwd() # show current directory
with exec.cwd(path):
  exec...
```
