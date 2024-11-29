# Programming Ideas #1
A idea suggested by @brunozhon, programming ranks (not professional but could be)
## Absolute Beginner
#### Print "Your Name"
name = "YourNameHere"
print(f"Hello, {name}")
## Beginner Max
#### Count to ten
for i in range(1, 11):
    print(i)
## Ultra Beginner
#### To do list app
tasks = []

while True:
    print("\nTo-Do List:")
    for i, task in enumerate(tasks, start=1):
        print(f"{i}. {task}")
    
    print("\nOptions: [1] Add Task  [2] Remove Task  [3] Exit")
    choice = input("Choose an option: ")

    if choice == "1":
        task = input("Enter a task: ")
        tasks.append(task)
    elif choice == "2":
        task_num = int(input("Enter task number to remove: "))
        if 1 <= task_num <= len(tasks):
            tasks.pop(task_num - 1)
    elif choice == "3":
        break
    else:
        print("Invalid choice!")
## Pro
#### Tic-Tac-Toe!
board = [' ' for _ in range(9)]

def print_board():
    for i in range(0, 9, 3):
        print(f"{board[i]} | {board[i+1]} | {board[i+2]}")
        if i < 6:
            print("--+---+--")

def check_winner():
    wins = [(0, 1, 2), (3, 4, 5), (6, 7, 8),
            (0, 3, 6), (1, 4, 7), (2, 5, 8),
            (0, 4, 8), (2, 4, 6)]
    for a, b, c in wins:
        if board[a] == board[b] == board[c] != ' ':
            return True
    return False

player = 'X'
for turn in range(9):
    print_board()
    move = int(input(f"Player {player}, choose a spot (1-9): ")) - 1
    if board[move] == ' ':
        board[move] = player
        if check_winner():
            print_board()
            print(f"Player {player} wins!")
            break
        player = 'O' if player == 'X' else 'X'
    else:
        print("Spot taken, try again!")
else:
    print("It's a draw!")
## Expierienced Pro
#### Maze maker/solver
import random

def generate_maze(size):
    maze = [[random.choice([' ', '#']) for _ in range(size)] for _ in range(size)]
    maze[0][0] = 'S'  # Start
    maze[size-1][size-1] = 'E'  # End
    return maze

def print_maze(maze):
    for row in maze:
        print("".join(row))

def solve_maze(maze, x, y, path):
    if x < 0 or y < 0 or x >= len(maze) or y >= len(maze) or maze[x][y] in ('#', '*'):
        return False
    if maze[x][y] == 'E':
        path.append((x, y))
        return True
    
    maze[x][y] = '*'
    path.append((x, y))

    if (solve_maze(maze, x+1, y, path) or
        solve_maze(maze, x-1, y, path) or
        solve_maze(maze, x, y+1, path) or
        solve_maze(maze, x, y-1, path)):
        return True

    path.pop()
    return False

##### Generate and solve a maze
size = 5
maze = generate_maze(size)
print("Generated Maze:")
print_maze(maze)

path = []
if solve_maze(maze, 0, 0, path):
    print("\nMaze Solved! Path:", path)
else:
    print("\nNo solution found.")
## Hacker
#### Custom Language
def evaluate(command):
    # Your implementation of SimpleLang should go here.
    return "Not implemented"

def interpret(command):
    try:
        print(evaluate(command))
    except Exception as e:
        print(f"Error: {e}")

print("SimpleLang Interpreter. Type 'exit' to quit.")
while True:
    cmd = input(">> ")
    if cmd.lower() == "exit":
        break
    interpret(cmd)
## Pro Hacker
#### Multi-Player Online Game!

import socket
import threading

clients = []

def broadcast(message, sender_socket):
    for client in clients:
        if client != sender_socket:
            try:
                client.send(message)
            except:
                clients.remove(client)

def handle_client(client_socket):
    while True:
        try:
            message = client_socket.recv(1024)
            broadcast(message, client_socket)
        except:
            clients.remove(client_socket)
            client_socket.close()
            break

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(("0.0.0.0", 5555))
server.listen()

print("Server is running...")
while True:
    client_socket, client_address = server.accept()
    print(f"Connection from {client_address}")
    clients.append(client_socket)
    threading.Thread(target=handle_client, args=(client_socket,)).start()
##### file: client.py
#
##### Handles the client for the game.

import socket
import threading

def receive_messages(client_socket):
    while True:
        try:
            message = client_socket.recv(1024).decode("utf-8")
            print(message)
        except:
            print("Disconnected from server.")
            break

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(("127.0.0.1", 5555))

threading.Thread(target=receive_messages, args=(client,)).start()

print("Connected to server. Type your messages below.")
while True:
    message = input()
    client.send(message.encode("utf-8"))
## God Of Development (GOD)
#### Minimal Operating System Kernal
[BITS 16]
[ORG 0x7C00]

start:
    mov ax, 0x07C0
    add ax, 288
    mov ss, ax
    mov sp, 4096

    mov si, msg
    call print_string

    mov bx, 0x1000  ; Load kernel to memory location 0x1000
    mov ah, 0x02
    mov al, 1
    mov ch, 0
    mov dh, 0
    mov cl, 2
    int 0x13

    jmp 0x1000

print_string:
    mov ah, 0x0E
.repeat:
    lodsb
    cmp al, 0
    je .done
    int 0x10
    jmp .repeat
.done:
    ret

msg db "Booting the OS...", 0

times 510 - ($-$$) db 0
dw 0xAA55
// file: kernel.c
//
// The kernel for the operating system. Process management and memory allocation should be written here (ideally).

void print_string(char* str) {
    unsigned short* video_memory = (unsigned short*)0xB8000;
    while (*str) {
        *video_memory++ = (*str++ | 0x0F00); // White text on black background
    }
}

void main() {
    print_string("Hello, OS World!");
    while (1); // Infinite loop to keep the OS running
}
##### Build the os using these commands
nasm -f bin boot.asm -o boot.bin
gcc -ffreestanding -m32 -c kernel.c -o kernel.o
ld -m elf_i386 -Ttext 0x1000 --oformat binary kernel.o -o kernel.bin
cat boot.bin kernel.bin > os_image.bin
qemu-system-x86_64 -drive format=raw,file=os_image.bin
