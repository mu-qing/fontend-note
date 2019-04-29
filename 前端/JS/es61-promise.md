#### 基本使用

```javascript
new Promise(function (resolve, reject) {
        // 假设此处是异步请求某个数据
         if () {
             resolve()
		  } else {
    	 	reject()
          }
})
.then(function A(data) {
        // 成功，下一步
      console.log( data);
 }, function B(error) {
        // 失败，做相应处理
       console.log(error)
 });
```

##### Promise是一个对象，它的内部其实有三种状态。

- 初始状态（ pending ）。
- 已完成（ fulfilled ）： Promise 的异步操作已结束成功。
- 已拒绝（ rejected ）： Promise 的异步操作未成功结束。

resolve 方法可以使 Promise 对象的状态改变成成功，同时传递一个参数用于后续成功后的操作。
reject 方法则是将 Promise 对象的状态改变为失败，同时将错误的信息传递到后续错误处理的操作。

#####  注意
- then可以捕获到上面的所有then的错误，所以只在最后一个then里，写错误捕获函数就可以。
- 每次异步操作时候需要返回一个新的promise，因为只有用promise对象才会等异步操作执行完，才去执行下面的then，才能拿到异步执行后的数据，所以第二个then里的异步请求，也需要声明Promise对象。如果then里面返回常量，可以直接返回。如下：

```javascript
new Promise((resolve, reject) => {
     setTimeout( () => {
          if （...）{
            resolve（[1, 2, 3]）
        } else {
            reject('error');
        }
   }, 2000);
})
 .then( value => {
        return '222';    // 如果是直接返回常量，可直接return
    })
 .then( value2 => {
     console.log(value2 ); // 打印出222
 })
```

下面忽略error情况，看两个例子，大家可以自己思考下打印结果


```javascript
new Promise(resolve => {
    setTimeout( () => {
        resolve('value1');
    }, 2000);
})
 .then( value1 => {
        console.log(value1);
        (function () {
            return new Promise(resolve => {
                setTimeout(() => {
                    console.log('Mr.Laurence');
                    resolve('Merry Xmas');
                }, 2000);
            });
        }());
        return false;
    })
 .then( value => {
     console.log(value + ' world');
 });

value1
false world
Mr.Laurence
```

```javascript
new Promise( resolve => {
    console.log('Step 1');
    setTimeout(() => {
        resolve(100);
    }, 1000);
})
.then( value => {
     return new Promise(resolve => {
         console.log('Step 1-1');
         setTimeout(() => {
            resolve(110);
         }, 1000);
    })
    .then( value => {
           console.log('Step 1-2');
           return value;
    })
   .then( value => {
         console.log('Step 1-3');
         return value;
    });
})
.then(value => {
      console.log(value);
      console.log('Step 2');
});

console:
Step 1
Step 1-1
Step 1-2
Step 1-3
110
Step 2
```


### catch
catch 方法是 then(onFulfilled, onRejected) 方法当中 onRejected 函数的一个简单的写法，也就是说可以写成 then(fn).catch(fn)，相当于 then(fn).then(null, fn)。使用 catch 的写法比一般的写法更加清晰明确。我们在捕获错误的时候，直接在最后写catch函数即可。

