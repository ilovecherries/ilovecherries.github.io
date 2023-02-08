---
layout: post
title:  "Mutating Values in Arrays in React"
date:   2023-01-07 10:41:08 -0500
categories: programming
---
currently using preact to update one of my projects and i realized i came across something that was annoying that i didn't encounter in vue (my preferred framework), arrays need to be copied entirely on every kind of mutation because react tracks references to object.

this is especially frustrating if you have an array of things that are each supposed to be reactive and changeable using controls on the page.

```jsx
function App() {
  const [arr, setArr] = React.useState([false, false, false]);
  function modifyArray(index, value) {
    const copy = arr.slice()
    copy[index] = value
    setArr(copy)
  }
  return <div>
    {arr.map((v, i) => <div>
      <button onClick={() => modifyArray(i, !v)}>Toggle</button>
      <span>{v ? "Yes!" : "No."}</span>
    </div>)}
  </div>
}
```

as demonstrated, each item has a button that, when clicked, updates the item at the specified index using the `modifyArray` function. this code makes a copy of the array, sets a value in the copy, and then changes the state of the `arr` variable. you end up having to do this individually for each array, unless there's a helper of some kind i'm like totally missing (which sounds exactly like me to miss...)

## using `__proto__` to bypass copying

there's something very cursed here we can do to not have to copy the array on every change though, it's possible to make an object with a new reference that refers to the same object without having to create a copy. by setting the `__proto__` field in a new object, this new object has its own reference but still inherits everything from the original object, including the stored data. this means we can modify the array without having to make a copy.

```js
function modifyArray(index, value) {
  arr[index] = value
  setArr({__proto__: arr})
}
```

and according to this [benchmark](http://jsben.ch/MHuLv) i created that compares between making a copy with `arr.slice()` and making an object with the same prototype with `{__proto__:arr}` with a unique reference, it's technically faster to make an object by making a new object and setting `__proto__`.

![results presented from jsbench: making new object with reference is 100% ratio while making copy is 90.57% ratio](/assets/2023-01-07-jsbench.avif)

one known issue with creating a new object with `__proto__` set to the array is that the criteria that signifies that it is an array is lost, so methods like `Array.isArray` don't work anymore. there may be other inconsistencies that arise from using this technique, so it's probably best just to use array copying unless you KNOW what you're doing.

i don't think i'm ever going to use it, but i think it's neat to know its existence.

## we can make this easier for ourselves (really really evil)
something that i *could* do (but i would rather just not use react) is to use [proxies](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy).  
 
here's a helper that i made [(and a POC)](https://codepen.io/iadorecherries/pen/RwBoOEY) that you can use to modify arrays and update the state simultaneously without having to constantly repeat the boilerplate for array mutation every single time.

```js
function useArray([arr, setArr]) {
  return new Proxy(arr, {
    set(target, prop, value) {
      const copy = target.slice()
      copy[prop] = value
      setArr(copy)
    }
  })
}
```

using this new `useArray` helper, we can wrap `useState` in arrays and every mutation that happens to a value in the array automatically updates the state in the component. of course, this doesn't work with other actions, like changing the length, pushing, shifting, you need to set the state of the array manually for those actions. this is a very specific proxy that helps with my specific use-case, which is to mutate values that already exist in an array. it *is* possible, however, to be able to implement those things in a proxy, but i won't....

this helper allows us to reduce the code we had earlier using the `modifyArray` function and remove it and replace it with something that is smaller and can be reused whenever we have this same pattern:

```jsx
function App() {
  const arr = useArray(React.useState([false, false, false]))
  return <div>
    {arr.map((v, i) => <div>
      <button onClick={() => arr[i] = !v}>Toggle</button>
      <span>{v ? "Yes!" : "No."}</span>
    </div>)}
  </div>
}
```

finally, getting the features i want without having to do that awful boilerplate... and i can keep reusing this for all arrays that this pattern fits. of course, i think it being non-obvious behaviour is enough reason to be disgusted w.

# enter vue
so as a sort of update, i decided to actually just convert the entire project to vue (which took like less than an hour for me) and vue handles these kinds of situations so much better.

i recreated the example that i made for demonstrating mutating values in preact here:
```html
<div id="app">
  <div v-for="(v, i) in arr" :key="i">
    <label>
      <button @click="arr[i] = !v">Toggle!</button>
      <span v-if="v">Yes.</span>
      <span v-else>No!</span>
    </label>
  </div>
</div>
```

```js
Vue.createApp({
  data() {
    return {
      arr: [false, false, false]
    }
  }
}).mount("#app")
```

[vue actually uses proxies](https://vuejs.org/guide/extras/reactivity-in-depth.html#how-reactivity-works-in-vue) in order to do the same thing that i did in my react example, which is to capture a change to something reactive in the array and then update the state.

of course, a better way to actually make controls like these that play nicely with vue is to use form controls that have the `v-model` interface for what we want to express. in this case, those would be checkboxes.

```html
<input type="checkbox" v-model="arr[i]">
```

unfortunately, we need to refer to some kind of reference instead of a value (which makes sense) so we can't just plop `v` in `v-model`. however, you are able to update properties of an object since you are able to resolve a reference from a property access.

just need to add the following to our data function:

```js
arr2: [{x: false}, {x: false}, {x: false}]
```

add this to our template:

```html
<div v-for="(v, i) in arr2" :key="i">
  <label>
    <input type="checkbox" v-model="v.x">
    <span v-if="v.x">Yes.</span>
    <span v-else>No!</span>
  </label>
</div>
```

and we can keep modifying properties of an object that are in an array like this. i basically used this all over my personal project and it was nice.

---

this post was originally made on cohost: <https://cohost.org/cherry/post/812098-so-as-a-sort-of-upda>