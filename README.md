[![Build Status](https://travis-ci.org/desta-study/lab006.svg?branch=master)](https://travis-ci.org/desta-study/lab006)
## Laboratory work VI

Данная лабораторная работа посвещена изучению фреймворков для тестирования на примере **Catch**

```ShellSession
$ open https://github.com/philsquared/Catch
```

## Tasks

- [ ] 1. Создать публичный репозиторий с названием **lab06** на сервисе **GitHub**
- [ ] 2. Выполнить инструкцию учебного материала
- [ ] 3. Ознакомиться со ссылками учебного материала
- [ ] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAME=<имя_пользователя>
$ alias gsed=sed # for *-nix system
```

```ShellSession
desta-study@HOME:~$ export GITHUB_USERNAME=desta-study
desta-study@HOME:~$ alias gsed=sed
desta-study@HOME:~$ cd ${GITHUB_USERNAME}/workspace
desta-study@HOME:~/desta-study/workspace$ pushd .
~/desta-study/workspace ~/desta-study/workspace
desta-study@HOME:~/desta-study/workspace$ source scripts/activate
desta-study@HOME:~/desta-study/workspace$ git clone https://github.com/${GITHUB_USERNAME}/lab05 projects/lab06
Cloning into 'projects/lab06'...
remote: Counting objects: 43, done.
remote: Compressing objects: 100% (27/27), done.
remote: Total 43 (delta 10), reused 35 (delta 9), pack-reused 0
Unpacking objects: 100% (43/43), done.
Checking connectivity... done.
desta-study@HOME:~/desta-study/workspace$ cd projects/lab06 # Переходим в каталог lab06
desta-study@HOME:~/desta-study/workspace/projects/lab06$ git remote remove origin #Удалить старый репозиторий
desta-study@HOME:~/desta-study/workspace/projects/lab06$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab06 # Добавляем новый удаленный репозиторий
desta-study@HOME:~/desta-study/workspace/projects/lab06$ mkdir tests
desta-study@HOME:~/desta-study/workspace/projects/lab06$ wget https://github.com/philsquared/Catch/releases/download/v1.9.3/catch.hpp -O tests/catch.hpp #Подключаем библиотеку для модульного тестирования на языке С++ catch.hpp
--2017-12-12 06:09:32--  https://github.com/philsquared/Catch/releases/download/v1.9.3/catch.hpp
Resolving github.com (github.com)... 192.30.253.112, 192.30.253.113
Connecting to github.com (github.com)|192.30.253.112|:443... connected.
HTTP request sent, awaiting response... 301 Moved Permanently
Location: https://github.com/catchorg/Catch2/releases/download/v1.9.3/catch.hpp [following]
--2017-12-12 06:09:32--  https://github.com/catchorg/Catch2/releases/download/v1.9.3/catch.hpp
Reusing existing connection to github.com:443.
HTTP request sent, awaiting response... 302 Found
Location: https://github-production-release-asset-2e65be.s3.amazonaws.com/1062572/61afa568-29c4-11e7-9755-a1211db07475?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20171212%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20171212T030933Z&X-Amz-Expires=300&X-Amz-Signature=778eebdeae4f28313fb723426da521ce4028ed92c3cf71f3f1060ac3f748fb0d&X-Amz-SignedHeaders=host&actor_id=0&response-content-disposition=attachment%3B%20filename%3Dcatch.hpp&response-content-type=application%2Foctet-stream [following]
--2017-12-12 06:09:33--  https://github-production-release-asset-2e65be.s3.amazonaws.com/1062572/61afa568-29c4-11e7-9755-a1211db07475?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20171212%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20171212T030933Z&X-Amz-Expires=300&X-Amz-Signature=778eebdeae4f28313fb723426da521ce4028ed92c3cf71f3f1060ac3f748fb0d&X-Amz-SignedHeaders=host&actor_id=0&response-content-disposition=attachment%3B%20filename%3Dcatch.hpp&response-content-type=application%2Foctet-stream
Resolving github-production-release-asset-2e65be.s3.amazonaws.com (github-production-release-asset-2e65be.s3.amazonaws.com)... 52.216.99.83
Connecting to github-production-release-asset-2e65be.s3.amazonaws.com (github-production-release-asset-2e65be.s3.amazonaws.com)|52.216.99.83|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 417675 (408K) [application/octet-stream]
Saving to: ‘tests/catch.hpp’

tests/catch.hpp                    100%[================================================================>] 407,89K   502KB/s    in 0,8s    

2017-12-12 06:09:34 (502 KB/s) - ‘tests/catch.hpp’ saved [417675/417675]

desta-study@HOME:~/desta-study/workspace/projects/lab06$ cat > tests/main.cpp <<EOF # Изменяем файл main.cpp
> #define CATCH_CONFIG_MAIN
> #include "catch.hpp"
> EOF
desta-study@HOME:~/desta-study/workspace/projects/lab06$ gsed -i '/option(BUILD_EXAMPLES "Build examples" OFF)/a\ # Записываем option(BUILD_TESTS "Build tests" OFF) в файл CMakeLists.txt
> option(BUILD_TESTS "Build tests" OFF)
> ' CMakeLists.txt
desta-study@HOME:~/desta-study/workspace/projects/lab06$ cat >> CMakeLists.txt <<EOF
> 
> if(BUILD_TESTS)
> enable_testing()
> file(GLOB \${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
> add_executable(check \${\${PROJECT_NAME}_TEST_SOURCES})
> target_link_libraries(check \${PROJECT_NAME} \${DEPENDS_LIBRARIES})
> add_test(NAME check COMMAND check "-s" "-r" "compact" "--use-colour" "yes") 
> endif()
> EOF
desta-study@HOME:~/desta-study/workspace/projects/lab06$ cat >> tests/test1.cpp <<EOF #Добавляем изменения в test1.cpp
> #include "catch.hpp"
> #include <print.hpp>
> 
> TEST_CASE("output values should match input values", "[file]") {
>   std::string text = "hello";
>   std::ofstream out("file.txt");
>   
>   print(text, out);
>   out.close();
>   
>   std::string result;
>   std::ifstream in("file.txt");
>   in >> result;
>   
>   REQUIRE(result == text);
> }
> EOF
desta-study@HOME:~/desta-study/workspace/projects/lab06$ cmake -H. -B_build -DBUILD_TESTS=ON # Собираем проект
-- The C compiler identification is GNU 5.4.0
-- The CXX compiler identification is GNU 5.4.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/desta-study/desta-study/workspace/projects/lab06/_build
desta-study@HOME:~/desta-study/workspace/projects/lab06$ cmake --build _build # Запускаем сборку в каталоге _build
Scanning dependencies of target print
[ 20%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[ 40%] Linking CXX static library libprint.a
[ 40%] Built target print
Scanning dependencies of target check
[ 60%] Building CXX object CMakeFiles/check.dir/tests/test1.cpp.o
[ 80%] Building CXX object CMakeFiles/check.dir/tests/main.cpp.o
[100%] Linking CXX executable check
[100%] Built target check
desta-study@HOME:~/desta-study/workspace/projects/lab06$ cmake --build _build --target test #Сборка test
Running tests...
Test project /home/desta-study/desta-study/workspace/projects/lab06/_build
    Start 1: check
1/1 Test #1: check ............................   Passed    0.00 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.00 sec
desta-study@HOME:~/desta-study/workspace/projects/lab06$ _build/check -s -r compact
/home/desta-study/desta-study/workspace/projects/lab06/tests/test1.cpp:15: passed: result == text for: "hello" == "hello"
Passed 1 test case with 1 assertion.

desta-study@HOME:~/desta-study/workspace/projects/lab06$ cmake --build _build --target test -- ARGS=--verbose
Running tests...
UpdateCTestConfiguration  from :/home/desta-study/desta-study/workspace/projects/lab06/_build/DartConfiguration.tcl
UpdateCTestConfiguration  from :/home/desta-study/desta-study/workspace/projects/lab06/_build/DartConfiguration.tcl
Test project /home/desta-study/desta-study/workspace/projects/lab06/_build
Constructing a list of tests
Done constructing a list of tests
Checking test dependency graph...
Checking test dependency graph end
test 1
    Start 1: check

1: Test command: /home/desta-study/desta-study/workspace/projects/lab06/_build/check "-s" "-r" "compact" "--use-colour" "yes"
1: Test timeout computed to be: 9.99988e+06
1: /home/desta-study/desta-study/workspace/projects/lab06/tests/test1.cpp:15: passed: result == text for: "hello" == "hello"
1: Passed 1 test case with 1 assertion.
1: 
1/1 Test #1: check ............................   Passed    0.00 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.00 sec
desta-study@HOME:~/desta-study/workspace/projects/lab06$ gsed -i 's/lab05/lab06/g' README.md # Добавляем изменения в файле README.md
desta-study@HOME:~/desta-study/workspace/projects/lab06$ gsed -i 's/\(DCMAKE_INSTALL_PREFIX=_install\)/\1 -DBUILD_TESTS=ON/' .travis.yml # Добавляем изменения в файле .travis.yml
desta-study@HOME:~/desta-study/workspace/projects/lab06$ gsed -i '/cmake --build _build --target install/a\
> - cmake --build _build --target test -- ARGS=--verbose
> ' .travis.yml
desta-study@HOME:~/desta-study/workspace/projects/lab06$ git add .
desta-study@HOME:~/desta-study/workspace/projects/lab06$ git commit -m"added tests"
[master e752916] added tests
 7 files changed, 11506 insertions(+), 2 deletions(-)
 create mode 100644 file.txt
 create mode 100644 tests/catch.hpp
 create mode 100644 tests/main.cpp
 create mode 100644 tests/test1.cpp
desta-study@HOME:~/desta-study/workspace/projects/lab06$ git push origin master
Username for 'https://github.com': desta-study
Password for 'https://desta-study@github.com': 
remote: Repository not found.
fatal: repository 'https://github.com/desta-study/lab06/' not found
desta-study@HOME:~/desta-study/workspace/projects/lab06$ git push origin master
Username for 'https://github.com': desta-study
Password for 'https://desta-study@github.com': 
To https://github.com/desta-study/lab06
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/desta-study/lab06'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
desta-study@HOME:~/desta-study/workspace/projects/lab06$ git pull origin master
warning: no common commits
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/desta-study/lab06
 * branch            master     -> FETCH_HEAD
 * [new branch]      master     -> origin/master
Merge made by the 'recursive' strategy.
desta-study@HOME:~/desta-study/workspace/projects/lab06$ git push origin master
Username for 'https://github.com': desta-study
Password for 'https://desta-study@github.com': 
Counting objects: 52, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (44/44), done.
Writing objects: 100% (52/52), 86.73 KiB | 0 bytes/s, done.
Total 52 (delta 13), reused 0 (delta 0)
remote: Resolving deltas: 100% (13/13), done.
To https://github.com/desta-study/lab06
   afc4a5b..cf462c3  master -> master
desta-study@HOME:~/desta-study/workspace/projects/lab06$ travis login --auto # Авторизация через GitHub
We need your GitHub login to identify you.
This information will not be sent to Travis CI, only to api.github.com.
The password will not be displayed.

Try running with --github-token or --auto if you don't want to enter your password anyway.

Username: desta-study
Password for desta-study: *************
Successfully logged in as desta-study!
desta-study@HOME:~/desta-study/workspace/projects/lab06$ travis enable # Включение проекта
Detected repository as desta-study/lab06, is this correct? |yes| yes
desta-study/lab06: enabled :)
desta-study@HOME:~/desta-study/workspace/projects/lab06$ mkdir artifacts #Создаем каталог
desta-study@HOME:~/desta-study/workspace/projects/lab06$ sleep 20s && gnome-screenshot --file artifacts/screenshot.png #Делаем скриншот экрана и кидаем в каталог artifacts
desta-study@HOME:~/desta-study/workspace/projects/lab06$ git push origin master
Username for 'https://github.com': desta-study
Password for 'https://desta-study@github.com': 
Everything up-to-date
desta-study@HOME:~/desta-study/workspace/projects/lab06$ git pull origin master
From https://github.com/desta-study/lab06
 * branch            master     -> FETCH_HEAD
Already up-to-date.
desta-study@HOME:~/desta-study/workspace/projects/lab06$ git push origin master
Username for 'https://github.com': desta-study
Password for 'https://desta-study@github.com': 
Everything up-to-date
desta-study@HOME:~/desta-study/workspace/projects/lab06$ git pull origin master
From https://github.com/desta-study/lab06
 * branch            master     -> FETCH_HEAD
Already up-to-date.
desta-study@HOME:~/desta-study/workspace/projects/lab06$ git push origin master
Username for 'https://github.com': desta-study
Password for 'https://desta-study@github.com': 
Everything up-to-date
desta-study@HOME:~/desta-study/workspace/projects/lab06$ git add .
desta-study@HOME:~/desta-study/workspace/projects/lab06$ git commit -m"EZ"
[master 3acfd07] EZ
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 artifacts/screenshot.png
desta-study@HOME:~/desta-study/workspace/projects/lab06$ git pull origin master
From https://github.com/desta-study/lab06
 * branch            master     -> FETCH_HEAD
Already up-to-date.
desta-study@HOME:~/desta-study/workspace/projects/lab06$ git push origin master
Username for 'https://github.com': desta-study
Password for 'https://desta-study@github.com': 
Counting objects: 4, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (4/4), 366.77 KiB | 0 bytes/s, done.
Total 4 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/desta-study/lab06
   cf462c3..3acfd07  master -> master
desta-study@HOME:~/desta-study/workspace/projects/lab06$ 

```

## Links

- [Boost.Tests](http://www.boost.org/doc/libs/1_63_0/libs/test/doc/html/)
- [Google Test](https://github.com/google/googletest)

```
Copyright (c) 2017 Братья Вершинины
```
