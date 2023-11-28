<script>
export default {
  props: {
    name: String,
    avatar: String,
    state: {
      type: String,
      default: 'alive'
    }
  },
  methods: {
    check() {
      if (this.state == 'dead') {
        return 'rotate-90'
      }
    }
  }
}
</script>

<template>
  <div class="flex flex-col items-center">
    <div class="px-2 py-1 mb-2 bg-black/40 rounded-md text-white">{{ name }}</div>
    <img :class="check()" class="w-max" :src="'src/assets/guddas/' + avatar + '.png'" alt="" />
  </div>
</template>
