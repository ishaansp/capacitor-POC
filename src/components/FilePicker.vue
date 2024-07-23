<template>
  <div>
    <input type="file" @change="handleFileInputChange" ref="fileInput" style="display: none;" />
    <button @click="$refs.fileInput.click()">Open File Dialog</button>
    <p v-if="pickedFile">Picked File: {{ pickedFile.name }}</p>
    <p v-if="fileMetadata">File Metadata: {{ fileMetadata }}</p>
    <p v-if="currentPosition">Current Position: Latitude {{ currentPosition.latitude }}, Longitude {{ currentPosition.longitude }}</p>
  </div>
</template>

<script>
import { defineComponent, ref } from 'vue';
import { Geolocation } from '@capacitor/geolocation';

export default defineComponent({
  name: 'FilePickerComponent',
  setup() {
    const pickedFile = ref(null);
    const fileMetadata = ref(null);
    const currentPosition = ref(null);

    const handleFileInputChange = async (event) => {
      console.log('File input change event:', event);
      const file = event.target.files[0];
      console.log('Selected file:', file);
      if (file) {
        pickedFile.value = file;

        // Simulate getting file metadata
        fileMetadata.value = `File size: ${file.size} bytes`;

        // Simulate getting geolocation
        try {
          const position = await Geolocation.getCurrentPosition();
          currentPosition.value = position.coords; // Store coordinates
        } catch (error) {
          console.error('Error getting geolocation:', error);
        }
      }
    };

    return {
      handleFileInputChange,
      pickedFile,
      fileMetadata,
      currentPosition
    };
  }
});
</script>

<style scoped>
/* Add any necessary styles */
</style>
