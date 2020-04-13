<template>
  <div class="w-full px-2 md:px-24">
    <repoItem v-for="(item, index) in items" :key="`repository_${index}`" :item="item" />
    <client-only>
      <infinite-loading v-if="!loading" spinner="spiral" @infinite="getMore">
        <div slot="no-more"></div>
        <div slot="no-results"></div>
      </infinite-loading>
    </client-only>
  </div>
</template>

<script>
import repoItem from "~/components/repository-item";

import moment from "moment";
export default {
  data(){
    return {
      pageNumber: 1,
      date: moment().add(-30, 'd').format('YYYY-MM-DD'),
      items: [],
      loading:true
    }
  },
  async mounted(){
    await this.getData();
  },
  methods: {
    async getData(){  
      this.loading = true;
      let endpoint = `https://api.github.com/search/repositories?q=created:>${this.date}&sort=stars&order=desc`;
      try {
        let { data } = await this.$axios.get(endpoint);
        this.items = data.items;
        this.pageNumber++;
        this.loading = false;
      } catch (error) {
        this.loading = false;
      }
    },
    async getMore($state) {
			let endpoint = `https://api.github.com/search/repositories?q=created:>${this.date}&sort=stars&order=desc&page=${this.pageNumber}`;
				try {
					let { data } = await this.$axios.get(endpoint);
					this.items = [...this.items, ...data.items];
          this.pageNumber++;
          if(!data.items.length) $state.complete();
					else $state.loaded();
				} catch (e) {
						$state.complete();
        }
		},
  },
  components: {
    repoItem
  }
}
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
  @apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}
</style>
