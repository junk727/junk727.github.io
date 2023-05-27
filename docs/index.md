# Mkdocs에 온 것을 환영! 

For full documentation visit [mkdocs.org](https://www.mkdocs.org).

## Commands

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.

### 한국어 응애

한국어 예시일까 아닐까?

## 코드
```cpp
tuple<int,int,int> eGCD(int a, int b)
{
    if(!b) return {a,1,0};
    auto [g,x,y]=eGCD(b,a%b);
    return {g,y,x-a/b * y};
}

int inverseMod(int a, int m)
{
    auto [g,x,_] = eGCD(a,m);
    if(g!=1) return -1;
    return x 
    //or return (x+m)%m;
}
```