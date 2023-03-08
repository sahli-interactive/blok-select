<template>
  <div>
    <div>
      <select v-model="model.blok" class="select">
        <option value="">Bitte Block wählen ...</option>
        <option v-for="component in components" :value="component" v-bind:key="component._uid">
          {{ component.name }}
        </option>
      </select>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      components: []
    }
  },
  mixins: [window.Storyblok.plugin],
  methods: {
    initWith() {
      return {
        plugin: 'sai-blok-selector',
        blok: ''
      }
    },
    async fetchComponents() {
      const res = await fetch(`https://api.storyblok.com/v2/cdn/stories/${this.options.slug}?token=${this.options.token}`);
      const data = await res.json();
      this.components = data.story ? data.story.content[this.options.wrapper] : [];
    }
  },
  mounted() {
    this.fetchComponents()
  },
  watch: {
    'model': {
      handler: function (value) {
        this.$emit('changed-model', value);
      },
      deep: true
    }
  }
}
</script>

<style>
  .select {
    width: 100%;
    border-radius: 4px;
  }
</style>