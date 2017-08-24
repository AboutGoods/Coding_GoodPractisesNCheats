# VueJS Cheat Sheet
"Faster than the doc" - this cheat sheet 2017
## Change Detection Caveats
When something is too beautiful to be true : *Reactivity*
### Reactivity in Object caveats
```js
var vm = new Vue({
  data: {
    a: 1
  }
})
// `vm.a` is now reactive
vm.b = 2
// `vm.b` is NOT reactive
```

**Solution**
```js
this.$set(this.someObject, 'b', 2)```

### Reactivity in Array caveats

```js
vm.items[indexOfItem] = newValue //NOT Reactive

vm.items.length = newLength //NOT Reactive
```

**Solutions**
```js
//1
this.$set(example1.items, indexOfItem, newValue)
//2
example1.items.splice(indexOfItem, 1, newValue)
//3
example1.items.splice(newLength)
```
