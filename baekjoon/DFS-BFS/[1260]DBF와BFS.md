https://www.acmicpc.net/problem/1541

## [문제설명]

그래프를 DFS로 탐색한 결과와 BFS로 탐색한 결과를 출력하는 프로그램을 작성하시오. 단, 방문할 수 있는 정점이 여러 개인 경우에는 정점 번호가 작은 것을 먼저 방문하고, 더 이상 방문할 수 있는 점이 없는 경우 종료한다. 정점 번호는 1번부터 N번까지이다.


## [입력]
첫째 줄에 정점의 개수 N(1 ≤ N ≤ 1,000), 간선의 개수 M(1 ≤ M ≤ 10,000), 탐색을 시작할 정점의 번호 V가 주어진다. 다음 M개의 줄에는 간선이 연결하는 두 정점의 번호가 주어진다. 어떤 두 정점 사이에 여러 개의 간선이 있을 수 있다. 입력으로 주어지는 간선은 양방향이다.



## [출력]
첫째 줄에 DFS를 수행한 결과를, 그 다음 줄에는 BFS를 수행한 결과를 출력한다. V부터 방문된 점을 순서대로 출력하면 된다.




```js
const fs = require("fs");
const input = fs.readFileSync("/dev/stdin").toString().trim().split("\n");


const start = Number(input[0].split(" ")[2]);


const temp = [];
for (let i = 1; i < input.length; i++) {
  const left = Number(input[i].split(" ")[0]);
  const right = Number(input[i].split(" ")[1]);
  if (temp[left]) temp[left] = [...temp[left], right];
  else temp[left] = [right];
  if (temp[right]) temp[right] = [...temp[right], left];
  else temp[right] = [left];
}

const nodeList = temp.map((el) => el.sort((a, b) => a - b));

// DFS
const dfsResult = [];
const dfs = (vertex) => {
  if (dfsResult.includes(vertex)) return;
  dfsResult.push(vertex);
  // 정점과 연결된 정점이 있는 경우만 수행
  if (nodeList[vertex])
    nodeList[vertex].forEach((el) => {
      dfs(el);
    });
};
dfs(start);

// BFS
const bfsResult = [];
const check = [];
const queue = [start];
const bfs = () => {
  const vertex = queue.shift();
  if (check[vertex]) return;
  if (bfsResult.includes(vertex)) return;
  check[vertex] = true;
  bfsResult.push(vertex);
  if (nodeList[vertex])
    nodeList[vertex].forEach((el) => {
      queue.push(el);
    });
};
while (queue.length) {
  bfs(start);
}

console.log(...dfsResult);
console.log(...bfsResult);


}
```
