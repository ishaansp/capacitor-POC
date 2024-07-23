<template>
  <div>
    <input type="file" @change="handleFileInputChange" ref="fileInput" style="display: none;" />
    <button @click="$refs.fileInput.click()">Open File Dialog</button>
    <p v-if="pickedFile">Picked File: {{ pickedFile.name }}</p>
    <p v-if="fileMetadata">File Metadata:</p>
    <ul v-if="fileMetadata">
      <li>Name: {{ fileMetadata.name }}</li>
      <li>Type: {{ fileMetadata.type }}</li>
      <li>Size: {{ fileMetadata.size }} bytes</li>
      <li>Last Modified: {{ formattedLastModified }}</li>
    </ul>
    <p v-if="currentPosition">Current Position: Latitude {{ currentPosition.latitude }}, Longitude {{ currentPosition.longitude }}</p>
  </div>
</template>

<script>
import { defineComponent, ref, computed } from 'vue';
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

        // Get file metadata
        fileMetadata.value = {
          name: file.name,
          type: file.type,
          size: file.size,
          lastModified: file.lastModified
        };

        // Simulate getting geolocation
        try {
          const position = await Geolocation.getCurrentPosition();
          currentPosition.value = position.coords; // Store coordinates
        } catch (error) {
          console.error('Error getting geolocation:', error);
        }
      }
    };

    // Computed property to format lastModified date
    const formattedLastModified = computed(() => {
      if (fileMetadata.value && fileMetadata.value.lastModified) {
        const timestamp = fileMetadata.value.lastModified;
        const date = new Date(timestamp);
        return date.toLocaleString();
      }
      return '';
    });

    return {
      handleFileInputChange,
      pickedFile,
      fileMetadata,
      currentPosition,
      formattedLastModified
    };
  }
});
</script>

<style scoped>
/* Add any necessary styles */
</style>
