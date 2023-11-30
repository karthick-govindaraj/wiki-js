---
title: IDE
description: server debbuging
published: true
date: 2023-11-30T05:30:41.742Z
tags: ide
editor: markdown
dateCreated: 2023-11-30T05:30:37.811Z
---

 

## 1. Configuration

1.1 **Install C/C++ Debug Extension in VS Code** 

- Launch VS Code. 
- Go to the Extensions tab or use the shortcut (Ctrl+Shift+X). 
- Search for “C/C++" by Microsoft and install it. 

1**.2 Create launch.json Configuration** 

- Select **Run** from the top menu. 
- Click "Add Configuration" to create launch.json. 
- Choose **C/C++: (gdb) Launch** configuration. 
- Replace the content of launch.json with the provided configuration. 
     {
      "version": "0.2.0",
      "configurations": [
        {
          "name": "C/C++: Remote",
          "type": "cppdbg",
          "request": "launch",
          "program": "${workspaceFolder}/${fileBasenameNoExtension}",
          "cwd": "${workspaceFolder}",
          "stopAtEntry": true,
          "MIMode": "gdb",
          "miDebuggerPath": "/opt/riscv/bin/riscv64-unknown-linux-gnu-gdb",
          "miDebuggerServerAddress": "<ip address>:2000"
        }
      ]
    }
