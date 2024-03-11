<template>
  <div>
    <input type="file" ref="fileInput" @change="handleFileChange" />
    <button v-if="!isLoading" @click="uploadFile">Upload</button>
    <span v-else>Uploading...</span>
  </div>
</template>

<script>
import axios from 'axios'; // Import axios for HTTP requests

export default {
  data() {
    return {
      isLoading: false,
      selectedFile: null,
    };
  },
  methods: {
    handleFileChange(event) {
      this.selectedFile = event.target.files[0];
    },
    async uploadFile() {
      if (!this.selectedFile) return;

      this.isLoading = true;
      try {
        const formData = new FormData();
        const originalName = this.selectedFile.name;
        formData.append('file', this.selectedFile); // Add file to form data
        formData.append('originalname', originalName); // Add file to form data


        // Replace with your NestJS backend API endpoint URL
        const response = await axios.post('http://localhost:3000/files/upload', formData, {
          headers: {
            'Content-Type': 'multipart/form-data', // Set content type for file upload
          },
        });

        console.log('Upload successful:', response.data);
        this.selectedFile = null; // Clear selected file after upload
      } catch (error) {
        console.error('Upload failed:', error);
      } finally {
        this.isLoading = false;
      }
    },
  },
};
</script>

<style>
/* Add any styling for the component if needed */
</style>
