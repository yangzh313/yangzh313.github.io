title:: c++ Shell: Calling shell commands from C++

- > 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [dev.to](https://dev.to/aggsol/calling-shell-commands-from-c-8ej)
- > I like programming in Bash because it allows you to access a wide range of powerful tools like grep,.......
- I like programming in Bash because it allows you to access a wide range of powerful tools like grep, sed, awk, jq or wget just to name a few. The toolbox is filled with great tools. But Bash scripts tend to get _complicated_ with a certain size and complexity.
- For a more complex task I tend to C++ whenever I can. But there you start from a clean slate. Getting the features that e. g. grep or sed provide into your C++ program can be difficult. There are great libraries like libcurl and many functions are in the standard library. So you can get feature parity with a bit of work.
- But sometime you feel like a Bash one liner is nearly impossible to realize in C++. Why not call that one liner from C++ and get done with it? You might feel dirty doing so but there is no need to. There is popen(3) for a good reason! It helps you to get things done.
- The popen() function wraps the creation of a pipe, forking a child process and calling a shell. In the most basic usage you want to execute a command and receive the result as a string. You want a function like this `std::string executeCommand(const std::string& cmd)`. Let's have a look at an easy popen() wrapper.
- ```
  std::string execCommand(const std::string cmd, int& out_exitStatus)
  {
    out_exitStatus = 0;
    auto pPipe = ::popen(cmd.c_str(), "r");
    if(pPipe == nullptr)
    {
        throw std::runtime_error("Cannot open pipe");
    }
  - std::array<char, 256> buffer;
  - std::string result;
  - while(not std::feof(pPipe))
    {
        auto bytes = std::fread(buffer.data(), 1, buffer.size(), pPipe);
        result.append(buffer.data(), bytes);
    }
  - auto rc = ::pclose(pPipe);
  - if(WIFEXITED(rc))
    {
        out_exitStatus = WEXITSTATUS(rc);
    }
  - return result;
  }
  - ```
  - Not only do you want the result but also the exit status. Commands can fail for various reasons. Most tools also have different exit codes for errors. This function works for many basic commands. You would use it like this.
  - ```
  int exitStatus = 0;
  auto result = execCommand("grep --version", exitStatus);
  - ```
  - But there are two things you cannot do. You cannot write to the stdin of the called command and you cannot read stderr. For those to be accessible you need to create the pipes for all three yourself and also do the forking.
  - ```
  class Command
  {
  public:
    int             ExitStatus = 0;
    std::string     Command;    
    std::string     StdIn;
    std::string     StdOut;
    std::string     StdErr;
  - void execute()
    {
        const int READ_END = 0;
        const int WRITE_END = 1;
  - int infd[2] = {0, 0};
        int outfd[2] = {0, 0};
        int errfd[2] = {0, 0};
  - auto cleanup = [&]() {
            ::close(infd[READ_END]);
            ::close(infd[WRITE_END]);
  - ::close(outfd[READ_END]);
            ::close(outfd[WRITE_END]);
  - ::close(errfd[READ_END]);
            ::close(errfd[WRITE_END]);  
        };
  - auto rc = ::pipe(infd);
        if(rc < 0)
        {
            throw std::runtime_error(std::strerror(errno));
        }
  - rc = ::pipe(outfd);
        if(rc < 0)
        {
            ::close(infd[READ_END]);
            ::close(infd[WRITE_END]);
            throw std::runtime_error(std::strerror(errno));
        }
  - rc = ::pipe(errfd);
        if(rc < 0)
        {
            ::close(infd[READ_END]);
            ::close(infd[WRITE_END]);
  - ::close(outfd[READ_END]);
            ::close(outfd[WRITE_END]);
            throw std::runtime_error(std::strerror(errno));
        }
  - auto pid = fork();
        if(pid > 0) // PARENT
        {
            ::close(infd[READ_END]);    // Parent does not read from stdin
            ::close(outfd[WRITE_END]);  // Parent does not write to stdout
            ::close(errfd[WRITE_END]);  // Parent does not write to stderr
  - if(::write(infd[WRITE_END], StdIn.data(), StdIn.size()) < 0)
            {
                throw std::runtime_error(std::strerror(errno));
            }
            ::close(infd[WRITE_END]); // Done writing
        }
        else if(pid == 0) // CHILD
        {
            ::dup2(infd[READ_END], STDIN_FILENO);
            ::dup2(outfd[WRITE_END], STDOUT_FILENO);
            ::dup2(errfd[WRITE_END], STDERR_FILENO);
  - ::close(infd[WRITE_END]);   // Child does not write to stdin
            ::close(outfd[READ_END]);   // Child does not read from stdout
            ::close(errfd[READ_END]);   // Child does not read from stderr
  - ::execl("/bin/bash", "bash", "-c", Command.c_str(), nullptr);
            ::exit(EXIT_SUCCESS);
        }
  - // PARENT
        if(pid < 0)
        {
            cleanup();
            throw std::runtime_error("Failed to fork");
        }
  - int status = 0;
        ::waitpid(pid, &status, 0);
  - std::array<char, 256> buffer;
  - ssize_t bytes = 0;
        do
        {
            bytes = ::read(outfd[READ_END], buffer.data(), buffer.size());
            StdOut.append(buffer.data(), bytes);
        }
        while(bytes > 0);
  - do
        {
            bytes = ::read(errfd[READ_END], buffer.data(), buffer.size());
            StdErr.append(buffer.data(), bytes);
        }
        while(bytes > 0);
  - if(WIFEXITED(status))
        {
            ExitStatus = WEXITSTATUS(status);
        }
  - cleanup();
    }
  };
  - ```
  - Keep in mind a pipe is unidirectional so you need two file descriptors for each stream (stdin, stdout, stderr). But these details are wrapped away and you can easily execute a shell command like this.
  ```
    Command cmd;
    cmd.Command = "grep diam - ";
    cmd.StdIn = R"(
   Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua.    
   )";
- cmd.execute();
- std::cout << cmd.StdOut;
    std::cerr << cmd.StdErr;
    std::cout << "Exit Status: " << cmd.ExitStatus << "\n";
- ```
- It is easier than expected and works well. If something can be done in Bash then it can be done in C++, too!
- **Update**
- As Vlad pointed out to me the order of processing after `fork()` could be either way. So the parent could be first. The coded reflects that now as the parent now reads the pipes after a wait.