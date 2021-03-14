---
title: Vue3 compoistion API [setup()]
date: 2021-03-24
category: Vue
---

# 什么是 compoistion API

若果你使用过 vue2 的话，那么就肯定对 Options API 不陌生。如果有人不知到什么是 Options API，那么看到下面这段代码就明白了。

```js
export default {
  data(){
    return {
      count: 0;
    }
  },
  methods: {
    plus(){
      this.count++;
    }
  },
  computed: {
    dou(){
      return this.count * 2;
    }
  },
  watch: {
    count(val){
      consolo.log(val);
    }
  }
}
```

这种形式的就是 Options API。这样的方式我们用的很舒服，但是问题，尤其是当您的应用程序变得非常大时-考虑一下数百个组件。 当处理如此大的应用程序时，共享和重用代码变得尤为重要。

或者的单页面的逻辑十分的复杂，你会有很多的 `data` 和 `method`。然而你 `data` 和它相对应的 `method` 并不能发在一起，那么当你要修改时就变得麻烦了。而且相信你复杂的单组件中大概率存在和其他组件重复的逻辑。

为了解决这种问题，给使用者提供等多的选择，在 Vue3 中了加入 Compositions API， 其实就 `setup()` 方法。

而且 Compositions API 和 Options API 可以一起使用的。

# setup()

使用设置函数时，它将带有两个参数：

- `props`
  - 然而，因为 `props` 是反应性的，你不能使用 ES6 解构，因为它会移除道具的反应性。 如果你需要解构你的道具，你可以通过使用设置函数中的 `toRefs` 来完成:

```js
import { toRefs } from "vue";

export default {
  setup(props) {
    consolo.log(props);

    // 假设 props 中带有一个属性叫 name
    let { name } = toRefs(props);
    consolo.log(name);
  },
};
```

- `context`
  - 传递给 setup 函数的第二个参数是上下文。`context` 是一个普通的 JavaScript 对象，它公开了三个组件属性: `attrs`,`slots`,`emit`
  - `context` 对象是一个普通的 JavaScript 对象，也就是说，它不是被动的，这意味着您可以在 `context` 上安全地使用 ES6 解构。

```js
export default {
  setup(props, context) {
    consolo.log(context.attrs);
    consolo.log(context.slots);
    consolo.log(context.emit);
  },
};

// or
export default {
  setup(props, { attrs, slots, emit }) {
    ...
  },
};
```

## data

那么怎么在 setup 中像 Options API 的 data 一样定义变量， Vue3 提供了 `ref` 和 `reactive` 两个方法。

- `ref` 接受参数并将其包装在一个对象中，并返回一个 `value` 属性，然后可用于访问或更改反应变量的值, `ref` 会推导数据类型，所以直接初始值给入就 OK 了。

```js
import { ref } from "vue";
export default {
  setup() {
    let count = ref(0);
    return { count };
  },
};
```

- reactive 当我们需要定义一个对象,并且要求里面的每一个属性都是响应式时，我们可使用它。

```js
import { reactive } from "vue";
export default {
  setup() {
    let student = reactive({
      name: "tom",
      age: "20",
    });

    return { student };
  },
};
```

## method

如果你想在 setup 中定义方法十分的简单， 你只需要像定义普通的方法一样定义你的方法，最后 `return` 出来就可以模版中使用。

```js
import { ref } from "vue";

export default {
  setup() {
    let count = ref(0);

    function plus() {
      count.value++;
    }

    return { count, plus };
  },
};
```

## lifehook

为了使 Composition API 与 Options API 相比功能更完整，还需要一种在其中注册生命周期挂钩的方法。 由于 Vue3 导出了几个新功能，因此可以实现这一点。 Composition API 上的生命周期挂钩与 Options API 的名称相同，但前缀为 on。除了 `beforeCreate` 和 `created`， 想达到一样的效果，直接在 setup 中执行你的方法就 OK 了。

- `beforeMount` -> `onBeforeMount`
- `mounted` -> `onMounted`
- `beforeUpdate` -> `onBeforeUpdate`
- `updated` -> `onUpdated`
- `beforeUnmount` -> `onBeforeUnmount`
- `unmounted` -> `onUnmounted`
- `errorCaptured` -> `onErrorCaptured`
- `renderTracked` -> `onRenderTracked`
- `renderTriggered` -> `onRenderTriggered`

```js
import { ref, onMounted } from "vue";

export default {
  setup() {
    let wel = ref("");

    function initWelcome() {
      wel.value = "welcome vue3 compoistion API";
    }

    // just like `beforeCreate` or `created`
    initWelcome();

    // this is `mounted`
    onMounted(() => {
      initWelcome();
    });

    return { wel };
  },
};
```

## computed

如同 `ref` 和 `watch`，也可以通过从 vue 中导入的方式在 Vue 组件创建计算属性。

```js
import { ref, computed } from "vue";

export default {
  setup() {
    let count = ref(0);
    let dou = computed(() => count.value * 2);

    return { count, dou };
  },
};
```

## watch

```js
import { ref, watch } from "vue";

export default {
  setup() {
    let count = ref(0);

    watch(count, (oldVal, newVal) => {
      consolo.log("old:", oldVal, "new:", "newVal");
    });

    return { count };
  },
};
```

## 和模版一起使用

```html
<template>
  <h1>{{ title }}</h1>
  <div>count: {{ count }}</div>
  <div>double count: {{ dou }}</div>
  <button @click="plus">+1</button>

  <hr />

  <div>
    <ul>
      <li v-for="(item, key) in student" :key="key">{{key}}: {{item}}</li>
    </ul>
  </div>
</template>

<script>
  import { ref, reactive, computed } from "vue";

  export default {
    props: {
      title: String,
    },
    setup(props) {
      let count = ref(0);
      let dou = computed(() => count.value * 2);
      function plus() {
        readersNumber.value++;
      }

      let student = reactive({
        name: "tom",
        age: "20",
      });

      // expose to template
      return {
        count,
        dou,
        plus,
        student,
      };
    },
  };
</script>
```
