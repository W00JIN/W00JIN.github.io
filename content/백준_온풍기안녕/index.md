---
emoji: ๐ฆ
title: ๋ฐฑ์ค 23289 ์จํ๊ธฐ ์๋!
date: '2022-05-12 23:47:00'
author: ์ฐ์ง
tags: ์ฝ๋ฉํ์คํธ
categories: Problem_Solving
---

<br/>

### [๋ฐฑ์ค 23289 ์จํ๊ธฐ ์๋!](https://www.acmicpc.net/problem/23289)

<br/>

### ๐ฅย ๊ตฌํ ๊ณผ์ 
1) ์จํ๊ธฐ, ๋๊ฐ์ , ๋ฒฝ ๋ฐฉํฅ ์ค์ 
2) ์๋ ฅ๊ฐ์ ๋ฐ๋ฅธ ์จํ๊ธฐ ๋ฐฉํฅ, ๋ฒฝ ๋ฐฉํฅ ์ ์ฅ
3) ์จํ๊ธฐ ์๋ ๊ตฌํ
4) ์จ๋ ์กฐ์ 
5) ์จ๋ ์ฒดํฌ - ์ข๋ฃ์กฐ๊ฑด

<br/>

### ๐ฅย 0) ๋ฌธ์  ๋ถ์

![์ฌ์ง](./2328915.png)

<br/>

`Cell` ์ ์ ์ฅ๋๋ ์ ๋ณด๋ฅผ ๊ตฌ์กฐ์ฒด๋ก ๊ตฌํํ๋ค. 

* ์ฃผ์ด์ง๋ ์ ๋ณด : ์จํ๊ธฐ ์ ๋ฌด์ ๋ฐฉํฅ, ์จ๋, ๋ฒฝ

<br/>

`heater` : ์จํ๊ธฐ ๋ฐฉํฅ (0์ด๋ฉด ์จํ๊ธฐ๊ฐ ์๋ ์นธ)

`wall` : ํ ์นธ์ ๊ธฐ์ค์ผ๋ก ์, ํ, ์ข, ์ฐ์ ๋ฒฝ์ ์ ์ฅ (๐ฅ ์ค๋ณตํด์ ๋ฒฝ ์กด์ฌ ๊ฐ๋ฅํ๊ธฐ ๋๋ฌธ์ vector๋ก ๊ตฌํ)

`degree` : ํด๋น ์นธ์ ์จ๋

<br/>

์จํ๊ธฐ์ ํผ์ ธ๋๊ฐ๋ ๋ฐ๋์ `queue`๋ฅผ ์ฌ์ฉํ์ฌ ๋ฐ๋์ด ์ด๋ํ  ์นธ์ ์ ์ฅ, ์นธ๋ง๋ค ์ธ๋ฒ์ฉ ํ์ํ๋ค.

์์ฐจ์ ์ผ๋ก ํผ์ ธ๋๊ฐ๋ฉฐ 3๋ฒ ํ์ํด์ผ ํ๊ธฐ ๋๋ฌธ์ ์ ์์ ์ถ ์๋ฃ๊ตฌ์กฐ์ธ `queue`๋ฅผ ์ฌ์ฉ.

์จํ๊ธฐ์ ๋ฐ๋์ด ํผ์ ธ๋๊ฐ ๋ ์จ๋๊ฐ 5๋ถํฐ 1๊น์ง๋ก ๊ฐ์ํ๋ฉฐ ์ด๋ํ๊ธฐ ๋๋ฌธ์ 

depth๋ฅผ ์ ์ฅํ๊ธฐ ์ํ `Wind` ๊ตฌ์กฐ์ฒด๋ ์ ์ธํ๋ค.

```java
struct Cell{
    int heater=0;
    vector<int> wall;
    int degree=0;
};

struct Wind{
    int x;
    int y;
    int depth;
};
```

<br/><br/>



### ๐ฅย 1) ์จํ๊ธฐ, ๋๊ฐ์ , ๋ฒฝ ๋ฐฉํฅ ์ค์ 

๋ฌธ์ ์์ ์ฃผ์ด์ง๋ `์จํ๊ธฐ ๋ฐฉํฅ`์ ๋ค์๊ณผ ๊ฐ๋ค.

1: ๋ฐฉํฅ์ด ์ค๋ฅธ์ชฝ

2: ๋ฐฉํฅ์ด ์ผ์ชฝ

3: ๋ฐฉํฅ์ด ์

