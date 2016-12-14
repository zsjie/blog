# 数组方法

关于遍历方法的遍历元素范围：遍历的元素的范围在第一次调用 callback 时就已经确定了。在调用遍历方法后被添加到数组中的值不会被 callback 访问到。如果数组中存在且还未被访问到的元素被 callback 改变了，则其传递给 callback 的值是遍历方法访问到它那一刻的值。

工具类方法：

- Array.from()
- Array.isArray()
- Array.of()

遍历方法：

- Array.prototype.forEach
- Array.prototype.map
- Array.prototype.reduce
- Array.prototype.reduceRight
- Array.prototype.every
- Array.prototype.some
- Array.prototype.entries（**ES6**）
- Array.prototype.keys（**ES6**）
- Array.prototype.values（**ES6**）
- Array.prototype.filter

查找方法：

- Array.prototype.find（**ES6**）
- Array.prototype.findIndex（**ES6**）
- Array.prototype.indexOf
- Array.prototype.lastIndexOf
- Array.prototype.includes（**ES6**）

增删元素方法：

- Array.prototype.pop
- Array.prototype.push
- Array.prototype.unshift
- Array.prototype.shift
- Array.prototype.splice

截取/拼接数组方法：

- Array.prototype.concat
- Array.prototype.slice

排序方法：

- Array.prototype.reserve
- Array.prototype.sort

其他：

- Array.prototype.copyWithin（**ES6**）
- Array.prototype.fill（**ES6**）
- Array.prototype.join
- Array.prototype.toLocalString
- Array.prototype.toString

## 数组方法的参数和返回值

