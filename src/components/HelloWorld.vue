<template>
  <div>
    <input id="select_pic" type="file" v-on:change="onInputFileChange">
    <div class="preview">
      <img id="image" v-bind:src="img_src" v-on:load="onImgload"/>
      <p id="info" v-text="img_info"/>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator'
// import { MCClient, Status, Result  } from '../../lib/ws-mc/mc-client-service'
// import { ConnectionProperties, Address, Credentials, Protocol } from '../../lib/ws-mc/connection-properties'

@Component
export default class HelloWorld extends Vue {
  //@Prop() private mcUrl: string = 'input the MC server url';
  // @Prop() private response: string = '';
  @Prop() private file = undefined;
  /**
   * name
   */
  // @Prop() private input = document.querySelector("input")
  // @Prop() private preview = document.querySelector('.preview');

  @Prop() private fileTypes = [
    'image/jpeg',
    'image/pjpeg',
    'image/png'
  ];

  @Prop() private width = -1;
  @Prop() private height = -1;
  @Prop() private curInput = undefined;
  @Prop() private curImg = undefined;
  @Prop() private curImgInfo = undefined;

  data: {
    img_src: '';
    img_info: ''
  }

  public beforeMount(){
    this.curInput = document.querySelector("#select_pic");
    this.curImg = document.querySelector('#image');
    this.curImgInfo = document.querySelector('#info');

    this.curImg.onImgload = function(){
      this.width = this.curImg.width;
      this.height = this.curImg.height;
    };
  }
  
  public validFileType (file) {
    for(var i = 0; i < this.fileTypes.length; i++) {
      if(file.type === this.fileTypes[i]) {
        return true;
      }
    }
    return false;
  }

  public returnFileSize(number) {
    if(number < 1024) {
      return number + 'bytes';
    } else if(number > 1024 && number < 1048576) {
      return (number/1024).toFixed(1) + 'KB';
    } else if(number > 1048576) {
      return (number/1048576).toFixed(1) + 'MB';
    }
  }

  public onInputFileChange (e) {
    var preview = document.querySelector('.preview');
    var curFiles = this.curInput.files;
    if(curFiles.length != 0) {
      //var list = document.createElement('ol');
      // preview.appendChild(list);
      for(var i = 0; i < curFiles.length; i++) {
        var imageItem = document.createElement('li');
        // var para = document.createElement('p');
        if(this.validFileType(curFiles[i])) {
          this.curImgInfo.textContent = 'File name ' + curFiles[i].name + ', file size ' + this.returnFileSize(curFiles[i].size) + '.';

          this.curImg.src = window.URL.createObjectURL(curFiles[i]);
        }
      }
    }
  }



    // this.mcClient = new MCClient(this.mcUrl)
    // var result:Result = this.mcClient.login(connectionPropertiesMock)
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.box__dragndrop,
.box__uploading,
.box__success,
.box__error {
  display: none;
}
</style>