4: ๋ฐฉํฅ์ด ์๋

<br/>

๋ฐ๋ผ์ ์ฃผ์ด์ง๋ ๋ฐฉํฅ์ ๋ง์ถ์ด x,y์ขํ๋ฅผ ์์ง์ด๋ dx, dy ๋ฐฐ์ด์ ์ค์ ํ๋ ค๊ณ  ํ๋ค. 

> int dx[5] = { 0, 0, 0, -1, 1}, dy[5] = { 0, 1, -1, 0, 0};

> ์์ - heater ๊ธฐ์ค  : ์ฐ ์ข ์ ํ

<br/><br/>

๋ค์์ `๋ฒฝ` ๋ฐฉํฅ์ ์ค์ ํ๋ ค๊ณ  ํ๋ค.

๋ฒฝ์ ๋ ์ ์ฌ์ด์ ์กด์ฌํ๋ฏ๋ก, ๋ฒฝ ํ๋๋ฅผ ์๋ ฅ๋ฐ์ผ๋ฉด `์์ชฝ ์์ ๋ฒฝ ์ ๋ณด๋ฅผ ์ ์ฅ`ํด์ผ ํ๋ค.

<br/>

![์ฌ์ง](./2328916.png)

<br/>

์จํ๊ธฐ ๋ฐ๋๊ณผ ๋ฒฝ์ ๋ฐฉํฅ์ด ๋ฐ๋์ธ ๊ฒฝ์ฐ, ํด๋น Cell์ ๋ฐ๋์ด ์ด๋ํ์ง ์๋๋ค.


![์ฌ์ง](./2328912.png)

<br/>

๋ฐ๋ผ์ `์จํ๊ธฐ ๋ฐฉํฅ`๊ณผ `๋ฒฝ ๋ฐฉํฅ`์ ๋ฐ๋ ๋ฐฉํฅ์ผ๋ก ๊ตฌํํ๋ค.

โ ํ ์์์ `์จํ๊ธฐ ๋ฐฉํฅ`, `๋ฒฝ ๋ฐฉํฅ` ๊ฐ์ด ๊ฐ์ผ๋ฉด ๋ฐ๋์ ์ํฅ์ ๋ฐ์ง ์์ผ๋ฏ๋ก ๊ฒ์ฆ๋ถ ๊ตฌํ์ด ์ฌ์์ง๋ค.

<br/>

```java
if(cell.heater == cell.wall) return false;

```


> wall   : 1-์ข 2-์ฐ 3-ํ 4-์ 

> heater : 1-์ฐ 2-์ข 3-์ 4-ํ

<br/><br/>


์จํ๊ธฐ๋ ์ถ๋ฐ ์ง์ ์ ๊ธฐ์ค์ผ๋ก ์ผ๊ฐํ ๊ผด๋ก ํผ์ ธ๋๊ฐ๋ค.

`์จํ๊ธฐ๊ฐ ํผ์ง๋ ๋ฐฉํฅ`์ ์ค์ ํ๊ธฐ ์ํด `windDx`, `windDy`์ 2์ฐจ์ ๋ฐฐ์ด๋ก ์ค์ ํ๋ค. 

๊ฐ ํ์ ์จํ๊ธฐ ๋ฐ๋๊ณผ ๋์ผํ๊ณ , ์ด์ ํผ์ ธ๋๊ฐ๋ ๋ฐฉํฅ์ ์๋ฏธํ๋ค.

์จํ๊ธฐ ๋ฐฉํฅ์ด `์ฐ์ธก`์ด ๋ผ๋ฉด `๋๊ฐ์  ์`, `์ฐ์ธก`, `๋๊ฐ์  ํ` ๋ฐฉํฅ์ผ๋ก ์จํ๊ธฐ๊ฐ ํผ์ ธ๋๊ฐ๋ค.

![์ฌ์ง](./2328917.png)

<br/>


๋ฐ๋์ด ๋๊ฐ์ ์ผ๋ก ์ด๋ํ๋ ๊ฒฝ์ฐ๋ ๋๊ฐ์ ๋ฒฝ์ ์ฒดํฌํด์ผํ๋ค.

`์ด๋ํ ์ขํ์์ ์ฒดํฌํด์ผํ๋ ๋ฒฝ`์ ๊ธฐ์กด๊ณผ ๋์ผํ๊ฒ ์จํ๊ธฐ ๋ฐ๋ ๋ฐฉํฅ๊ณผ ๋ฐ๋์ธ์ง ์ฒดํฌํ๋ค.

