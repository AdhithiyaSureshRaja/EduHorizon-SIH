<template>

  <div class="live-classes">
    <h1>Published Live Classes</h1>
    <!-- Show loading message if the data is still being fetched -->
    <div v-if="loading">Loading live classes...</div>

    <!-- Show error message if something went wrong -->
    <div v-if="error" class="error">
      Error: {{ error }}
    </div>

    <!-- Render live classes once data is available -->
    <ul v-if="!loading && !error">
      <li v-for="(liveClass, index) in liveClasses" :key="index">
        <h2>{{ liveClass.title }}</h2>
        <p>{{ liveClass.date }}</p>

        <!-- If a Jitsi meeting link exists, display the iframe -->
         <div v-if="liveClass?.jitsi_meeting_link">
          <a :href="liveClass.jitsi_meeting_link" target="_blank" rel="noopener noreferrer">
            Join Meeting
          </a>
        </div>        
   
        <!-- If no Jitsi meeting link, provide a fallback message or link -->
        <div v-else>
          <p>No Jitsi meeting link available. Please check again later.</p>
        </div>
        
        
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      liveClasses: [], // Store the list of live classes
      loading: true,   // Loading state
      error: null      // Error state
    };
  },
  mounted() {
    // Fetch live classes when the component is mounted
    this.fetchLiveClasses();
  },
  methods: {
    fetchLiveClasses() {
     const csrfToken = document.querySelector('meta[name="csrf_token"]').content;
      // Replace with your actual Frappe API endpoint
      axios.get('http://invictus.com:8000/api/method/lms.lms.doctype.live_class.live_class.get_published_live_classes',{
        headers: {
      'X-CSRFToken': csrfToken,  // Add CSRF token in the headers
       },
     })
        .then((response) => {
          this.liveClasses = response.data.message.message; // Store the live classes
          this.loading = false;  // Set loading to false after data is fetched
        })
        .catch((error) => {
          this.error = error.message; // Handle any errors
          this.loading = false;
        });
    }
  }
};
</script>

<style scoped>
.live-classes {
  font-family: Arial, sans-serif;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin-bottom: 15px;
  padding: 10px;
  border: 1px solid #ddd;
  background-color: #f9f9f9;
}

h2 {
  font-size: 1.5rem;
  margin: 0;
}

a {
  color: #007bff;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

iframe {
  margin-top: 10px;
  border: 1px solid #ddd;
  max-width: 100%;
}

.error {
  color: red;
  font-weight: bold;
}
</style>

