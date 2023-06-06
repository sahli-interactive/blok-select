<template>
  <div>
    <div>
      <select v-model="model.blok">
        <option value="">Bitte Block wählen ...</option>
        <option v-for="component in components" :value="component" v-bind:key="component._uid">
          {{ component.name }} &bull; {{ component.profession }}
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
  props: ['sbLanguage', 'token'],
  methods: {
    initWith() {
      return {
        plugin: 'sai-blok-selector',
        blok: ''
      }
    },
    async fetchComponents() {
      const res = await fetch(
        `https://api.storyblok.com/v2/cdn/stories/${this.options.slug}?token=${this.token}&language=${this.sbLanguage}&version=draft`
      );
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