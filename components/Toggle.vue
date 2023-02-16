<template>
    <div class="switch-holder">
        <label class="switch">
            <input type="checkbox" v-model="selectedFh">
            <span class="slider round"></span>
        </label>
        <p class="labeling">
            <span>C</span>
            <span>F</span>
        </p>
    </div>
</template>
<script>
export default {
    data() {
        return {
            selectedFh: false,
        }
    },
    mounted() {
      this.selectedFh = (localStorage.getItem('showFh') === 'true') ? true : false
      this.$emit('fhValue', this.selectedFh)
    },
    watch: {
        selectedFh: function(val) {
          this.$emit('fhValue', val)
        }
    }
}
</script>
<style lang="scss" scoped>
.switch-holder {
    display: inline-block;
    position: absolute;
    top: 32px;
    right: 32px;
    text-align: right;
}

.switch {
  position: relative;
  display: inline-block;
  width: 60px;
  height: 24px;
}

.switch input { 
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #dedede;
  -webkit-transition: .4s;
  transition: .4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 16px;
  width: 16px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;
}

input:checked + .slider {
  background-color: #FAD2BB;
}

input:focus + .slider {
  box-shadow: 0 0 1px #FAD2BB;
}

input:checked + .slider:before {
  -webkit-transform: translateX(36px);
  -ms-transform: translateX(36px);
  transform: translateX(36px);
}

.slider.round {
  border-radius: 24px;
}

.slider.round:before {
  border-radius: 50%;
}

.labeling {
    display: flex;
    justify-content: space-between;
    width: 64px;
    margin-top: 4px;
    color: #848484;
}
</style>