<template>
  <div>
    <button id="start">Start</button>
    <button id="stop">Stop</button>
    <div>
      <video
        width="600"
        id="live"
        style="box-sizing: border-box; border: 4px solid #f48"
      ></video>
    </div>


    <button @click="updata">上传</button>
  </div>
</template>

<script>
// import {uploadVideoFile } from "../api/index";
export default {
  data() {
    return {
      mediaRecorderData: "",
      n:0,
      timer:"",
      blobData:"",
    };
  },
  mounted() {
    this.getNavigator()
  },
  created() {},
  methods: {
    getNavigator(){
      
      let stopButton = document.getElementById("stop");
      let startButton = document.getElementById("start");

      navigator.mediaDevices
      .getUserMedia({
        audio: true,
        video: true,
      })
      .then((stream) => {
        // console.log(stream, "stream");
        let liveVideo = document.getElementById("live");
        // liveVideo.src = URL.createObjectURL(stream); // 你会看到一些警告
        liveVideo.srcObject = stream;
        liveVideo.play();

        stopButton.addEventListener("click", this.stopLive);
        startButton.addEventListener("click", (e) => {
          this.startLive(stream);
        });
      });
    },
	
    // 暂停后下载视频
    downLoadVideo(chunks) {
        let downloadLink = document.createElement("a");
        downloadLink.href = URL.createObjectURL(
            new Blob(chunks, {
                type: "application/video",
            })
        );
        // downloadLink.download = 'live.webm';
        // downloadLink.download = "live.ogg";
        downloadLink.download = "live.mp4";
        downloadLink.click();
    },

    // 结束录制
    stopLive() {
      this.n = 0;
      if (this.mediaRecorderData) { 
        this.mediaRecorderData.stop();
      } else {
        alert("还没有开始。");
      }
    },

    // 开始
    startLive(stream) {
        let recordedChunks = [];
        this.mediaRecorderData = new MediaRecorder(stream);

        this.mediaRecorderData.start();

        this.mediaRecorderData.addEventListener("dataavailable", (e)=> {
            if (e.data.size > 0) {
                recordedChunks.push(e.data);
                // console.log(recordedChunks,"recordedChunks");
                this.blobData = recordedChunks;
                console.log(this.blobData,"this.blobData");
            };
        });

        this.mediaRecorderData.addEventListener("stop", ()=>{
            console.log("暂停 自动下载");
            this.downLoadVideo(recordedChunks);
        });

        this.mediaRecorderData.addEventListener("start", (e) => {
            console.log("开始 录制");
        });
    },

    // 上传视频
    updata(){
      var file = new File(this.blobData, 'video-' + (new Date).toISOString().replace(/:|\./g, '-') + '.mp4', {
          type: 'video/mp4'
        });

        const data = new FormData();
        // data.append('video-filename', file.name);
        data.append('file', file);
        // uploadVideoFile(data).then(response=>{

        //     // console.log(response,"response");
        // })
    }
  },
};
</script>

<style>
</style>