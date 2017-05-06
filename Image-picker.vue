<template lang='html'>
  <div class='image-picker'>

    <div :class='status'>
      <img :src='value' alt='text'>

      <button class='choose-image btn btn-md btn-block btn-primary'>
        <input type='file' @change='onFileChange'>
        Choose image
      </button>

      <button @click='removeImage()'
        class='remove-image btn btn-md btn-block btn-warning'>
        Remove image
      </button>
    </div>

  </div>
</template>

<script>
  export default {
    name: 'ImagePicker',

    data () {
      return {}
    },

    props: ['value'],

    computed: {
      status () {
        if (this.value) return 'active'
        else return 'empty'
      }
    },

    methods: {
      onFileChange (event) {
        let files = event.target.files || event.dataTransfer.files

        if (files.length) {
          this.createImage(files[0])
        }
      },

      createImage (file) {
        let reader = new FileReader()

        reader.onload = (event) => {
          this.updateValue(event.target.result)
        }

        reader.readAsDataURL(file)
      },

      removeImage () {
        this.updateValue('')
      },

      updateValue (value) {
        this.$emit('input', value)
      }
    }
  }
</script>

<style lang='scss'>
  .image-picker {
    display: block;
    width: 100%;
    margin: 0 auto;
  }

  .image-picker img {
    width: 100%;
    border-radius: 8px;
  }

  .image-picker .btn.choose-image {
    position: relative;
  }

  .image-picker input[type='file'] {
    position: absolute;
    top: 0;
    left: 0;
    display: block;
    width: 100%;
    height: 100%;
    opacity: 0;
    cursor: pointer;
  }

  .active {
    img {
      height: auto;
      border: 4px solid white;
      opacity: 1;
    }

    .choose-image {
      display: none;
    }

    .remove-image {
      display: block;
    }
  }

  .empty {
    img {
      height: 0;
      opacity: 0;
    }

    .btn.choose-image {
      display: block;
    }

    .btn.remove-image {
      display: none;
    }
  }
</style>
