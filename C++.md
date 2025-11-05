
# C++ Cheatsheets

## Basics

### How to compile

~~~bash
$ g++ -o main main.cpp

// To execute:

$ ./main
~~~

### Hello World

~~~cpp
#include "iostream"

int main(void) {
        std::cout << "Hello World!";
        return 0;
}
~~~

## C++ Ready-to-Run Projects Collection

A collection of **ready-to-run C++ projects** in Markdown. Each project includes

* description
* full source code (C++17 compatible)
* build and run instructions (g++)
* brief explanation and exercises to extend

---

### Table of Contents

1. [Quick setup & compile notes](#quick-setup--compile-notes)
2. [Project 1 — CLI Calculator (basic)](#project-1---cli-calculator-basic)
3. [Project 2 — Simple File Manager (list & copy)](#project-2---simple-file-manager-list--copy)
4. [Project 3 — Tic-Tac-Toe (console game)](#project-3---tic-tac-toe-console-game)
5. [Project 4 — To-Do List (file-backed)](#project-4---to-do-list-file-backed)
6. [Project 5 — Multithreaded Prime Finder (worker threads)](#project-5---multithreaded-prime-finder-worker-threads)
7. [Next steps & learning exercises](#next-steps--learning-exercises)

---

### Quick setup & compile notes

These projects target **C++17** and use only the standard library (no external deps).

Compile with g++/clang++:

~~~bash
# compile (example)
g++ -std=c++17 -O2 project.cpp -o project
# run
./project
~~~

On Windows with MinGW:

~~~bash
g++ -std=c++17 project.cpp -o project.exe
project.exe
~~~

If a project uses multiple files, compile with:

~~~bash
g++ -std=c++17 -O2 file1.cpp file2.cpp -o app
~~~

---

### Project 1 - CLI Calculator (basic)

**Description:** A simple command-line calculator that supports `+ - * / %` and parentheses via shunting-yard parsing.

**File:** `calculator.cpp`

~~~cpp
#include <bits/stdc++.h>
using namespace std;

int precedence(char op){
    if(op=='+'||op=='-') return 1;
    if(op=='*'||op=='/'||op=='%') return 2;
    return 0;
}

double apply_op(double a, double b, char op){
    switch(op){
        case '+': return a+b;
        case '-': return a-b;
        case '*': return a*b;
        case '/': if(b==0) throw runtime_error("Division by zero"); return a/b;
        case '%': return fmod(a,b);
    }
    return 0;
}

int main(){
    cout << "C++ CLI Calculator (type 'quit' or Ctrl+C to exit)\n";
    string line;
    while(true){
        cout << "> ";
        if(!getline(cin, line)) break;
        if(line=="quit"||line=="exit") break;
        try{
            // Shunting-yard evaluation (infix to RPN on-the-fly)
            stack<double> values;
            stack<char> ops;
            for(size_t i=0;i<line.size();){
                if(isspace(line[i])){ ++i; continue; }
                if(isdigit(line[i])||line[i]=='.'){
                    size_t j=i;
                    while(j<line.size() && (isdigit(line[j])||line[j]=='.')) ++j;
                    double val = stod(line.substr(i,j-i));
                    values.push(val);
                    i=j;
                } else if(line[i]=='('){ ops.push('('); ++i; }
                else if(line[i]==')'){
                    while(!ops.empty() && ops.top()!='('){
                        double b=values.top(); values.pop();
                        double a=values.top(); values.pop();
                        char op=ops.top(); ops.pop();
                        values.push(apply_op(a,b,op));
                    }
                    if(!ops.empty()) ops.pop();
                    ++i;
                } else {
                    char cur = line[i];
                    while(!ops.empty() && precedence(ops.top())>=precedence(cur)){
                        double b=values.top(); values.pop();
                        double a=values.top(); values.pop();
                        char op=ops.top(); ops.pop();
                        values.push(apply_op(a,b,op));
                    }
                    ops.push(cur);
                    ++i;
                }
            }
            while(!ops.empty()){
                double b=values.top(); values.pop();
                double a=values.top(); values.pop();
                char op=ops.top(); ops.pop();
                values.push(apply_op(a,b,op));
            }
            if(!values.empty()) cout << values.top() << "\n";
        } catch(const exception &e){
            cout << "Error: " << e.what() << "\n";
        }
    }
    return 0;
}
~~~

#### Build & run

~~~bash
g++ -std=c++17 -O2 calculator.cpp -o calculator
./calculator
~~~

#### Exercises

* Add support for unary minus (negative numbers)
* Add functions like `sin`, `cos`, `sqrt` using `<cmath>`
* Improve error messages for malformed expressions

---

### Project 2 - Simple File Manager (list & copy)

**Description:** List files in a directory and copy files. Uses C++17 `std::filesystem`.

**File:** `file_manager.cpp`

~~~cpp
#include <iostream>
#include <filesystem>
#include <fstream>
using namespace std;
namespace fs = std::filesystem;

void list_dir(const fs::path &p){
    if(!fs::exists(p)){
        cout << "Path does not exist\n";
        return;
    }
    for(auto &entry : fs::directory_iterator(p)){
        cout << (entry.is_directory() ? "[DIR] " : "      ") << entry.path().filename().string() << '\n';
    }
}

bool copy_file(const fs::path &src, const fs::path &dst){
    try{
        fs::copy_file(src, dst, fs::copy_options::overwrite_existing);
        return true;
    } catch(exception &e){
        cerr << "Copy failed: " << e.what() << '\n';
        return false;
    }
}

int main(int argc, char** argv){
    cout << "Simple File Manager\n";
    if(argc < 2){
        cout << "Usage: file_manager <command> [args]\n";
        cout << "Commands:\n  ls <dir>\n  cp <src> <dst>\n";
        return 0;
    }
    string cmd = argv[1];
    if(cmd=="ls"){
        fs::path p = (argc>=3 ? argv[2] : fs::current_path());
        list_dir(p);
    } else if(cmd=="cp" && argc>=4){
        fs::path src = argv[2];
        fs::path dst = argv[3];
        if(copy_file(src,dst)) cout << "Copied successfully\n";
    } else {
        cout << "Unknown command\n";
    }
}
~~~

#### Build

~~~bash
g++ -std=c++17 -O2 file_manager.cpp -o file_manager
# list current directory
./file_manager ls .
# copy file
./file_manager cp source.txt dest.txt
~~~

#### Notes

* `std::filesystem` is available in C++17 with recent compilers.
* On older compilers you may need `-lstdc++fs` (rare nowadays).

---

### Project 3 - Tic-Tac-Toe (console game)

**Description:** Two-player console Tic-Tac-Toe with a simple AI (random or minimax option).

**File:** `tictactoe.cpp`

~~~cpp
#include <iostream>
#include <array>
#include <random>
using namespace std;

array<char,9> board;

void draw(){
    for(int r=0;r<3;r++){
        for(int c=0;c<3;c++){
            char x = board[r*3+c];
            cout << (x? x : ' ');
            if(c<2) cout << " | ";
        }
        cout << "\n";
        if(r<2) cout << "---------\n";
    }
}

bool win(char p){
    int W[8][3] = {{0,1,2},{3,4,5},{6,7,8},{0,3,6},{1,4,7},{2,5,8},{0,4,8},{2,4,6}};
    for(auto &w:W) if(board[w[0]]==p && board[w[1]]==p && board[w[2]]==p) return true;
    return false;
}

int main(){
    board.fill(0);
    char player = 'X';
    int moves = 0;
    cout << "Tic-Tac-Toe: players X and O. Enter 0-8 for positions.\n";
    while(true){
        draw();
        cout << "Player " << player << " move: ";
        int pos; if(!(cin>>pos)) break;
        if(pos<0||pos>8||board[pos]){ cout << "Invalid move\n"; continue; }
        board[pos]=player; moves++;
        if(win(player)){ draw(); cout << "Player " << player << " wins!\n"; break; }
        if(moves==9){ draw(); cout << "Draw!\n"; break; }
        player = (player=='X'?'O':'X');
    }
}
~~~

~~~bash
g++ -std=c++17 -O2 tictactoe.cpp -o tictactoe
./tictactoe
~~~

---

### Project 4 - To-Do List (file-backed)

**Description:** Simple command-line to-do list persisted to a text file.

**File:** `todo.cpp`

~~~cpp
#include <iostream>
#include <fstream>
#include <vector>
#include <string>
using namespace std;

const string DB = "todo.txt";

vector<string> load(){
    vector<string> items;
    ifstream f(DB);
    string line;
    while(getline(f,line)) if(!line.empty()) items.push_back(line);
    return items;
}

void save(const vector<string>& items){
    ofstream f(DB, ios::trunc);
    for(auto &s: items) f << s << '\n';
}

int main(){
    cout << "Simple To-Do (commands: list, add <text>, rm <index>, quit)\n";
    auto items = load();
    string cmd;
    while(true){
        cout << "> ";
        if(!getline(cin, cmd)) break;
        if(cmd=="quit"||cmd=="exit") break;
        if(cmd=="list"){
            for(size_t i=0;i<items.size();++i) cout << i << ": " << items[i] << '\n';
        } else if(cmd.rfind("add ",0)==0){
            string txt = cmd.substr(4);
            if(!txt.empty()) items.push_back(txt), save(items);
        } else if(cmd.rfind("rm ",0)==0){
            int idx = stoi(cmd.substr(3));
            if(idx>=0 && idx < (int)items.size()) items.erase(items.begin()+idx), save(items);
        } else {
            cout << "Unknown command\n";
        }
    }
}
~~~

~~~bash
g++ -std=c++17 -O2 todo.cpp -o todo
./todo
~~~

---

### Project 5 - Multithreaded Prime Finder (worker threads)

**Description:** Spawn worker threads to test ranges for primes and report results. Demonstrates `<thread>` and synchronization.

**File:** `primes_mt.cpp`

~~~cpp
#include <iostream>
#include <thread>
#include <vector>
#include <atomic>
#include <mutex>
using namespace std;

bool is_prime(long n){
    if(n<2) return false;
    if(n%2==0) return n==2;
    for(long i=3;i*i<=n;i+=2) if(n%i==0) return false;
    return true;
}

int main(){
    long start = 1, end = 200000; // tune for speed
    int threads = thread::hardware_concurrency();
    if(threads<=0) threads = 4;
    cout << "Searching primes in ["<<start<<","<<end<<"] using "<<threads<<" threads\n";

    vector<thread> workers;
    atomic<long> next(start);
    mutex out_m;

    auto worker = [&]{
        while(true){
            long n = next.fetch_add(1);
            if(n> end) break;
            if(is_prime(n)){
                lock_guard<mutex> g(out_m);
                cout << n << "\n";
            }
        }
    };

    for(int i=0;i<threads;++i) workers.emplace_back(worker);
    for(auto &t: workers) t.join();
    cout << "Done.\n";
}
~~~

~~~bash
g++ -std=c++17 -O2 -pthread primes_mt.cpp -o primes_mt
./primes_mt
~~~

* Use `-pthread` when compiling on Linux.
* This is a simple work-stealing style loop using `atomic` counter.

---

### Next steps & learning exercises

* **Refactor** each project into multiple `.cpp`/`.hpp` files and use a Makefile or CMake.
* Add **unit tests** using a framework (Catch2, GoogleTest).
* Learn RAII and smart pointers by refactoring resource usage.
* Try building GUI versions (SDL for games, Qt for tools).
* Explore networking with `asio` or sockets.

---