```javascript
/***********************************
 * 工具方法
 ***********************************/

// from 方法可以将一个类数组对象或可遍历对象转换成真正的数组
function fromTest () {
    // 字符串对象既是类数组又是可迭代对象
    console.log(Array.from("foo"))                      // ["f", "o", "o"]

    // 使用 map 函数转换数组元素
    console.log(Array.from([1, 2, 3], x => x + x))      // [2, 4, 6]

    // 生成一个数字序列
    console.log(Array.from({length: 5}, (v, k) => k))    // [0, 1, 2, 3, 4]
}

// of 方法会将它的任意类型的多个参数放在一个数组里并返回
// Array.of() 和 Array 构造函数不同的是：在处理数值类型的参数时，Array.of(42) 创建的数组只有一个元素，即 42, 但 Array(42) 创建了42个元素，每个元素都是undefined。
function ofTest () {
    Array.of(1);         // [1]
    Array.of(1, 2, 3);   // [1, 2, 3]
    Array.of(undefined); // [undefined]
}

/***********************************
 * 遍历方法
 ***********************************/

// forEach 方法，对数组中每一个元素执行一次 callback，返回值是 undefined
// 不会改变原数组
var arr = ['a', 'b', 'c']
var obj = {
    a: 'foo',
    b: 'bar',
    c: 'log'
}
function forEachTest () {
    arr.forEach(function(ele, index, array) {
        console.log(this[ele])
    }, obj)
}

// map 方法返回数组中每一个元素调用 callback 产生的返回值组成的新数组
// 不会改变原数组
function mapTest () {
    var newArr = arr.map(function(ele, index, array) {
        return ele + '1'
    })
    console.log(newArr)
}

// every 方法测试数组中每一个元素是否通过指定函数的测试
// 如果全部通过，则返回 true，否则返回 false
// 不会改变原数组
function everyTest () {
    arr.every(function (ele, index, array) {
        return ele.length > 1
    })
}

// some 方法测试数组是否有元素通过指定函数的测试
// 如果找到元素通过测试，立即返回 true，否则返回 false
// 不会改变原数组
function someTest () {
    arr.some(function (ele, index, array) {
        return ele.length > 1
    })
}

// entries 方法返回一个 Array Iterator 对象，该对象包含数组中每一个索引的键值对。
function entriesTest () {
    var iterator = arr.entries(),
        item

    while (item = iterator.next().value) {
        console.log(item)
    }
}

// keys 方法返回一个数组索引的迭代器
function keysTest () {
    var iterator = arr.keys(),
        item

    while ((item = iterator.next().value) != undefined) {
        console.log(item)
    }
}

// values 方法返回一个新的 Array Iterator 对象，该对象包含数组每个索引的值
function valuesTest () {
    var  iterator = arr.values() // 当前 chrome 版本未支持该方法
    for (let item of iterator) {
        console.log(item)
    }
}

// reduce 方法接收一个函数作为累加器（accumulator），数组中的每个值（从左到右）开始合并，最终为一个值
// 两个参数，callback 和 initValue
// 其中 callback 又有四个参数：
// - preValue, 上一次调用返回的值
// - curValue，当前正在处理的元素
// - index，当前元素的索引
// - array，原数组
// reduce 方法和 reduceRight 的区别是，reduceRight 从右到左遍历
function reduceTest () {
    var arr = [2, [3, 4], 1, 5, [7, 8, 10], 9]
    var res = arr.reduce(function (pv, cv, i, array) {
        if (Array.isArray(cv)) pv = pv.concat(cv)
        else pv.push(cv)

        return pv
    }, [])
    console.log(res)
}

// filter 方法使用指定的函数测试所有元素，并创建一个包含所有通过测试的元素的新数组
// filter 不会改变原数组。
function filterTest () {
    var arr = [12, 5, 8, 130, 44]
    var res = arr.filter(function (ele, index, array) {
        return ele > 10
    })
    console.log(res) // [12, 130, 44]
}

/***********************************
 * 查找方法
 ***********************************/

// find 方法，如果数组中某个元素满足测试条件，find 方法就会返回那个元素的值，如果没有满足条件的元素，则返回 undefined
// 区别：findIndex 方法返回的是满足条件的元素的索引，而非它的值
function findTest () {
    var inventory = [
        {name: 'apples', quantity: 2},
        {name: 'bananas', quantity: 0},
        {name: 'cherries', quantity: 5}
    ];

    function findCherries(fruit) {
        return fruit.name === 'cherries';
    }

    console.log(inventory.find(findCherries)); // { name: 'cherries', quantity: 5 }
    console.log(inventory.findIndex(findCherries)); // 2
}

// indexOf 方法返回给定元素能找在数组中找到的第一个索引值，否则返回-1
// 可以指定开始查找的位置，如果该索引值大于或等于数组长度，意味着不会在数组里查找，返回-1
// lastIndexOf 则返回在数组中找到的最后一个索引值
function indexOfTest () {
    var array = [2, 5, 9];
    array.indexOf(2);     // 0
    array.indexOf(7);     // -1
    array.indexOf(9, 2);  // 2
    array.indexOf(2, -1); // -1
    array.indexOf(2, -3); // 0
}

// includes 方法用来判断当前数组是否包含某指定的值，如果是，则返回 true，否则返回 false
function includesTest () {
    [1, 2, 3].includes(2);     // true
    [1, 2, 3].includes(4);     // false
    [1, 2, 3].includes(3, 3);  // false
    [1, 2, 3].includes(3, -1); // true
    [1, 2, NaN].includes(NaN); // true
}

/***********************************
 * 增删元素方法
 ***********************************/

// pop 方法删除一个数组中的最后一个元素，并返回该元素
// pop 被有意设计成具有通用性，该方法可以通过 call 或 apply 方法应用于一个类数组（array-like）对象上。
// pop 会修改原数组
function popTest () {
    var myFish = ["angel", "clown", "mandarin", "surgeon"];

    console.log("myFish before: " + myFish);

    var popped = myFish.pop();

    console.log("myFish after: " + myFish);
    console.log("Removed this element: " + popped);
}

// push 方法添加一个或多个元素到数组的末尾，并返回数组新的长度（length 属性值）
// push 会修改原数组
function pushTest () {
    var sports = ["soccer", "baseball"];
    var total = sports.push("football", "swimming");

    console.log(sports); // ["soccer", "baseball", "football", "swimming"]
    console.log(total);  // 4
}

// unshift 方法在数组的开头添加一个或者多个元素，并返回数组新的 length 值
// unshift 会修改原数组
function unshiftTest () {
    var arr = [1, 2];

    arr.unshift(0); //result of call is 3, the new array length
    //arr is [0, 1, 2]

    arr.unshift(-2, -1); // = 5
    //arr is [-2, -1, 0, 1, 2]

    arr.unshift( [-3] );
    //arr is [[-3], -2, -1, 0, 1, 2]
}

// shift 方法删除数组的 第一个 元素，并返回这个元素
// shift 会修改原数组
function shiftTest () {
    var myFish = ['angel', 'clown', 'mandarin', 'surgeon'];

    console.log('调用 shift 之前: ' + myFish);
    // "调用 shift 之前: angel,clown,mandarin,surgeon"

    var shifted = myFish.shift();

    console.log('调用 shift 之后: ' + myFish);
    // "调用 shift 之后: clown,mandarin,surgeon"

    console.log('被删除的元素: ' + shifted);
    // "被删除的元素: angel"
}

// splice 方法用新元素替换旧元素，以此修改数组的内容
// array.splice(start, deleteCount[, item1[, item2[, ...]]])
// start: 从数组的哪一位开始修改内容。如果超出了数组的长度，则从数组末尾开始添加内容；如果是负值，则表示从数组末位开始的第几位。
// deleteCount: 整数，表示要移除的数组元素的个数。如果 deleteCount 是 0，则不移除元素。这种情况下，至少应添加一个新元素。如果 deleteCount 大于 start 之后的元素的总数，则从 start 后面的元素都将被删除（含第 start 位）
// itemN: 要添加进数组的元素。如果不指定，则 splice() 只删除数组元素
function spliceText () {
    var myFish = ["angel", "clown", "mandarin", "surgeon"];

    //从第 2 位开始删除 0 个元素，插入 "drum"
    var removed = myFish.splice(2, 0, "drum");
    //运算后的 myFish:["angel", "clown", "drum", "mandarin", "surgeon"]
    //被删除元素数组：[]，没有元素被删除

    //从第 3 位开始删除 1 个元素
    removed = myFish.splice(3, 1);
    //运算后的myFish：["angel", "clown", "drum", "surgeon"]
    //被删除元素数组：["mandarin"]

    //从第 2 位开始删除 1 个元素，然后插入 "trumpet"
    removed = myFish.splice(2, 1, "trumpet");
    //运算后的myFish: ["angel", "clown", "trumpet", "surgeon"]
    //被删除元素数组：["drum"]

    //从第 0 位开始删除 2 个元素，然后插入 "parrot", "anemone" 和 "blue"
    removed = myFish.splice(0, 2, "parrot", "anemone", "blue");
    //运算后的myFish：["parrot", "anemone", "blue", "trumpet", "surgeon"]
    //被删除元素的数组：["angel", "clown"]

    //从第 3 位开始删除 2 个元素
    removed = myFish.splice(3, Number.MAX_VALUE);
    //运算后的myFish: ["parrot", "anemone", "blue"]
    //被删除元素的数组：["trumpet", "surgeon"]
}

/***********************************
 * 截取/拼接数组方法
 ***********************************/

// concat 方法将传入的数组或非数组值与原数组合并,组成一个新的数组并返回。可以传入多个数组
// 不会修改原数组
function concatTest () {
    var alpha = ["a", "b", "c"],
        numeric = [1, 2, 3]

    // 组成新数组 ["a", "b", "c", 1, 2, 3]; 原数组 alpha 和 numeric 未被修改
    var alphaNumeric = alpha.concat(numeric)
}

// slice 方法会浅复制（shallow copy）数组的一部分到一个新的数组，并返回这个新数组
// 不会修改原数组
// slice(begin, end) // [begin, end)
function sliceTest () {
    var arr = ['apple', 'orange', 'banana', 'mango']
    console.log(arr.slice(1, 3)) // ['orange', 'banana']
}

/***********************************
 * 排序方法
 ***********************************/

// reserve 方法颠倒数组中元素的位置。第一个元素会成为最后一个，最后一个会成为第一个
// sort 法对数组的元素做原地的排序，并返回这个数组。 sort 排序可能是不稳定的。默认按照字符串的Unicode码位点（code point）排序
// compareFunction: 可选。用来指定按某种顺序进行排列的函数。如果省略，元素按照转换为的字符串的诸个字符的Unicode位点进行排序。
function sortTest1 () {
    var fruit = ['cherries', 'apples', 'bananas'];
    fruit.sort(); // ['apples', 'bananas', 'cherries']

    var scores = [1, 10, 2, 21];
    scores.sort(); // [1, 10, 2, 21]
    // Watch out that 10 comes before 2,
    // because '10' comes before '2' in Unicode code point order.

    var things = ['word', 'Word', '1 Word', '2 Words'];
    things.sort(); // ['1 Word', '2 Words', 'Word', 'word']
    // In Unicode, numbers come before upper case letters,
    // which come before lower case letters.
}

// 如果指明了 compareFunction ，那么数组会按照调用该函数的返回值排序。记 a 和 b 是两个将要被比较的元素：
// 如果 compareFunction(a, b) 小于 0 ，那么 a 会被排列到 b 之前；
// 如果 compareFunction(a, b) 等于 0 ， a 和 b 的相对位置不变。备注： ECMAScript 标准并不保证这一行为，而且也不是所有浏览器都会遵守（例如 Mozilla 在 2003 年之前的版本）；
// 如果 compareFunction(a, b) 大于 0 ， b 会被排列到 a 之前。
// compareFunction(a, b) 必须总是对相同的输入返回相同的比较结果，否则排序的结果将是不确定的。
// 希望比较数字而非字符串，比较函数可以简单的以 a 减 b，如下的函数将会将数组升序排列

function sortTest2 () {
    var numbers = [4, 2, 5, 1, 3];
    numbers.sort(function(a, b) {
        return a - b;
    });
    console.log(numbers); // [1, 2, 3, 4, 5]

    // 对象可以按照某个属性排序：
    var items = [
        { name: 'Edward', value: 21 },
        { name: 'Sharpe', value: 37 },
        { name: 'And', value: 45 },
        { name: 'The', value: -12 },
        { name: 'Magnetic' },
        { name: 'Zeros', value: 37 }
    ];

    items.sort(function (a, b) {
        if (a.value > b.value) {
            return 1;
        }
        if (a.value < b.value) {
            return -1;
        }
        // a 必须等于 b
        return 0;
    });
}

/***********************************
 * 其他
 ***********************************/

// copyWithin 方法浅拷贝数组的部分元素到同一数组的不同位置，且不改变数组的大小，返回该数组
// arr.copyWithin(target[, start[, end]])
// target: 0 为基底的索引，复制序列到该位置。如果是负数，target 将从末尾开始计算。如果 target 大于等于 arr.length，将会不发生拷贝。如果 target 在 start 之后，复制的序列将被修改以符合 arr.length。
// start: 0 为基底的索引，开始复制元素的起始位置。如果是负数，start 将从末尾开始计算。如果 start 被忽略，copyWithin 将会从0开始复制。
// end: 0 为基底的索引，开始复制元素的结束位置。copyWithin 将会拷贝到该位置，但不包括 end 这个位置的元素。如果是负数， end 将从末尾开始计算。如果 end 被忽略，copyWithin 将会复制到 arr.length。
function copyWithin () {
    [1, 2, 3, 4, 5].copyWithin(-2);
    // [1, 2, 3, 1, 2]

    [1, 2, 3, 4, 5].copyWithin(0, 3);
    // [4, 5, 3, 4, 5]

    [1, 2, 3, 4, 5].copyWithin(0, 3, 4);
    // [4, 2, 3, 4, 5]

    [1, 2, 3, 4, 5].copyWithin(0, -2, -1);
    // [4, 2, 3, 4, 5]
}

// 使用 fill() 方法，可以将一个数组中指定区间的所有元素的值, 都替换成或者说填充成为某个固定的值
// fill 方法接受三个参数 value, start 以及 end. start 和 end 参数是可选的, 其默认值分别为 0 和 this 对象的 length 属性值。
// 具体要填充的元素区间是 [start, end) , 一个半开半闭区间
function fillTest () {
    [1, 2, 3].fill(4)            // [4, 4, 4]
    [1, 2, 3].fill(4, 1)         // [1, 4, 4]
    [1, 2, 3].fill(4, 1, 2)      // [1, 4, 3]
    [1, 2, 3].fill(4, 1, 1)      // [1, 2, 3]
    [1, 2, 3].fill(4, -3, -2)    // [4, 2, 3]
    [1, 2, 3].fill(4, NaN, NaN)  // [1, 2, 3]
    Array(3).fill(4);            // [4, 4, 4]
    [].fill.call({length: 3}, 4) // {0: 4, 1: 4, 2: 4, length: 3}
}

// join，toLocalString 和 toString 比较简单，就不展开讲了

```
