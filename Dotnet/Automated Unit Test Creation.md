
# Automatically create c# unit test code with Ollama

~~~bash
# MyCode.cs: The code for which we need unit tests.
$ cat MyCode.cs | ollama run qwen3-coder "Referencing the file, create unit tests with 100% code coverage" > tests.cs
~~~
