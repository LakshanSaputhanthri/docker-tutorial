# Lesson 2: Linux

## Steps

1. Start Docker.

2. Run an Ubuntu image:
   ```bash
   docker run ubuntu
   ```

3. Show running containers:
   ```bash
   docker ps
   ```

4. Show all containers:
   ```bash
   docker ps -a
   ```

5. Run an Ubuntu image interactively:
   ```bash
   docker run -it ubuntu
   ```
   "Interactively" (the `-it` flags) means the container gives you a live shell you can type into, instead of just running one command and exiting.

   - `-i` (interactive) keeps **STDIN** open so you can send input. STDIN (standard input) is the input stream a program reads from — normally your keyboard. When a program's STDIN is open, you can type things and the program receives them; when it's closed, the program can't receive any input even if you type.
   - `-t` (tty) allocates a pseudo-**TTY** so you get a proper terminal prompt, with formatting, arrow keys, etc. A TTY (teletypewriter, historically) means a terminal/console device. A pseudo-terminal is a virtual one Docker creates for the container, so a program inside behaves as if it's connected to a real terminal.

   Both flags are needed for a normal, usable interactive shell:
   - Without `-i`, STDIN is closed — even if bash starts, you couldn't type anything to it.
   - Without `-t`, there's no TTY — bash might run, but you'd get no proper prompt/formatting, and things like arrow-key history wouldn't work.

   Together, `docker run -it ubuntu` drops you into a bash shell inside the Ubuntu container, where you can run commands like `ls`, `pwd`, `cat /etc/os-release`, etc., just like you would in a real terminal — until you type `exit`.

   Compare that to plain `docker run ubuntu` (no `-it`): Ubuntu's default command isn't a long-running process, so the container starts, has nothing to do, and immediately exits.

6. Run the following commands and see the output:
   ```bash
   echo hello
   whoami
   echo $0
   history
   pwd
   ls
   ls -l
   ls /home
   cd ~
   cd ..
   ```

7. Update apt:
   ```bash
   apt update
   ```

8. Install nano:
   ```bash
   apt install nano
   ```

9. Uninstall nano:
   ```bash
   apt remove nano
   ```

10. Manipulate files and directories:
    - Create/edit a file with nano:
      ```bash
      nano file.txt
      ```
    - View a file's contents:
      ```bash
      cat file.txt
      ```
    - Install `less` and use it to page through a file:
      ```bash
      apt install less
      less file.txt
      ```
    - Redirect a command's output into a file with `>` (overwrites the file):
      ```bash
      cat file.txt > file1.txt
      echo whatever > whatever.txt
      ls -l /etc > file.txt
      ```
    - Concatenate multiple files into one:
      ```bash
      cat file.txt file1.txt > combined.txt
      ```
    - List files, including hidden ones, and long-format listings:
      ```bash
      ls -a
      ls -l /etc
      ```
    - Navigate directories:
      ```bash
      cd ~
      cd ..
      cd etc
      ```

11. Environment variables:
    - View all environment variables, or a single one:
      ```bash
      printenv
      printenv DB_USER
      echo $DB_USER
      ```
    - Persist a variable by adding it to `~/.bashrc`, then reload it into the current shell:
      ```bash
      echo DB_TEST=testenv2 >> .bashrc
      source .bashrc
      ```
      A new shell reads `.bashrc` automatically; `source` re-reads it in the shell you already have open, without needing to exit and reconnect.

12. Processes:
    - List running processes, and start one in the background with `&`:
      ```bash
      ps
      sleep 1000 &
      ps
      ```
    - Stop a process by its PID:
      ```bash
      kill <pid>
      ```

13. User management:
    - Create a user (`-m` also creates their home directory), and view all users:
      ```bash
      useradd -m john
      cat /etc/passwd
      ```
    - Change a user's login shell:
      ```bash
      usermod -s /bin/bash john
      ```
    - Delete a user:
      ```bash
      userdel john
      ```


