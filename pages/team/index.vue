<template>
<div class="bg-white">
  <div class="mx-auto py-12 pt-32 px-4 max-w-7xl sm:px-6 lg:px-8 lg:py-24">
    <div class="grid grid-cols-1 gap-12 lg:grid-cols-3 lg:gap-8">
      <div class="space-y-5 sm:space-y-4">
        <h2 class="text-3xl font-extrabold tracking-tight sm:text-4xl">{{ $t('team.header') }}</h2>
        <p class="text-xl text-gray-500">{{ $t('team.subtext') }}</p>
      </div>
      <div class="lg:col-span-2">
        <ul class="space-y-12 sm:grid sm:grid-cols-2 sm:gap-12 sm:space-y-0 lg:gap-x-8">
          <li>
            <div class="flex items-center space-x-4 lg:space-x-6">
              <img class="w-16 h-16 rounded-full lg:w-20 lg:h-20" src="../../assets/rru.jpeg" alt="">
              <div class="font-medium text-lg leading-6 space-y-1">
                <h3>Rashmi Rathi Upadhyay</h3>
                <p class="text-indigo-600">Assistant Professor, BIT Mesra</p>
              </div>
            </div>
          </li>
          <li>
            <div class="flex items-center space-x-4 lg:space-x-6">
              <img class="w-16 h-16 rounded-full lg:w-20 lg:h-20" src="../../assets/rp.jpeg" alt="">
              <div class="font-medium text-lg leading-6 space-y-1">
                <h3>Richa Pandey </h3>
                <p class="text-indigo-600">Reasoning Educator, Unacademy</p>
              </div>
            </div>
          </li>

          <li>
            <div class="flex items-center space-x-4 lg:space-x-6">
              <img class="w-16 h-16 rounded-full lg:w-20 lg:h-20" src="../../assets/karn.png" alt="">
              <div class="font-medium text-lg leading-6 space-y-1">
                <h3>Gyan Prakash Karn</h3>
                <p class="text-indigo-600">Developer</p>
              </div>
            </div>
          </li>
          <li>
            <div class="flex items-center space-x-4 lg:space-x-6">
              <img class="w-16 h-16 rounded-full lg:w-20 lg:h-20" src="../../assets/vishesh.png" alt="">
              <div class="font-medium text-lg leading-6 space-y-1">
                <h3>Vishesh Agrawal</h3>
                <p class="text-indigo-600">Developer</p>
              </div>
            </div>
          </li>
          <li>
            <div class="flex items-center space-x-4 lg:space-x-6">
              <img class="w-16 h-16 rounded-full lg:w-20 lg:h-20" src="../../assets/sk.jpeg" alt="">
              <div class="font-medium text-lg leading-6 space-y-1">
                <h3>Shubham Kumar</h3>
                <p class="text-indigo-600">Analyst at Better.com</p>
              </div>
            </div>
          </li>
          <li>
            <div class="flex items-center space-x-4 lg:space-x-6">
              <img class="w-16 h-16 rounded-full lg:w-20 lg:h-20" src="../../assets/q.jpeg" alt="">
              <div class="font-medium text-lg leading-6 space-y-1">
                <h3>You</h3>
                <p class="text-indigo-600">mail us at mail@karngyan.com</p>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>



    <div v-show="volunteers.length > 0" class="mt-6 grid grid-cols-1 gap-12 lg:grid-cols-3 lg:gap-8">
      <div class="space-y-5 sm:space-y-4">
        <h2 class="text-3xl font-extrabold tracking-tight sm:text-4xl">{{ $t('team.vheader') }}</h2>
        <p class="text-xl text-gray-500">{{ $t('team.vsubtext') }} ❤️</p>
      </div>
      <div class="lg:col-span-2">
        <ul class="space-y-12 sm:grid sm:grid-cols-2 sm:gap-12 sm:space-y-0 lg:gap-x-8">
          <li v-for="v in volunteers">
            <div class="flex items-center space-x-4 lg:space-x-6">
              <div class="w-16 h-16 rounded-full lg:w-20 lg:h-20 flex justify-center items-center bg-purple-700">
                <span class="text-white font-extrabold">{{ (v.name.charAt(0) + v.name.charAt(1)).toUpperCase() }}</span>
              </div>
              <div class="font-medium text-lg leading-6 space-y-1">
                <h3>{{ v.name }}</h3>
                <p class="text-indigo-600">{{ v.city }}</p>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>




  </div>
</div>

</template>

<script>
export default {
  data() {
    return {
      volunteers: []
    }
  },
  methods: {
    fetchData() {
      const store = this.$store
      store.dispatch('fetchVolunteersTeam')
        .then((data) => {
          this.volunteers = data
        }).catch((error) => {
          store.dispatch('danger', 'Fetch volunteers failed')
      })

    }
  },
  created() {
    this.fetchData()
  }
}
</script>

<style scoped>

</style>
