# js-study
js学习笔记

##Promise
###回调的问题
1. 嵌套层次很深，难以维护
2. 无法正常使用 return 和throw
3. 无法正常检索堆栈信息
4. 多个回调之间难以建立联系
###Promise特点
1. Promise 是一个 代理对象，它和原先的操作并无关系。
2. Promise 有3个状态：
1. pending [待定] 初始状态
2. fulfilled [实现] 操作成功
3. rejected [被否决] 操作失败
3. Promise 实例一经创建，执行器立即执行。
4. Promise 状态发生改变 ，就会触发 .then() 执行后续步
4. Promise 状态发生改变 ，就会触发 .then() 执行后续步
骤。
5. Promise 状态一经改变，不会再变
###.then()
1. .then() 接受两个函数作为参数，分别代表 fulfilled 和 rejected
2. .then() 返回一个新的 Promise 实例，所以它可以链式调用
3. 当前面的 Promise 状态改变时， .then() 根据其最终状态，选择特定的状态响应
函数执行
4. 状态响应函数可以返回新的 Promise，或其它值
5. 如果返回新的 Promise，那么下一级 .then() 会在新 Promise 状态改变之后执行
6. 如果返回其它任何值，则会立刻执行下一级 .then()
####.then() 里有 .then() 的情况
因为 .then() 返回的还是 Promise 实例。
会等里面的 .then() 执行完，在执行外面的。
对于我们来说，此时最好将其展开，会更好读。
##错误处理
Promise 会自动捕获内部异常，并交给 rejected 响应函数处理。
通常有两种做法：
1. reject('错误信息').then(null, message => {})
2. throw new Error('错误信息').catch( message => {})****
##批量执行
Promise.all([p1, p2, p3, ....]) 用于将多个 Promise 实例，包装成一个新
的 Promise 实例。
1. 它接受一个数组作为参数
2. 数组里可以是 Promise 对象，也可以是别的值，只有 Promise 会等待状态改
变
3. 当所有子 Promise 都完成，该 Promise 完成，返回值是全部值的数组
3. 当所有子 Promise 都完成，该 Promise 完成，返回值是全部值的数组
4. 有任何一个失败，该 Promise 失败，返回值是第一个失败的子 Promise 的结
果
##实现队列
1. 有时候我们不希望所有动作一起发生，而是按照一定顺序，逐个进行。
##使用 .forEach()
`function queue(things) {
let promise = Promise.resolve();
things.forEach( thing => {
promise = promise.then( () => {
return new Promise( resolve => {
doThing(thing, () => {
resolve();
});
});
});
});
});
return promise;
}
queue(['lots'
,
'of'
,
'things'
, ....]);`
##使用 .reduce()
`function queue(things) {
return things.reduce( (promise, thing) => {
return promise.then( () => {
return new Promise( resolve => {
doThing(thing, () => {
resolve();
});
});
});
}, Promise.resolve());
}, Promise.resolve());
}
queue(['lots'
,
'of'
,
'things'
, ....]);`
##Promise.resolve()
1. 参数为空，返回一个状态为 fulfilled 的 Promise 实例
2. 参数是一个跟 Promise 无关的值，同上，不过 fulfuilled 响应函数会得到这个
参数
3. 参数为 Promise 实例，则返回该实例，不做任何修改
4. 参数为 thenable ，立刻执行它的 .then()
4. 参数为 thenable ，立刻执行它的 .then()
##Promise.reject()
Promise.reject() 会返回一个状态为 rejected 的 Promise 实例。
Promise.reject() 不认 thenable
##Promise.race()
Promise.race() 功能类似 Promise.all() ，不过它是有一个完成就算完成
##现实中的PROMISE
###把回调包装成PROMISE
把回调包装成 Promise 是最常见的应用。
它有两个显而易见好处：
1. 可读性更好
2. 返回的结果可以加入任何 Promise 队列
###把任何异步操作包装成PROMISE
假设需求：用户点击按钮，弹出确认窗体，用户确认和取消有不同的处理。
且不能用 window.confirm() 。
`
// 弹出窗体
let confirm = popupManager.confirm('您确定么？');
confirm.promise
.then(() => {
// do confirm staff
})
.catch(() => {
// do cancel staff
});
// 窗体的构造函数
class Confirm {
constructor() {
constructor() {
this.promise = new Promise( (resolve, reject) => {
this.confirmButton.onClick = resolve;
this.cancelBUtton.onClick = reject;

`
##FETCHAPI
Fetch API 是 XMLHttpRequest 的现代化替代方案，它更强大，也更友好。
它直接返回一个 Promise 实例。
`
fetch('some.json')
.then( response => {
return response.json();
})
.then( json => {
// do something with the json
})
.catch( err => {
console.log(err);
});
`
##总结
1. Promise 可以很好的解决异步回调问题
2. Promise 引入了不少新概念，新写法
3. Promise 也会有嵌套，可能看起来还很复杂







