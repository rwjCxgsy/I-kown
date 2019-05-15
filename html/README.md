# 语义类标签

这里是列举了一部分标签

## ul

==多数出现正在行文中间，它的上文多数在提示：要列举某些项。但是，如果所有并列关系都用 ul，会造成大量冗余标签==

```html
    中国的省份
    <!-- ul表示要列举的中国省份列表 -->
    <ul>
        <li>北京</li>
        <li>上海</li>
        <li>...</li>
        <li>台湾</li>
    </ul>
```

## em

==表示强调（侧重于局部强调）==

这里强调一个还是两个

    今天我上了一个小时网
    今天我上了两个小时网

## strong

==表示强烈强调（侧重于全局强调）==

### em和strong 的区别

    em 是斜体、strong 是粗体
    em 表示内容的重点，strong 表示强烈的重要性、严重性或内容的紧迫性
    strong 不会改变所在句子的语意，em 则会改变所在句子的语义

## section 和 article 的区别

    section 一个有语义的div标签
    article 是一种特别的结构，它表示具有一定独立性质的文章

我们正确使用整体结构类的语义标签，可以让页面对机器更友好。比如：

```html
<body>
    <header>
        <nav>
            ……
        </nav>
    </header>
    <aside>
        <nav>
            ……
        </nav>
    </aside>
    <section>……</section>
    <section>……</section>
    <section>……</section>
    <footer>
        <address>……</address>
    </footer>
</body>

```

### 总结

语义化标签：==用对比不用好，乱用不如不用==

我们应该分开一些场景来看语义，把它用在合适的场景下，可以获得额外的效果。本篇文中，我们至少涉及了三个明确的场景：

* 自然语言表达能力的补充
* 文章标题摘要
* 适合机器阅读的整体结构

#### 参考文章

* [em和strong的区别](https://segmentfault.com/a/1190000002481725)
* [《重学前端》HTML语义：div和span不是够用了吗？](https://time.geekbang.org/column/article/78158)
* [《重学前端》HTML语义：如何运用语义类标签来呈现Wiki网页？](https://time.geekbang.org/column/article/78168)