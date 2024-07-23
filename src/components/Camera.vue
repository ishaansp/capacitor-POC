<template>
  <div>
    <button @click="takePicture">Take Picture</button>
    <img v-if="photoData" :src="photoData" style="max-width: 100%; height: auto;" />
  </div>
</template>

<script>
import { defineComponent, ref } from 'vue';
import { Camera, CameraResultType } from '@capacitor/camera';

export default defineComponent({
  name: 'CameraComponent',
  setup() {
    const photoData = ref(null);

    const takePicture = async () => {
      try {
        const image = await Camera.getPhoto({
          quality: 90,
          allowEditing: false,
          resultType: CameraResultType.Uri
        });

        // Display the photo
        photoData.value = image.webPath;
      } catch (error) {
        console.error('Error taking picture:', error);
      }
    };

    return {
      takePicture,
      photoData
    };
  }
});
</script>

<style scoped>
/* Add your component-specific styles here */
</style>