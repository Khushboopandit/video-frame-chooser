//DOM elements
<template>
  <div class="container py-3">
    <h3>Select print image</h3>
    <p class="text-secondary">
      Choose the image that will be printed on your video card.
    </p>
    <Frames
      @onClickValue="changeFrame"
      v-bind:data="frames"
      v-bind:selected_frames="selected_frames"
    />
    <button
      @click="postData"
      type="button"
      class="px-4 py-2 mb-3 btn btn-primary"
    >
      Submit
    </button>
  </div>
</template>

//Script for writing funtionality and storing data
<script>
import axios from "axios";
export default {
  //data storage
  data: () => ({
    frames: [],
    selected_frames: []
  }),

  methods: {
  //Storing a new value of a frame 
    changeFrame(video_id, new_id) {
      let index = this.selected_frames.findIndex(item => item.id == video_id);
      this.selected_frames[index].frameId = new_id;
    },

  //Sending updated data
    async postData() {
      try {
        const data = await axios.post(
          "http://www.mocky.io/v2/5ed609363400004d0006d602",
          { data: this.selected_frames }
        );
        this.frames = data.data;
      } catch (err) {
        alert("Error, Please try again.");
      }
    }
  },
  async mounted() {
  //Emit root for getting value on click to a function
    this.$root.$on("changeFrame_", (video_id, new_id) => {
      console.log(video_id, new_id);
      this.changeFrame();
    });

    //Get data to render
    try {
      const data = (
        await axios.get(
          "http://www.mocky.io/v2/5ed5fda4340000740006d560?mocky-delay=500ms"
        )
      ).data;
      this.frames = data;
      //Set default frames for highlighting
      data.map((frame, index) => {
        let obj = {
          id: frame.id,
          frameId: frame.frames[0].id
        };
        this.selected_frames.push(obj);
      });
    } catch (err) {
      console.log(err)
    }
  }
};
</script>

<style>
  body {
    font-family: sans-serif;
  }
</style>
