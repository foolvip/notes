# grid基本语法
阮一峰教程：https://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html   
张旭鑫教程：https://www.zhangxinxu.com/wordpress/2018/11/display-grid-css-css3/   
## 属性列表
### 作用在grid容器上
- grid-template-columns
- grid-template-rows
- grid-template-areas
- grid-template
- grid-column-gap
- grid-row-gap
- grid-gap
- justify-items
- align-items
- place-items
- justify-content
- align-content
- place-content
- grid-auto-columns
- grid-auto-rows
- grid-auto-flow
- grid（grid-template-rows，grid-template-columns，grid-template-areas，grid-auto-rows，grid-auto-columns和grid-auto-flow）
### 作用在grid子项上
- grid-column-start: <number> | <name> | span <number> | span <name> | auto
- grid-column-end: <number> | <name> | span <number> | span <name> | auto
- grid-row-start: <number> | <name> | span <number> | span <name> | auto (span <name>: 表示当前网格会自动扩展，直到命中指定的网格线名称。)
- grid-row-end: <number> | <name> | span <number> | span <name> | auto (span <number>: 表示当前网格会自动跨越指定的网格数量。)
- grid-column: grid-column: <start-line> / <end-line> | <start-line> / span <value>;
- grid-row: grid-row: <start-line> / <end-line> | <start-line> / span <value>;
- grid-area: grid-area: <name> | <row-start> / <column-start> / <row-end> / <column-end>;
- justify-self: justify-self: stretch | start | end | center;
- align-self: align-self: stretch | start | end | center;
- place-self
## 容器属性
### grid属性
### grid-template属性
### grid-row-gap、grid-column-gap、grid-gap属性
grid-row-gap属性设置行与行的间隔（行间距），grid-column-gap属性设置列与列的间隔（列间距）
```css
grid-gap: <grid-row-gap> <grid-column-gap>;
```
### grid-template-areas属性
```css
.container {
    display: grid;
    grid-template-columns: 100px 100px 100px;
    grid-template-rows: 100px 100px 100px;
    grid-template-areas: 'a b c'
                        'd e f'
                        'g h i';
}
```
上面代码先划分出9个单元格，然后将其定名为a到i的九个区域，分别对应这九个单元格。  
如果某些区域不需要利用，则使用"点"（.）表示。
```css
grid-template-areas: 'a . c'
                     'd . f'
                     'g . i';
```

## 项目属性



