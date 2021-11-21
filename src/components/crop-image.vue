<template>
  <div>
    <img :src="option.imgSrc" ref="image" id="imageInput" alt="test">
    
    <div class="modal-footer">
      <button type="button" class="btn btn-secondary" data-dismiss="modal" @click="close">Close</button>
      <button type="button" class="btn btn-primary" @click="crop">
        Crop
      </button>
    </div>
  </div>
  
</template>

<script>// @ts-nocheck

// @ts-ignore
import Cropper from 'cropperjs';

export default {
  name:'CropImage',
  data() {
    return {
      imgValue:'',
      option:{
        imgSrc:'',
        size: 1,
        outputType:'jpg',
      },
      cropper:{},
      destination:{},
      image:null,
    };
  },
  mounted() {
    this.image = this.$refs.image;
    this.cropper = new Cropper( this.image ,{
      autoCrop:false,
      viewMode:3,
      zoomable:false,
      scalable:false,
      modal:false,
      aspectRatio:1,
      background:false,
      highlight:false,
      rotatable:false,
      autoCropArea:1,
      guides:false,
    });
  },
  methods: {
    close(){
      this.$emit('toggleModal',this.showmodal);
    },
    async crop(){
      this.$emit('toggleModal',this.showmodal);
      //this.$emit('updateImageData','test'); 
      let canvas = this.cropper.getCroppedCanvas({
        with:180,
        height:180,
      });
      const that = this;
      canvas.toBlob((blob) => {
        let base64;
        let url = URL.createObjectURL(blob);
        let reader = new FileReader();
        reader.readAsDataURL(blob); 
        reader.onloadend = () => {
          base64 = reader.result;
          setTimeout(()=>{
            that.$parent.localValue = base64;
          },1000);
          //that.$attrs.localValue=base64;
          console.log(that);
          //console.log(that.$options.propsData.localValue);
        };
      });
     
    },
  },
  created(){
    this.option.imgSrc= this.options.target;
  },
  props:['showmodal','options',' localValue'],
};
</script>

<style scoped>
 .previw{
   width:200px;
   height: 200px;
 }
</style>