```javascript
 let promise = new Promise(function(resolve, reject) {
	throw new Error("Explosion!");
});
promise.catch(function(error) {
      console.log(error.message); // "Explosion!"
});

```
上面代码等于与下面的代码
```javascript
 let promise = new Promise(function(resolve, reject) {
	throw new Error("Explosion!");
});
promise.catch(function(error) {
      console.log(error.message); // "Explosion!"
});
```
###### 异步代码错误抛出要用reject
```javascript
new Promise( resolve => {
    setTimeout( () => {
        throw new Error('bye');
    }, 2000);
})
.then( value => {
 })
.catch( error => {
      console.log( 'catch', error);
 });
控制台会直接报错 Uncaught Error: bye
```
解析：因为异步情况下，catch已经执行完了，错误才抛出，所以无法捕获，所以要用reject，如下：
```javascript
new Promise( (resolve, reject) => {
    setTimeout( () => {
        reject('bye');
    }, 2000);
})
.then( value => {
        console.log( value + ' world');
 })
.catch( error => {
      console.log( 'catch', error);
 });

catch bye
利用reject可以抓捕到promise里throw的错
```
###### catch 可以捕获then里丢出来的错，且按顺序只抓捕第一个没有被捕获的错误
```javascript
new Promise( resolve => {
    setTimeout( () => {
        resolve();
    }, 2000);
})
.then( value => {
    throw new Error('bye');
 })
.then( value => {
   throw new Error('bye2');
 })
.catch( error => {
  console.log( 'catch', error);
 });

console: Error： bye

```
```javascript
new Promise( resolve => {
    setTimeout( () => {
        resolve();
    }, 2000);
})
.then( value => {
    throw new Error('bye');
 })
.catch( error => {
  console.log( 'catch', error);
 })
.then( value => {
   throw new Error('bye2');
 })
.catch( error => {
  console.log( 'catch', error);
 });

console: Error： bye
console: Error： bye2
catch 抓捕到的是第一个没有被捕获的错误

```
######  错误被捕获后，下面代码可以继续执行
```javascript
new Promise(resolve => {
    setTimeout(() => {
        resolve();
    }, 1000);
})
    .then( () => {
        throw new Error('test1 error');
    })
    .catch( err => {
        console.log('I catch：', err);   // 此处捕获了 'test1 error'，当错误被捕获后，下面代码可以继续执行
    })
    .then( () => {
        console.log(' here');
    })
    .then( () => {
        console.log('and here');
         throw new Error('test2 error');
    })
    .catch( err => {
        console.log('No, I catch：', err);  // 此处捕获了 'test2 error'
    });

I catch： Error: test2 error
here
and here
 I catch： Error: test2 error
```
######  错误在捕获之前的代码不会执行
```javascript
new Promise(resolve => {
    setTimeout(() => {
        resolve();
    }, 1000);
})
    .then( () => {
        throw new Error('test1 error');
    })
    .catch( err => {
       console.log('I catch：', err);   // 此处捕获了 'test1 error'，不影响下面的代码执行
       throw new Error('another error'); // 在catch里面丢出错误，会直接跳到下一个能被捕获的地方。
    })
    .then( () => {
        console.log('and here');
         throw new Error('test2 error');
    })
    .catch( err => {
        console.log('No, I catch：', err);  // 此处捕获了 'test2 error'
    });

I catch： Error: test2 error
I catch： Error: another error
```
```javascript
new Promise(resolve => {
    setTimeout(() => {
        resolve();
    }, 1000);
})
    .then( () => {
        console.log('start');
        throw new Error('test1 error');
    })
    .then( () => {
        console.log('arrive here');
    })
    .then( () => {
        console.log('... and here');
         throw new Error('test2 error');  
    })
    .catch( err => {
        console.log('No, I catch：', err);   // 捕获到了第一个
    });

No, I catch： Error: test1 error
    at Promise.then (<anonymous>:8:1
```




#### Promise.all
```javascript
Promise.all([1, 2, 3])
      .then( all => {
          console.log('1：', all);
      })
[1, 2, 3]
```

```javascript
Promise.all([function () {console.log('ooxx');}, 'xxoo', false])
      .then( all => {
         console.log( all);
      });
 [ƒ, "xxoo", false]
```
```javascript
let p1 = new Promise( resolve => {
            setTimeout(() => {
                resolve('I\'m P1');
            }, 1500);
});
let p2 = new Promise( (resolve, reject) => {
            setTimeout(() => {
                resolve('I\'m P2');
            }, 1000);
 });
let p3 = new Promise( (resolve, reject) => {
            setTimeout(() => {
                resolve('I\'m P3');
            }, 3000);
 });

 Promise.all([p1, p2, p3]).then( all => {
       console.log('all', all);
}).catch( err => {
        console.log('Catch：', err);
});

all (3) ["I'm P1", "I'm P2", "I'm P3"]
```

```javascript
案例：删除所有数据后，做一些事情、、、、
db.allDocs({include_docs: true}).then(function (result) {
  return Promise.all(result.rows.map(function (row) {
    return db.remove(row.doc);
  }));
}).then(function (arrayOfResults) {
  // All docs have really been removed() now!
});

```


### Promise.resolve
```javascript
Promise.resolve()
    .then( () => {
        console.log('Step 1');
    })
```

###  其他
```javascript
Promise.resolve('foo').then(Promise.resolve('bar')).then(function (result) {
  console.log(result);
});
VM95:2 foo
如果你向 then() 传递的并非是一个函数（比如 promise）
它实际上会将其解释为 then(null)，这就会导致前一个 promise 的结果会穿透下面
```
#### How do I gain access to resultA here?
```javascript
function getExample() {
    return promiseA(…).then(function(resultA) {
        // Some processing
        return promiseB(…);
    }).then(function(resultB) {
        // More processing
        return // How do I gain access to resultA here?
    });
}
```
####  解决 Break the chain
```javascript
function getExample() {
    var a = promiseA(…);
    var b = a.then(function(resultA) {
        // some processing
        return promiseB(…);
    });
    return Promise.all([a, b]).then(function([resultA, resultB]) {
        // more processing
        return // something using both resultA and resultB
    });
}
```



#### 手写promise

```js
const PENDING = 'pending'
const RESOLVED = 'resolved'
const REJECTED = 'rejected'

function Promise(fn) {
    let self = this;
   try {
      fn(resolve, reject)
    } catch (e) {
      reject(e)
    }
}

function resolve(value) {
    if (self.state === PENDING) {
        self.state = RESOLVED
        self.value = value
        self.resolvedCallbacks.map(cb => cb(that.value))
    }
}

function reject(value) {
  if (that.state === PENDING) {
    that.state = REJECTED
    that.value = value
    that.rejectedCallbacks.map(cb => cb(that.value))
  }
}


待完成


```

