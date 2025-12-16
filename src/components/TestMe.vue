<script setup>
import { ref, onMounted } from 'vue';

const posts = ref([]);
const loading = ref(true);
const error = ref(null);

const fetchPosts = async () => {
  // Reset state
  posts.value = [];
  loading.value = true;
  error.value = null;

  try {
    // 1. Make the GET request
    const response = await fetch('https://jsonplaceholder.typicode.com/posts?_limit=5');

    // 2. Check for HTTP errors (fetch doesn't automatically throw on 4xx/5xx)
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    // 3. Parse the JSON response body
    const data = await response.json();
    posts.value = data;

  } catch (err) {
    // 4. Handle network or parsing errors
    error.value = err.message;

  } finally {
    // 5. Update loading state regardless of outcome
    loading.value = false;
  }
};

// Call the function when the component is mounted to the DOM
onMounted(fetchPosts);

function fetchData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const success = true; // Still simulates success/failure
      if (success) {
        resolve("Data fetched successfully!");
      } else {
        reject("Error: Network connection lost.");
      }
    }, 1000);
  });
}


async function handleDataFetch() {
  try {
    // 1. Execution pauses here until the fetchData Promise resolves.
    const result = await fetchData();
    
    // 2. If the Promise resolves, the value is stored in 'result'.
    console.log("Success:", result); 

  } catch (error) {
    // 3. If the Promise rejects, the execution jumps to the catch block.
    console.error("Failure:", error);
  } finally {
    // 4. Always runs after try or catch completes.
    console.log("Process complete.");
  }
}

console.log("Starting async function...");
handleDataFetch(); // Call the async function
console.log("Async function has been called and is running in the background.");

</script>

<template>
  <div>
    <h2>Blog Posts (using Fetch API)</h2>

    <p v-if="loading">Loading posts...</p>
    <p v-else-if="error" class="error">Error fetching data: {{ error }}</p>
    
    <ul v-else>
      <li v-for="post in posts" :key="post.id">
        <strong>{{ post.title }}</strong>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.error {
  color: red;
}
</style>