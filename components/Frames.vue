//Looping on a list using v-for
<template>
  <div class="row">
    <div
      v-for="(item, index) in data"
      :key="item.id"
      class="col-md-6 col-lg-4 col-12 mb-5"
    >
      <p class="mb-1">ID:{{ item.id }}</p>
      <iframe
        width="100%"
        style="border-radius:6px"
        v-bind:src="item.url"
        frameborder="0"
        allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
        allowfullscreen
      ></iframe>
      <div class="scrollframes">
        <div
          class="w-36 d-inline-block mx-1"
          v-for="(frame, ind) in item.frames"
        >
          <div>
            <div
              v-if="
                frame.id === selected_frames[index].frameId &&
                  item.id === selected_frames[index].id
              "
              class="frame-highlighter bg-primary"
            ></div>
          </div>
          <img
            @click="changeFrameCallAnother(item.id, frame.id)"
            v-bind:src="frame.url"
            class="rounded frame-style"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["data", "selected_frames"],
  methods: {
    //passing parameters to a parent function
    changeFrameCallAnother(video_id, new_id) {
      this.$emit("onClickValue", video_id, new_id);
    }
  }
};
</script>

<style>
.frame-style {
  width: 100%;
}
.w-36 {
  width: 36%;
}
div.scrollframes {
  overflow: auto;
  white-space: nowrap;
}
.frame-highlighter {
  width: 100%;
  padding: 2px;
  border-bottom-left-radius: 4px;
  border-bottom-right-radius: 4px;
}
</style>