`๊ธฐ์ค ์ขํ์์ ์ฒดํฌํด์ผํ๋ ๋ฒฝ`์ ํ์ธํ๊ธฐ ์ํด `wallChk`์ 2์ฐจ์ ๋ฐฐ์ด๋ก ์ค์ ํ๋ค. 


<br/>

![์ฌ์ง](./2328914.png)

<br/><br/>


์จํ๊ธฐ ์งํ ๋ฐฉํฅ๋ณ๋ก ํผ์ ธ๋๊ฐ๋ ์ขํ๋ฅผ `windDx`, `windDy`์ ์ ์ฅํ๋ค.

์จํ๊ธฐ ๋ฐฉํฅ์ด `์ฐ` ๋ผ๋ฉด `๋๊ฐ์  ์`, `์ฐ`, `๋๊ฐ์  ํ` ๋ฐฉํฅ์ผ๋ก ์จํ๊ธฐ๊ฐ ํผ์ ธ๋๊ฐ๋ค.


<br/>

![์ฌ์ง](./2328913.png)

<br/>

> int windDx[5][3] = {{0,0,0},{-1,0,1},{-1,0,1},{-1,-1,-1},{1,1,1}}; //์์ - heater ๊ธฐ์ค  : ์ฐ ์ข ์ ํ

> int windDy[5][3] = {{0,0,0},{1,1,1},{-1,-1,-1},{-1,0,1},{-1,0,1}}; //์์ - heater ๊ธฐ์ค  : ์ฐ ์ข ์ ํ

> int wallChk[5][3] = {{0,0,0},{4,2,3},{4,1,3},{1,4,2},{1,3,2}}; //์์ - heater ๊ธฐ์ค  : ์ฐ ์ข ์ ํ

<br/><br/>


### ๐ฅย 2) ์๋ ฅ๊ฐ์ ๋ฐ๋ฅธ ์จํ๊ธฐ ๋ฐฉํฅ, ๋ฒฝ ๋ฐฉํฅ ์ ์ฅ

<br/>

์ฐ๋ฆฌ๋ Cell๊ตฌ์กฐ์ฒด๋ฅผ ์ฌ์ฉํด map์ ์จํ๊ธฐ๋ฐฉํฅ, ์จ๋, ๋ฒฝ ์ ๋ณด๋ฅผ ์ ์ฅํ๋ ค๊ณ  ํ๋ค.

์ด๋ ์จํ๊ธฐ๋ฅผ ๋์์ํค๊ธฐ ์ํด์๋ ์จํ๊ธฐ์ ์์น๋ฅผ ์๊ณ ์์ด์ผ ํ๋ฏ๋ก

Cell ๊ตฌ์กฐ์ฒด์ ์จํ๊ธฐ ๋ฐฉํฅ๊ณผ๋ ๋ณ๊ฐ๋ก `heaters` ๋ฒกํฐ์ ๋ฆฌ์คํธ๋ฅผ ์ ์ฅํ๋ค.

์ข๋ฃ ์กฐ๊ฑด ์ฒดํฌ ์ง์ ์ `checkList` ๋ฒกํฐ์ ๋ฆฌ์คํธ๋ฅผ ์ ์ฅํ๋ค.

์ฃผ์ด์ง๋ ๋ฒฝ(๊ฐ๋ก, ์ธ๋ก)์ ๋ฐ๋ผ ์จํ๊ธฐ ๋ฐฉํฅ๊ณผ ๋ฐ๋๋ฐฉํฅ์ ์ ์ฅํ๋ค.


```java

//์ข๋ฃ์กฐ๊ฑด ์ฒดํฌ์ง์ , ์จํ๊ธฐ ์์น ์ ์ฅ
if(cell == 5) checkList.push_back({i,j});
else if (cell > 0) {
                heaters.push_back({i,j});
                map[i][j].heater = cell;
}

//๋ฒฝ ์ ์ฅ - ๋ฒฝ์ด map ๊ฒฝ๊ณ์ ๋์ผํ ๊ฒฝ์ฐ ๋ฐฉ์ด๋ก์ง ์ถ๊ฐ
if(dir == 0){
    if(x-1>=0) map[x-1][y].wall.push_back(3);
    map[x][y].wall.push_back(4);
}else if(dir == 1){
    if(y+1<m) map[x][y+1].wall.push_back(1);
    map[x][y].wall.push_back(2);
}

```
<br/><br/>

