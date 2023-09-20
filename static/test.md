<script>
function grid() {
const regex = /(```grid[\s\S]*?```)/g;
const grids = Array.from(markdown.matchAll(regex));
const mappedGrids = grids.map((block) => {
    const originalString = block[0];

    const lineRegex = /(.+?)\t(.+?)\t(.+?)(?:\n|$)/g;
    const lines = [...originalString.matchAll(lineRegex)];

    const modifiedLines = lines
        .map((line) => {
            const [_, item, value, description] = line;

            return `<div class="item">
                <div class="flex-between">
                    <div class="name">${item}</div>
                    <div class="price">${value}</div>
                </div>
                <div class="description">${description}</div>
            </div>`;
        })
        .join('\n');

    const modifiedBlock = modifiedLines;
    return modifiedBlock;
});

grids.forEach((match, index) => {
    markdown = markdown.replace(match[0], `<div class="grid">\n${mappedGrids[index]}\n</div>\n`);
});
}
grid();
</script>

<style>

.container {
	background-image: linear-gradient(to right, rgb(47, 55, 46), rgb(37, 45, 36));
}

body {
	color: rgba(205, 156, 112, 0.8);
}

h1 {
	font-size: 8em;
	line-height: 0px;
	color: black;
	text-decoration: bolder;
}

.grid {
    display: grid;
    margin: 25px 10px 50px 10px;
    grid-template-columns: repeat(2, 1fr);
    gap: 30px 60px;
}

.grid .flex-between {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.grid .name {
    font-size: 18px;
}

.grid .price {
    font-size: 16px;
}

.grid .description {
    font-size: 15px;
}
</style>

![도시술](http://localhost:5173/dosisool.svg)

## Speyside

```grid
글렌피딕 12	1.3	Glen Fiddich 12y 30ml
글렌피딕 15	1.6	Glen Fiddich 15y 30ml
```

## Cocktails

```grid
방구술  1.6	Fart Alcohol
도시숲  1.6	Forest of city
도시블루    1.2	Cityblue
```

## Highland

```grid
달모어 12	1.4	Dalmore 12y 30ml
달모어 15	2.8	Dalmore 15y 30ml
글렌모렌지 라산타	1.4	Glenmorangie Lasanta 30ml
글렌모렌지 퀀타루반	1.6	Glenmorangie Quinta Ruban 30ml
글렌모렌지 넥타도르	1.7	Glenmorangie Necta dor 30ml
글렌모렌지 시그넷	3.5	Glenmorangie Signet 30ml
글렌드로낙 12	1.3	Glendronach 12y 30ml
글렌드로낙 15	2.6	Glendronach 15y 30ml
오반 14	1.7	Oban 14y 30ml
달위니 15	1.5	Dalwhinnie 15y 30ml
에버펠디 12	1.1	Aberferdy 12y 30ml
에버펠디 16	1.6	Aberferdy 16y 30ml
에드라두어 발레친 10	1.5	Edradour Ballechin 10y 30ml
```
