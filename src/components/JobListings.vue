<script setup>
import jobData from '@/jobs.json'
import { reactive,ref, defineProps, onMounted } from 'vue'
import JobListing1 from '@/components/JobListing1.vue'
import { RouterLink } from 'vue-router'
import BounceLoader from 'vue-spinner/src/BounceLoader.vue'
import axios from 'axios'



defineProps({
    limit: Number,
    showButton: {
        type: Boolean,
        default: true
    }
});

const state = reactive({
  jobs: [],
  isloading: true
});

onMounted(async () => {
    try {
      const response = await axios.get('/api/jobs');
      state.jobs = response.data;
    } catch (error) {
      console.error('Error fetching jobs:', error);
    } finally {
      state.isloading = false;
    }
});

</script>

<template>
    <section class="bg-blue-50 px-4 py-10">
    <div class="container-xl lg: container m-auto">
    <h2 class="text-3xl font-bold text-green-500 mb-6 text-center">Browse Job Listings
    </h2>

    <!-- Loading Spinner -->
    <div v-if="state.isloading" 
    class="text-center text-gray-500 items-center py-6">
    <BounceLoader />
    </div>

    <div v-else class="grid grid-cols-1 md:grid-cols-4 gap-4">
        <JobListing1 v-for="job in state.jobs.slice(0, limit || state.jobs.length)" :key="job.id" :job="job"/>
        </div>
    </div>
<div v-for="job in jobs" :key="job.id">
</div>
  </section>

    <section class="m-auto max-w-lg my-10 px-6">
      <RouterLink
        to="/jobs"
        class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700"
        >View All Jobs
      </RouterLink>
    </section>

</template>
