<template>
  <div class="home">
    <Todos/>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Todos from '@/components/Todos.vue'; // @ is an alias to /src

@Component({
  components: {
    Todos,
  },
})
export default class Home extends Vue {
  private counter = 1;

  private head = document.querySelector("head");
  mounted() {
    setInterval(() => {
      const icon = document.querySelector("link[rel='icon']");
      if (icon) {
        icon.remove();
      }
      const shortIcon = document.querySelector("link[rel='shortcut icon']");
      if (shortIcon) {
        shortIcon.remove();
      }

      // add new favicon image
      if (this.head) {
        this.head.insertAdjacentHTML('beforeend', `<link rel="icon" href="fav${this.counter}.svg">`);
      }
      this.counter += 1;
      if (this.counter > 24) {
        this.counter = 1;
      }
    }, 200);
  }
}
</script>
