<template>
  <component v-if="asyncComponent" :is="asyncComponent" :key="asyncComponent.__file"></component>
</template>
<script>
import _ from 'lodash';
export default{
  computed: {
    page(){
      return this.$route.params.page;
    },
    id(){
      return this.$route.params.id;
    }

  },
  data: () => ({
    asyncComponent: null
  }),
  methods: {
    updateComponent(param) {
      console.log(`@/views/pages/${_.capitalize(param)}.vue`);
      // The dynamic import
      import(`@/views/pages/${_.capitalize(param)}.vue`).then(module => {
        this.asyncComponent = module.default;
      })
    }
  },
  beforeRouteEnter(to, from, next) {
    // When first entering the route
    next(vm => vm.updateComponent(to.params.page));
  },
  beforeRouteUpdate(to, from, next) {
    // When changing from one dynamic route to another
    this.updateComponent(to.params.page);
    next();
  }
}
</script>