### ๐ฅย 3) ์จํ๊ธฐ ์๋ ๊ตฌํ

<br/>

๋์ผํ ์จํ๊ธฐ๋ ๋ฐ๋์ด ์ค๋ณต์ผ๋ก ๋ค์ด์๋ ํ๋ฒ๋ง ์ฒดํฌ๋์ง๋ง,

๋ค๋ฅธ ์จํ๊ธฐ์๋ ์ค๋ณต ์์ธ ์ฒ๋ฆฌํ์ง ์๊ณ , ์ค๋ณตํด์ ๋ํด์ผ ํ๋ค.

๋ฐ๋ผ์ ์จํ๊ธฐ๋ง๋ค `map`์ ๋ณต์ฌ๋ณธ์ธ `windMap`์ ํ ๋นํ์ฌ ์ค๋ณต ์ฒ๋ฆฌ๋ฅผ ๊ตฌํํ๋ค.


```java
vector<vector<Cell>> windMap;
windMap.assign(map.begin(), map.end());

```

<br/>

<br/>

์จํ๊ธฐ ์๋์ ๊ตฌํํ๊ธฐ ์ํด queue< Wind >๋ฅผ ์ ์ธํด ์จํ๊ธฐ ์์์ ์ ๋ฃ๊ณ , ์์ depth๋ 4๋ก ์ค์ ํ๋ค.

for๋ฌธ์ 3๋ฒ ๋๋ฉฐ ๊ฐ Cell๋ง๋ค ์ธ๋ฒ ํผ์ ธ๋๊ฐ๋๋ก bfs๋ฅผ ๊ตฌํํ๋ค.

<br/>

์ข๋ฃ์กฐ๊ฑด์ `isExist`ํจ์๋ก map ๋ฒ์๋ฅผ ๋ฒ์ด๋๋์ง ์ฒดํฌํ๊ณ ,

`canGo`ํจ์๋ฅผ ํตํด์๋ ๋ฐ๋์ด ์ด๋ํ  ์ ์๋ ์นธ์ธ์ง ๊ฒ์ฆํ๋ค.

๊ฒ์ฆ ๋จ๊ณ์์ ๋ฐ๋์ ์ด๋ ๊ฐ๋ฅ ์ฌ๋ถ๋ ๋ถ์ ๋จ๊ณ์์ ์ค์ ํ ๊ธฐ์ค์ ์ ์ฉํ๋ค.

* `๊ธฐ์ค ์ขํ` : ๋๊ฐ์ , ์งํ๋ฐฉํฅ์ ๋ฒฝ ํ์ธ
* `์ด๋ํ  ์ขํ` : ์งํ๋ฐฉํฅ์ ๋ฒฝ ํ์ธ

<br/>

![์ฌ์ง](./2328914.png)

<br/>

`canGo`ํจ์์์๋ Cell์ ์ขํ์ ๋ฒฝ ๋ฐฉํฅ์ ๋ฐ์, Cell์ Wall ๋ฆฌ์คํธ์ ๋์ผํ ๋ฒฝ์ด ์๋์ง ์ฒดํฌํ๋ค.

`dir`์ ์จํ๊ธฐ์ ์งํ๋ฐฉํฅ์ ์๋ฏธํ๋ค.



```java
//map ๋ฒ์ ์ฒดํฌ ํจ์
bool isExist(int x, int y){
    if(x>=0 && x<n && y>=0 && y<m) return true;
    return false;
}

//๋ฒฝ ํ์ธ ํจ์
bool canGo(int x, int y, int wall){
    for(int i=0; i<map[x][y].wall.size(); i++){
        if(map[x][y].wall[i] == wall) return false;
    }
    return true;
}

//bfs ๊ตฌํ
while(!windQueue.empty()){
    int x = windQueue.front().x;
    int y = windQueue.front().y;
    int depth = windQueue.front().depth;
    
    windQueue.pop();
    
    for(int j=0; j<3; j++){
        int nx = x + windDx[dir][j];
        int ny = y + windDy[dir][j];
        
        if(isExist(nx, ny) && canGo(x,y,wallChk[dir][j]) && canGo(nx,ny,dir)){
            if(windMap[nx][ny].degree == map[nx][ny].degree) windMap[nx][ny].degree += depth;
            if(depth-1 > 0) windQueue.push({nx,ny,depth-1});
        }
    }
}

```
<br/><br/>

### ๐ฅย 4) ์จ๋ ์กฐ์ 

<br/>

