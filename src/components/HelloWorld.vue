<template>

  <div class="hello">
    <h1>{{ msg }}</h1>
    <h2>Ojb's name is {{ obj.name }}</h2>
    <h2>Ojb's age is {{ obj.age }}</h2>
    <button @click="changeMsg">change</button>
    <button @click="done">destory</button>
    <button @click="setAge">setAge</button>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      count: 0,
      obj: {
        name: 'john'
      }
    }
  },
  methods: {
    changeMsg () {
      this.count++
      this.msg = 'Hello World!' + this.count
    },
    done () {
      console.log(this)
      this.$destroy()
    },
    setAge () {
      this.obj.age = 20
    }

  },
  beforeCreate () {
    console.log('beforeCreate')
  },
  created () {
    console.log('created')
  },
  beforeMount () {
    console.log('beforeMount ')
  },
  mounted () {
    console.log('mounted ')
  },
  beforeUpdate () {
    console.log('beforeUpdate ')
  },
  updated () {
    console.log('updated ')
  },
  beforeDestroy () {
    console.log('beforeDestroy ')
  },
  destroyed () {
    console.log('destroyed ')
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
