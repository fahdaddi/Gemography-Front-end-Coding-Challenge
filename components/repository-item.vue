<template>
  <div class="w-full mb-10 rounded-lg bg-gray-100 hover:bg-gray-200 transition p-6">
      <span class="font-bold text-blue-900 cursor-pointer" @click="openLink(item.html_url)">{{item.name}}
      </span>
      <p>{{item.description}}</p>
      <div class="flex items-center justify-between mt-4">
          <div class="cursor-pointer flex items-center " @click="openLink(item.owner.html_url)">
              <img class="border border-blue-100 h-10 rounded-full w-10" :src="item.owner.avatar_url" alt="">
              <span class="font-medium ml-2 text-blue-600">{{item.owner.login}}</span>
          </div>
          <div class="flex items-center mt-4">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" class="fill-current text-blue-900" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"></polygon></svg>
                <span class="font-medium ml-2 text-blue-600">{{displayNumber(item.stargazers_count)}}</span>
          </div>
      </div>
  </div>
</template>

<script>
export default {
    props: {
        item: {
            type: Object,
            required: true
        }
    },
    methods: {
        openLink(url){
             window.open(url, "_blank"); 
        },
        displayNumber(num){
            return Math.abs(num) > 999 ? Math.sign(num)*((Math.abs(num)/1000).toFixed(1)) + 'k' : Math.sign(num)*Math.abs(num);
        }
    }
}
</script>

<style>
.transition {
  transition: .3s cubic-bezier(.3, 0, 0, 1.3)
}
</style>