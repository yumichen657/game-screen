<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <title>兆算玉璃 - 簡易範例</title>
  <style>
    body {
      background-color: #222;
      color: white;
      text-align: center;
      font-family: sans-serif;
    }
    h1 {
      margin-top: 20px;
    }
    .grid {
      display: grid;
      grid-template-columns: repeat(3, 120px);
      grid-gap: 10px;
      justify-content: center;
      margin-top: 40px;
    }
    .hex {
      width: 120px;
      height: 104px;
      background-color: #444;
      clip-path: polygon(
        50% 0%, 93% 25%, 93% 75%,
        50% 100%, 7% 75%, 7% 25%
      );
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 36px;
      cursor: pointer;
      transition: 0.3s;
    }
    .hex:hover {
      background-color: #666;
    }
    .empty {
      background-color: transparent;
      cursor: default;
    }
  </style>
</head>
<body>

<h1>兆算玉璃</h1>
<p>點擊兩個相鄰的模組來交換</p>

<div class="grid" id="grid"></div>

<script>
  // 初始棋盤：null 表示空白
  let board = [
    null, '🟩', null,
    '🟥', '🟦', '🟨',
    null, '🟪', null
  ];

  const goal = [...board]; // 目標形狀

  let selected = [];

  function render() {
    const grid = document.getElementById('grid');
    grid.innerHTML = '';

    board.forEach((item, index) => {
      const div = document.createElement('div');
      div.className = 'hex';
      if (item === null) {
        div.classList.add('empty');
      } else {
        div.innerText = item;
        div.addEventListener('click', () => select(index));
      }
      grid.appendChild(div);
    });
  }

  function select(index) {
    selected.push(index);
    if (selected.length === 2) {
      const [i, j] = selected;
      if (isAdjacent(i, j)) {
        [board[i], board[j]] = [board[j], board[i]];
        render();
        if (checkWin()) {
          setTimeout(() => alert('✅ 完成！恭喜！'), 100);
        }
      }
      selected = [];
    }
  }

  function isAdjacent(i, j) {
    const adjacent = {
      1: [3, 4],
      3: [1, 4, 7],
      4: [1, 3, 5, 7],
      5: [4, 7],
      7: [3, 4, 5, 8],
      8: [7]
    };
    return adjacent[i]?.includes(j);
  }

  function checkWin() {
    return JSON.stringify(board) === JSON.stringify(goal);
  }

  render();
</script>

</body>
</html>