์จ๋์กฐ์ ์ ๐ฅ๋์์๐ฅ ์ผ์ด๋๊ธฐ ๋๋ฌธ์ ์์ฐจ์ ์ผ๋ก map์ ๋ฐ์ํ์ฌ ์ด์  ๋ฐ์ ๋ฐ์ดํฐ๊ฐ ๋์ค ๋ฐ์ดํฐ์ ์ํฅ์ ๋ผ์น๋ฉด ์๋๋ค.

์จํ๊ธฐ์ ์ค๋ณต ์ ๊ฑฐ ์ฒ๋ผ ์๋ก์ด map ๋ณต์ฌ๋ณธ controllMap์ ์ ์ธํด ์จ๋๋ฅผ ์กฐ์ ํ๊ณ  ์จ๋์กฐ์ ์ ๋ชจ๋ ๋ง์น๊ณ  map์ ๋ฐ์ํ๋ค.

์จ๋์กฐ์ ์ Cell๋ณ๋ก ๋ฒฝ์ด ์๋ ์, ํ, ์ข, ์ฐ ์ธ์ ์นธ์ ๋น๊ตํ๋ค.

๊ธฐ์ค Cell๋ณด๋ค ์ธ์ ์นธ์ ์จ๋๊ฐ ๋์ ๊ฒฝ์ฐ, ๋์๊ฒ + ๋๋ฏ๋ก `(์ธ์ ์นธ - ๊ธฐ์ค ์นธ)/4`๋ฅผ ๋ํด์ค๋ค.

์จ๋์กฐ์ ์ ๋ง์น ํ map frame์ ์์นํ Cell ์จ๋๋ฅผ 1์ฉ ๊ฐ์์ํฌ ๋ 

๐ฅ(0,0), (R,0), (0,C), (R,C)๋ฅผ `์ค๋ณตํด์ ๋นผ์ง ์๋๋ก` ์ฃผ์ํ๋ค๐ฅ

```java
//์จ๋์กฐ์ 
void controllDegree(){
    
    vector<vector<Cell>> controllMap;
    controllMap.assign(map.begin(), map.end());
    for(int i=0; i<n; i++){
        for(int j=0; j<m; j++){
            int sum = 0;
            
            for(int dir = 1; dir<=4; dir++){
                int nx = i + dx[dir];
                int ny = j + dy[dir];
                
                if(isExist(nx, ny) && canGo(nx,ny,dir)) sum+= (map[nx][ny].degree - map[i][j].degree)/4;
                
            }
            controllMap[i][j].degree += sum;
        }
    }
    map.swap(controllMap);
}
//map frame ์จ๋๋ 1์ฉ ๊ฐ์
void minusFrame(){
    for(int i=0; i<n; i++){
        for(int j=0; j<m; j++){
            if(i==0||i==n-1||j==0||j==m-1)
                if(map[i][j].degree>0) map[i][j].degree--;
        }
    }
}
```

<br/><br/>


### ๐ฅย 5) ์จ๋ ์ฒดํฌ - ์ข๋ฃ์กฐ๊ฑด

<br/>

์ข๋ฃ ์กฐ๊ฑด์ ๋ชจ๋  ๊ตฌํ ์ค ๊ฐ์ฅ ์ฌ์ด ํํธ์ด๋ค.

๋ชจ๋  ์จ๋ ์กฐ์ ์ด ๋๋ ํ, ์ฃผ์ด์ง ์ขํ์ ์จ๋๊ฐ K ์ด์์ธ์ง ํ์ธํ๋ค.

```java
bool isFinished(){
    for(int i=0; i<searchList.size(); i++){
        int x = searchList[i].first;
        int y = searchList[i].second;
        if(map[x][y].degree < k) return false;
    }
    return true;
}
```

<br/>

### ๐ฅย ์ต์ข ์์ค


