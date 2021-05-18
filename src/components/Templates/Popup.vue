<template>
  <div class="popup" :class="{ 'popup-open': p_isOpen }">
    <div class="modal"></div>
    <div class="popup-detail" :class="p_mode">
      <!-- <div class="mi icon-close X" @click="closePopup"></div> -->
      <div class="popup-content">
        <div class="mi icon icon-message"></div>
        <div class="message-content">
          <span>{{ p_message }}</span>
        </div>
      </div>
      <div class="popup-bottom">
        <div class="btn-cancel" @click="closePopup">Không</div>
        <div class="btn default btn-delete" @click="btnActionOnClick">
          <p>Xóa</p>
        </div>
        <div class="btn default btn-close" @click="closePopup">
          <p>Đồng ý</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "PopUp",
  props: {
    p_message: {
      type: String,
      default: "",
    },
    p_isOpen: {
      type: Boolean,
      default: false,
    },
    p_mode: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      toast: {
        message: "Xóa thành công",
        mode: "alert",
      },
    };
  },
  methods: {
    btnActionOnClick() {
      this.$emit("p_deleteEmployee");
      this.loadData();
      this.closePopup();
    },
    closePopup() {
      this.$emit("p_closePopup");
    },
    loadData() {
      this.$emit("p_loadAllComponent");
    },
  },
};
</script>

<style scoped>
.popup-open {
  display: flex !important;
}
.popup {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 10001;
  width: 100%;
  height: 100%;
  display: none;
  align-items: center;
  justify-content: center;
}
.modal {
  position: absolute;
  background: #000000;
  opacity: 0.5;
  width: 100%;
  height: 100%;
}

.popup-detail {
  position: absolute;
  background-color: #ffffff;
  border-radius: 4px;
  overflow: hidden;
  max-width: 400px;
  min-width: 300px;
  padding: 32px;
}
.popup-content {
  position: relative;
  display: flex;
  background: #ffffff;
  border-bottom: 1px solid #c7c7c7;
  padding-bottom: 32px;
}

.popup-bottom {
  position: relative;
  bottom: -10px;
  height: 60px;
  width: 100%;
  display: flex;
  align-items: center;
}
.btn {
  position: absolute;
  min-width: 40px;
  right: 0;

  display: flex;
  justify-content: center;
}
.delete .btn {
  background-color: #ff5858;
  color: white;
}
.delete .btn:hover {
  background-color: #ff7878;
}
.alert .btn {
  background-color: #2ca01c;
  color: white;
}
.btn-cancel {
  color: #000000;
  height: 36px;
  padding-left: 20px;
  padding-right: 20px;
  display: flex;
  align-items: center;
  text-align: center;
  cursor: pointer;
  position: absolute;
  left: 0;
  box-sizing: border-box;
  border: 1px solid #8d9096;
  border-radius: 4px;
}
.btn-cancel:hover {
  background: #d2d3d6;
}
span {
  width: 100%;
}

.icon-close {
  background-position: -147px -147px;
  width: 18px;
  height: 18px;
  position: absolute;
  top: 8px;
  right: 8px;
  cursor: pointer;
}
.message-content {
  position: relative;
  display: flex;
  align-items: center;
  max-width: 350px;
  left: 10px;
  margin-left: 10px;
}
.default{
    display: none;
}
.icon {
  position: relative;
}
.alert .icon-message {
  background-position: -598px -463px;
  width: 36px;
  height: 37px;
}
.alert .btn-cancel {
  display: none;
}
.alert .btn-close{
    display: flex;
}
.delete .btn-delete{
    display: flex;
}
.delete .icon-message {
  background-position: -752px -462px;
  width: 36px;
  height: 36px;
}

</style>