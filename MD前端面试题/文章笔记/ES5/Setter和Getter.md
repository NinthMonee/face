Setter和Getter



```js
var obj={
    a:1,
    _c:0,
    b:function(){

    },
    //set有且仅有一个参数
    set c(value){
        doucument.doucumentElement.style.backgroundColor=value;
        this._c=value;
    },
    //get不允许有任何参数
    get c(){
        return this._c;
    }
}
obj.c=10;
console.log(obj.c);

setInterval(function(){
    obj.c++;
},16);
```

如果仅有set，没有get，这个属性就是只写属性

如果仅有get，没有set，这个属性就是只读属性

```
class Box{
  const EVENT_ID="Event_ID";
}
```