```java

#include <iostream>
#include <vector>
#include <queue>

using namespace std;

struct Cell{
    int heater=0;
    vector<int> wall;
    int degree=0;
};
struct Wind{
    int x;
    int y;
    int depth;
};

vector<vector<Cell>> map(21,vector<Cell>(21));
vector<pair<int,int>> heaters;
vector<pair<int,int>> checkList;
int n, m,K, answer = 0;
int dx[5] = {0,0,0,-1,1}, dy[5]={0,1,-1,0,0}; //์์ - heater ๊ธฐ์ค  : ์ฐ ์ข ์ ํ
int windDx[5][3] = {{0,0,0},{-1,0,1},{-1,0,1},{-1,-1,-1},{1,1,1}}; //์์ - heater ๊ธฐ์ค  : ์ฐ ์ข ์ ํ
int windDy[5][3] = {{0,0,0},{1,1,1},{-1,-1,-1},{-1,0,1},{-1,0,1}}; //์์ - heater ๊ธฐ์ค  : ์ฐ ์ข ์ ํ

int wallChk[5][3] = {{0,0,0},{4,2,3},{4,1,3},{1,4,2},{1,3,2}}; //์์ - heater ๊ธฐ์ค  : ์ฐ ์ข ์ ํ
// heater : ์ฐ1,์ข2,์3,ํ4
// wall   : ์ข1,์ฐ2,ํ3,์4

void input(){
    cin >> n >> m >> K;
    int cell;
    for(int i=0; i<n; i++){
        for(int j=0; j<m; j++){
            cin >> cell;
            if(cell == 5) checkList.push_back({i,j});
            else if (cell > 0) {
                heaters.push_back({i,j});
                map[i][j].heater = cell;
            }
        }
    }
    int wallCnt;
    cin >> wallCnt;
    for(int i=0; i<wallCnt; i++){
        int x, y, dir;
        cin >> x >> y >> dir;
        x--;
        y--;
        if(dir == 0){
            if(x-1>=0) map[x-1][y].wall.push_back(3);
            map[x][y].wall.push_back(4);
        }else if(dir == 1){
            if(y+1<m) map[x][y+1].wall.push_back(1);
            map[x][y].wall.push_back(2);
        }
    }
}
bool isExist(int x, int y){
    if(x>=0 && x<n && y>=0 && y<m) return true;
    return false;
}

bool canGo(int x, int y, int wall){
    for(int i=0; i<map[x][y].wall.size(); i++){
        if(map[x][y].wall[i] == wall) return false;
    }
    return true;
}

void runHeater(){
    
    for(int i=0; i<heaters.size(); i++){
        vector<vector<Cell>> windMap;
        windMap.assign(map.begin(), map.end());
        
        
        int hx = heaters[i].first;
        int hy = heaters[i].second;
        int dir = map[hx][hy].heater;
        queue<Wind> windQueue;
        
        int fx = hx + dx[dir];
        int fy = hy + dy[dir];
        
        
        windMap[fx][fy].degree += 5;
        
        
        windQueue.push({fx,fy,4});
        
        while(!windQueue.empty()){
            int x = windQueue.front().x;
            int y = windQueue.front().y;
            int depth = windQueue.front().depth;
            
            windQueue.pop();
            
            for(int j=0; j<3; j++){
                int nx = x + windDx[dir][j];
                int ny = y + windDy[dir][j];
                
                if(isExist(nx, ny) && canGo(x,y,wallChk[dir][j]) && canGo(nx,ny,dir)){
                    if(windMap[nx][ny].degree == map[nx][ny].degree) windMap[nx][ny].degree += depth;
                    if(depth-1 > 0) windQueue.push({nx,ny,depth-1});
                }
            }
        }
        map.swap(windMap);
    }
}

void controllDegree(){
    
    vector<vector<Cell>> controllMap;
    controllMap.assign(map.begin(), map.end());
    for(int i=0; i<n; i++){
        for(int j=0; j<m; j++){
            int sum = 0;
            
            for(int dir = 1; dir<=4; dir++){
                int nx = i + dx[dir];
                int ny = j + dy[dir];
                
                if(isExist(nx, ny) && canGo(nx,ny,dir)) sum+= (map[nx][ny].degree - map[i][j].degree)/4;
                
            }
            controllMap[i][j].degree += sum;
        }
    }
    map.swap(controllMap);
}

void minusFrame(){
    for(int i=0; i<n; i++){
        for(int j=0; j<m; j++){
            if(i==0||i==n-1||j==0||j==m-1)
                if(map[i][j].degree>0) map[i][j].degree--;
        }
    }
}

bool checkFinished(){
    for(int i=0; i<checkList.size(); i++){
        int x = checkList[i].first;
        int y = checkList[i].second;
        
        if(map[x][y].degree < K) return false;
    }
    return true;
}

void solve(){
    bool isFinished = false;
    while(!isFinished && answer<=100){
        runHeater();
        controllDegree();
        minusFrame();
        answer++;
        isFinished = checkFinished();
    }
    cout << answer;
}

int main(){
    input();
    solve();
}

```

<br/>

---