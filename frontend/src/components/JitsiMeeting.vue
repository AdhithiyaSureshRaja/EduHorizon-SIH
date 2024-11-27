<template>
  <div class="jitsi.cis.com">
    <iframe
      :src="meetingUrl"
      width="800"
      height="600"
      allow="camera; microphone; fullscreen; display-capture;"
      frameborder="0"
    ></iframe>
  </div>
</template>

<script>
export default {
  props: {
    meetingUrl: {
      type: String,
      required: true,
    },
  },
};
</script>

<style scoped>
.jitsi-meeting-container {
  margin: 20px;
}
</style>
