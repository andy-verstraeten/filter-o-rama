<template>
  <div class="container">
    <video ref="video" muted autoplay></video>
    <canvas ref="canvas"></canvas>
  </div>
</template>

<script>
import * as faceapi from "face-api.js/dist/face-api";
export default {
  name: "Webcam",
  methods: {},
  mounted() {
    this.video = this.$refs.video;

    const MODEL_URL = "/data/models";
    Promise.all([
      faceapi.nets.tinyFaceDetector.loadFromUri(MODEL_URL),
      faceapi.nets.faceLandmark68Net.loadFromUri(MODEL_URL)
    ]).then(() => {
      if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
        navigator.mediaDevices.getUserMedia({ video: {} }).then(stream => {
          this.video.srcObject = stream;
        });
      }
      this.video.addEventListener("play", () => {
        setInterval(async () => {
          const detections = await faceapi
            .detectAllFaces(
              this.$refs.video,
              new faceapi.TinyFaceDetectorOptions()
            )
            .withFaceLandmarks();
          console.log(detections);
          detections.forEach(detection => {
            const context = this.$refs.canvas.getContext("2d");
            const points = detection.landmarks.positions;
            context.fillStyle = "#FF00000";
            context.fillRect(
              points[37].x,
              points[36].y,
              points[41].x - points[37].x,
              points[45].y - points[36].y
            );
          });
        }, 5000);
      });
    });
  }
};
</script>

<style lang="scss" scoped>
.container {
  position: relative;
  video {
  }
  canvas {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 0;
    width: 100%;
    height: 100%;
  }
}
</style>
