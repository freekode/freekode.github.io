---
layout: post
title: Game of Life
---

Some time ago there was a task for the new job, task make my own implementation the Game of Life. And I have done it.
Hotkeys:
<ul>
<li>Space = Play/Pause</li>
<li> - = Decrease speed</li>
<li> + = Increase speed</li>
</ul>

<button onclick="pauseStop()">pause/stop</button>
<canvas id="game-canvas" width="960" height="760"></canvas>
<script>
var canvasSelector = '#game-canvas';

var deadCellColour = 'rgb(0, 0, 0)';
var liveCellColour = 'rgb(50, 205, 50)';

var canvas = {};
var context = {};

// size of one cell in pixels
var cellSize = 4;

// interval between generations
var time = 50;

// start amount of cells by x and y axis
var xCells = 0;
var yCells = 0;


// previous; generation;
var previous = [];
// next; generation;
var next = [];

// for clearing
var interval = null;


setCanvas = function () {
    canvas = document.querySelector(canvasSelector);
    xCells = canvas.width / cellSize;
    yCells = canvas.height / cellSize;
    context = canvas.getContext('2d');
};

setDashboard = function () {
    var x, y, _results;
    context.fillStyle = deadCellColour;
    context.fillRect(0, 0, canvas.width, canvas.height);
    x = 0;
    _results = [];
    while (x < xCells) {
        previous[x] = [];
        next[x] = [];
        y = 0;
        while (y < yCells) {
            previous[x][y] = false;
            y++;
        }
        _results.push(x++);
    }
    return _results;
};

drawCell = function (x, y, alive) {
    if (alive) {
        context.fillStyle = liveCellColour;
    } else {
        context.fillStyle = deadCellColour;
    }
    return context.fillRect(x * cellSize + 1, y * cellSize + 1, cellSize - 1, cellSize - 1);
};

checkNeighbour = function (x, y) {
    var count, i, neighbours;
    count = 0;
    neighbours = [
        previous[x][(y - 1 + yCells) % yCells],
        previous[(x + 1 + xCells) % xCells][(y - 1 + yCells) % yCells],
        previous[(x + 1 + xCells) % xCells][y],
        previous[(x + 1 + xCells) % xCells][(y + 1 + yCells) % yCells],
        previous[x][(y + 1 + yCells) % yCells],
        previous[(x - 1 + xCells) % xCells][(y + 1 + yCells) % yCells],
        previous[(x - 1 + xCells) % xCells][y],
        previous[(x - 1 + xCells) % xCells][(y - 1 + yCells) % yCells]];
    i = 0;
    while (i < neighbours.length) {
        if (neighbours[i]) {
            count++;
        }
        i++;
    }
    return count;
};

checkRules = function () {
    var count, x, y, _results;
    x = 0;
    _results = [];
    while (x < xCells) {
        y = 0;
        while (y < yCells) {
            count = checkNeighbour(x, y);
            if (previous[x][y]) {
                if (count < 2 || count > 3) {
                    next[x][y] = false;
                    drawCell(x, y, false);
                }
            } else if (count === 3) {
                next[x][y] = true;
                drawCell(x, y, true);
            }
            y++;
        }
        _results.push(x++);
    }
    return _results;
};

mainGame = function () {
    var x, y, _results;
    x = 0;
    while (x < xCells) {
        y = 0;
        while (y < yCells) {
            next[x][y] = previous[x][y];
            y++;
        }
        x++;
    }
    checkRules();
    x = 0;
    _results = [];
    while (x < xCells) {
        y = 0;
        while (y < yCells) {
            previous[x][y] = next[x][y];
            y++;
        }
        _results.push(x++);
    }
    return _results;
};

handleMouse = function () {
    canvas.onmousedown = function(e) {
        var state = void 0;

        var check = function (e) {


            var x = parseInt((e.offsetX) / cellSize);
            var y = parseInt((e.offsetY) / cellSize);

            if (x > xCells - 1 || y > yCells - 1) {
                return;
            }
            if (typeof state === 'undefined') {
                state = !previous[x][y];
            }

            previous[x][y] = state;
            drawCell(x, y, previous[x][y]);
        };

        check(e);

        canvas.onmousemove = function (e) {
            check(e);
        };
    };

    canvas.onmouseup = function () {
        canvas.onmousemove = null;
    }
};

Key = (function () {
    function Key(code) {
        this.code = code;
    }

    return Key;

})();

var keyBinding = function() {
    var spaceKey = new Key(32);
    var numMinus = new Key(189);
    var numPlus = new Key(187);

    document.onkeydown = function(e) {
        if (e.which === spaceKey.code) {
            pauseStop()
        } else if (e.which === numMinus.code) {
            slower()
        } else if (e.which === numPlus.code) {
            faster()
        }
    };
};

var pauseStop = function() {
    if (interval !== null) {
        clearInterval(interval);
        interval = null;
    } else {
        interval = setInterval(mainGame, time);
    }
};

var faster = function () {
    if (time > 10) {
        time -= 10;
    }
    clearInterval(interval);
    interval = setInterval(mainGame, time);
};

var slower = function() {
    if (time <= 990) {
        time += 10;
    }
    clearInterval(interval);
    interval = setInterval(mainGame, time);
};


window.onload = function () {
    setCanvas();
    setDashboard();
    handleMouse();
    keyBinding();
};
</script>