![](https://paper-attachments.dropboxusercontent.com/s_A5AA06F2108B46B6B1F3BC89F857D615A08D2FB7368ADEFD4E5CC36732ADD4C2_1693377717062_file.png)


1**.3 Cross Compilation Using tasks.json** 

- Select Terminal -> Configure Tasks. 
- Create a new tasks.json file from the template. 

Replace the content of tasks.json with the provided configuration. 

     {
      "version": "2.0.0",
      "tasks": [
        {
          "label": "C: Build",
          "type": "shell",
          "command": "/opt/riscv/bin/riscv64-unknown-linux-gnu-gcc",
          "args": [
            "-g",
            "${file}",
            "-o",
            "${fileDirname}/${fileBasenameNoExtension}"
          ],
          "group": {
            "kind": "build"
          }
        }
      ]
    }

 

![](https://paper-attachments.dropboxusercontent.com/s_A5AA06F2108B46B6B1F3BC89F857D615A08D2FB7368ADEFD4E5CC36732ADD4C2_1693377717062_file.png)



## 2. Building and Remote Debugging 
    2**.1 Build and Copy Executable to RISC-V Board** 
    - Build the project by pressing **Ctrl+Shift+B** or clicking Terminal -> Run Build Task. 
    - The binary file will be generated in the same directory as the source file. 
    - Copy the executable to the RISC-V board using SCP. 
                $ scp executable-file user@<ip address>:~/. 


    2.2  **Start GDB Server on RISC-V Board** 
    - Connect to the RISC-V board via SSH. 
      $ ssh user@<ip address>
    - Start the GDB server using the provided command. 
    $ gdbserver localhost:2000 executable-file 


    2.3 **Start Debugging in VS Code** 
    - Press F5 or click Run -> Start Debugging. 
    - Set breakpoints in VS Code. 
    - The output will be displayed in the RISC-V board terminal. 
    2.4  **Check Disassembly** 
    - Right-click to view the disassembly of the code using the "Open Disassembly View" option in the VS Code editor. 


# Python
## 1. Configuration 

1.1 **Install Python Extension in VS Code** 

- Launch VS Code. 
- Go to the Extensions tab or use the shortcut (Ctrl+Shift+X). 
- Search for “Python" by Microsoft and install it. 

1**.2 Create launch.json Configuration** 

- Select **Run** from the top menu. 
- Click "Add Configuration" to create launch.json. 
- Choose **Python** configuration. 
- Replace the content of launch.json with the provided configuration. 
     {
      "version": "0.2.0",
      "configurations": [
        {
          "name": "Python: Remote",
          "type": "python",
          "request": "attach",
          "port": 3000,
          "host": "<ip address>",
          "pathMappings": [
            {
              "localRoot": "${workspaceFolder}",
              "remoteRoot": "/home/user/example/Python"
            }
          ]
        }
      ]
    }
![](https://paper-attachments.dropboxusercontent.com/s_A5AA06F2108B46B6B1F3BC89F857D615A08D2FB7368ADEFD4E5CC36732ADD4C2_1693377701659_image.png)

##  2. Remote Debugging 

2.1 **Connect to the RISC-V Board via SSH** 

- Establish an SSH connection to the RISC-V board: 
    $ ssh user@<ip address>

**2.2 Install debugpy** 

- Install the **debugpy** library on the RISC-V board: 
    $ sudo pip3 install debugpy 

**2.3 Start the debugpy Server** 

- Start the debugpy server on the RISC-V board: 
    $ python3 -m debugpy --wait-for-client --listen 0.0.0.0:3000 executable-file.py 

2**.4 Start Debugging in VS Code** 

- Press F5 or click Run -> Start Debugging in VS Code. 
- Set breakpoints within VS Code. 
- The output will be displayed in the RISC-V board terminal and the **DEBUG CONSOLE** in VS Code. 
# Node.js
## 1. Configuration 

 **1.1 Create launch.json Configuration** 

- Select **Run** from the top menu. 
- Click "Add Configuration" to create launch.json. 
- Choose **Node.js: Attach** configuration. 
- Replace the content of launch.json with the provided configuration. 
     {
      "version": "0.2.0",
      "configurations": [
        {
          "name": "Node: Remote",
          "type": "node",
          "request": "attach",
          "port": 4000,
          "address": "192.168.4.26",
          "localRoot": "${workspaceFolder}",
          "remoteRoot": "/home/user/example/Node"
        }
      ]
    }
![](https://paper-attachments.dropboxusercontent.com/s_A5AA06F2108B46B6B1F3BC89F857D615A08D2FB7368ADEFD4E5CC36732ADD4C2_1693378538228_image.png)



- Update the **remoteRoot** path with the appropriate Node.js file path on the board. 
## 2. Remote Debugging 

 **2.1 Connect to the RISC-V Board** 

- Connect to the RISC-V board via SSH using the provided command. 
    $ ssh user@<ip address>

**2.2 Start the Node.js Server** 

- Start the Node.js server using the provided command. 
- The **--inspect-brk** flag enables debugging. 
    $ node --inspect-brk=0.0.0.0:4000 executable-file.js 

2**.3 Start Debugging in VS Code** 

- Press F5 or click Run -> Start Debugging. 
- Set breakpoints in VS Code. 
- Observe the output in the RISC-V board terminal and the **DEBUG CONSOLE** in VS Code. 




----------
----------




# ↓  Jupyter Notebook


## 1. Installation 

1.1 **Switch to Super User Mode** 

    $ sudo su 

 
1**.2 Install rustc and cargo** 

- Download the **rustup-init** script and grant execute permissions: 


    $ wget -O rustup-init.sh https://sh.rustup.rs 
    $ chmod +x rustup-init.sh 
    $ ./rustup-init.sh 

 

- Update the shell environment to include Cargo: 
    $ source "$HOME/.cargo/env"         

 
**1.3 Check Rustc and Cargo Version** 

    $ rustc --version 
    $ cargo --version 

 
1**.4 Install Jupyter Notebook Using pip** 

    $ sudo pip install notebook 


## 2. Running Jupyter Notebook

2.1 **Start Jupyter Notebook** 

    $ jupyter notebook --ip 0.0.0.0 --allow-root 

 
2**.2 Access Jupyter Notebook from Host Machine** 

- After starting Jupyter Notebook, a listening URL will be displayed. You can access it from your host machine by changing the URL IP to the RISC-V board's IP. 


## 3. Debugging 

  3.1 **Launching Debugger in JupyterLab** 

- Access JupyterLab by clicking the JupyterLab link in the top right corner. 
- Click the debugger icon to activate the debugger mode. 

**3.2 Setting Breakpoints** 

- Place breakpoints by clicking on the left side of line numbers in a Python cell. 

**3.3 Executing the Program** 

- Click the run icon on the status bar to execute the program. 
- The program will pause at the breakpoint. 

**3.4 Debugging Tools** 

- A panel on the right provides debugging tools for tracking variables, call stacks, breakpoints, and source code. 



# ↓  Eclipse CDT


| Tab 1                                                                                                                | Tab 2                                                                                                    | Tab 3 |
| -------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- | ----- |
| [C/C++](https://paper.dropbox.com/doc/Temp-dev-content-8JCKtPZ9Eb97AyvNt1W7R#:uid=253772822090068393684423&h2=C/C++) | [](https://paper.dropbox.com/doc/Temp-dev-8JCKtPZ9Eb97AyvNt1W7R#:uid=474831878829229537386713&h2=Python) |       |



# C/C++
## 1. **Installation Steps**

**1.1. Install Eclipse IDE on Ubuntu**

- Download the Eclipse binary from the following link: [Eclipse IDE for C/C++ Developers](https://www.eclipse.org/downloads/packages/release)
- Extract the Eclipse tar file:
    $ tar -xzvf eclipse-inst-jre-linux64.tar.gz
- Install Eclipse:
    $ cd eclipse-installer/
    $ ./eclipse-inst

**1.2. Install Eclipse IDE for Embedded C/C++**

- Click on **Eclipse IDE for Embedded C/C++ Developers** and complete the installation process.
- Provide an installation folder or use the default settings and click the Install button.
- After installation, launch the Eclipse IDE.

**1.3. Create a New Embedded C/C++ Project**

- After the Eclipse Welcome dashboard loads, click on **Create a new Embedded C/C++ project**.
- Choose **C Managed Build**, provide the project name, and change the Toolchain to "RISC-V Cross GCC." Click "Next."
- Select all the configurations and click **Next**.
- Set the Toolchain name as **RISC-V GCC/Linux (riscv64-unknown-linux-gnu-gcc)**.
- Ensure that the gcc cross-compiler is available. If not, follow the Cross Compile C/C++ steps to install the toolchain to a specific folder and update the PATH as mentioned below.
- Update the Toolchain name (e.g., **/opt/riscv/bin**) and click **Finish**.
- The workspace will be created.

1**.4. Create a C File**

- Right-click the project folder in Eclipse.
- Navigate to **New -> File -> Create the "<file name>.c"** file.
- Paste a simple "Hello, World!" program into the "<file name>.c" file:

**1.5. Compile the C File**

- Click the **Hammer icon** below the main menu or Click **Project → Build Project** to compile the "<file name>.c" file.

1**.6. Copy the Executable File to StarFive Board Example**

    $ scp /home/aximsoft/eclipse-workspace/hello/Debug/<file name>.elf user@<ip address>:~/

**1.7. Install GDBserver on RISC-V Board**

- Update the package manager's cache:
    $ sudo apt update


- Install GDBserver:
    $ sudo apt install gdbserver


- Verify the installation:
    $ gdbserver --version

**1.8. Start GDBserver on RISC-V Board**

- Start the GDB server:
    $ gdbserver localhost:5000 hel <file name> lo.elf

1**.9. Change Debug Configuration in Eclipse**

- Go to the top menu bar in Eclipse.
- Click Run -> Select **Debug Configurations.**
- Update settings:
    - Double-click the **C/C++ Remote Application**.
    - Add a debug name.
    - Update the executable file path in the C/C++ Application field (executable (PATH: 'Debug/hello.elf').
    - Click the check box **Disable auto build**.
    - Click the checkbox **Use configuration specific settings** and
    - Change the debug launcher to **GDB (DSF) Manual Remote Debugging Launcher**.
- Move to the **Debugger** tab in the popup:
    - Add the GDB debugger path **riscv64-unknown-linux-gnu-gdb**.
    - Go to the **Connection** sub-tab of the debugger.
    - Add the host address and required port number where the GDB server is running:
            Example: `Host name or IP address : 192.168.4.12 Port number : 5000
    - Apply the changes.
- Click debug.
    - A model pop-up Confirm Perspective Switch will be open and Click the switch button.
- Debugging is started to connect to the GDB server on the RISC-V board.
- You should be able to debug and view the output on the RISC-V board.




## GDB GUI

**gdbgui** is a web-based graphical interface for the GNU Debugger (**gdb**) that simplifies the process of debugging C programs


| Tab 1                                                                                                                                                                                                                               | Tab 2                                                                                                    | Tab 3 |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- | ----- |
| [](https://paper.dropbox.com/doc/Temp-dev-content-8JCKtPZ9Eb97AyvNt1W7R#:uid=253772822090068393684423&h2=C/C++)[C/C++](https://paper.dropbox.com/doc/Temp-dev-content-8JCKtPZ9Eb97AyvNt1W7R#:uid=302300561367064064471301&h2=C/C++) | [](https://paper.dropbox.com/doc/Temp-dev-8JCKtPZ9Eb97AyvNt1W7R#:uid=474831878829229537386713&h2=Python) |       |

# C/C++ 
## 1. Installation Steps

1.1**. Install gdbgui on RISC-V Board**

- Install the necessary dependencies and **gdbgui**:
    $ sudo apt-get install gdb python3
    $ sudo pip install gdbgui

**1.2. Compile the Program**

- Compile the program with debugging information using **-g** flag:


    $ gcc -g <file name>.c -o <file name>

**1.3. Start gdbgui**

- In the terminal, run the following command to start **gdbgui**:
    $ gdbgui -r <file name>


- This will start the service at the host IP, typically **http://*****<ip address>*****:5000**.
- Open a web browser on your local machine and enter the URL to access **gdbgui**, where the respective program will be loaded.
## 2. Debugging with gdbgui

**2.1.Set Breakpoints and Start Debugging:**

- Click on the line number in the code editor where you want to set a breakpoint.
- After placing the required breakpoint, start the program by typing **r** or **run** command in the **gdb** terminal. This will start the debugged program.
- Use the buttons on the top right corner of the **gdbgui** interface to navigate through the program's execution step-by-step, such as continue, pause, restart, step over, etc.

2**.2. Additional Features in gdbgui:**

- You can check the **disassembly** of the code by clicking on the top menu bar and selecting **Fetch Disassembly**. This allows you to debug at the assembly level.
- **gdbgui** allows you to track local variables, memory, expressions, and registers in the right panel.
