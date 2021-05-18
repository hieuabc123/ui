<template>
  <div class="dialog employeeDetail" :class="{ 'dialog-open': p_isOpen }">
    <div class="modal"></div>
    <div class="dialog-detail">
      <!-- HEADER dialog ----------------------------------------------------->
      <div class="dialog-header">
        <div class="mi mi-24 icon-close X" @click="btnCloseOnClick"></div>
        <div class="mi mi-24 icon-question"></div>
        <div class="popup-title">Thông tin nhân viên</div>
        <div class="dialog-type">
          <input type="checkbox" id="customer" />
          <label for="customer">
            <div class="mi custom-checkbox"></div>
          </label>
          <label for="customer" class="type-text">Là khách hàng</label>
        </div>
        <div class="dialog-type">
          <input type="checkbox" id="production" />
          <label for="production">
            <div class="mi custom-checkbox"></div>
          </label>
          <label for="production" class="type-text">Là nhà cung cấp</label>
        </div>
      </div>
      <!-- CONTENT dialog ---------------------------------------------------->
      <div class="dialog-content">
        <div class="form form-1">
          <div class="w-1/2 grid-left">
            <div class="grid-item" id="employeeCode">
              <div class="input-title">
                Mã
                <div style="color: red; padding-left: 4px">&nbsp; *</div>
              </div>
              <div class="input">
                <input
                  type="text"
                  ref="employeeCode"
                  v-model="p_employee.employeeCode"
                  @input="employeeCodeValidate(p_employee.employeeCode)"
                  @focusout="employeeCodeValidate(p_employee.employeeCode)"
                  :class="{ validate: validate.employeeCode.check }"
                />
                <div
                  class="required"
                  :class="{ visible: validate.employeeCode.check }"
                >
                  <span class="tip tip-down"></span>
                  <div class="message">
                    <span>{{ validate.employeeCode.error }}</span>
                  </div>
                </div>
              </div>
            </div>
            <div class="grid-item" id="fullName">
              <div class="input-title">
                Tên
                <div style="color: red; padding-left: 4px">&nbsp; *</div>
              </div>
              <div class="input">
                <input
                  type="text"
                  v-model="p_employee.fullName"
                  @input="fullNameValidate(p_employee.fullName)"
                  @focusout="fullNameValidate(p_employee.fullName)"
                  :class="{ validate: validate.fullName.check }"
                />
                <div
                  class="required"
                  :class="{ visible: validate.fullName.check }"
                >
                  <span class="tip tip-down"></span>
                  <div class="message">
                    <span>{{ validate.fullName.error }}</span>
                  </div>
                </div>
              </div>
            </div>
            <div class="grid-item column-2" id="departmentName">
              <div class="input-title">
                Đơn vị
                <div style="color: red; padding-left: 4px">&nbsp; *</div>
              </div>
              <div class="input">
                <select
                  name="departmentName"
                  id="departmentName"
                  v-model="p_employee.departmentId"
                  @change="departmentIdValidate(p_employee.departmentId)"
                  :class="{ validate: validate.departmentId.check }"
                >
                  <option
                    v-for="department in p_departments"
                    :key="department.departmentId"
                    :value="department.departmentId"
                  >
                    {{ department.departmentName }}
                  </option>
                </select>
                <div
                  class="required"
                  :class="{ visible: validate.departmentId.check }"
                >
                  <span class="tip tip-down"></span>
                  <div class="message">
                    <span>{{ validate.departmentId.error }}</span>
                  </div>
                </div>
              </div>
            </div>
            <div class="grid-item column-2" id="positionName">
              <div class="input-title">Chức danh</div>
              <div class="input">
                <input type="text" v-model="p_employee.positionName" />
              </div>
            </div>
          </div>
          <!-- -->
          <!-- -->
          <!------- Grid-RIGHT  ----------->
          <!-- -->
          <!-- -->
          <div class="w-1/2 grid-right">
            <div class="grid-item" id="dateOfBirth">
              <div class="input-title">Ngày sinh</div>
              <div class="input">
                <input type="date" v-model="p_employee.dateOfBirth" />
              </div>
            </div>
            <div class="grid-item column-2" id="gender">
              <div class="input-title">Giới tính</div>
              <div class="select-gender">
                <label for="1" class="flex">
                  <input
                    type="radio"
                    id="1"
                    value="1"
                    v-model="p_employee.gender"
                  />
                  <div class="custom-radio radio-border">
                    <div class="radio-content"></div>
                  </div>
                  <span>Nam</span>
                </label>

                <label for="0" class="flex">
                  <input
                    type="radio"
                    id="0"
                    value="0"
                    v-model="p_employee.gender"
                  />
                  <div class="custom-radio radio-border">
                    <div class="radio-content"></div>
                  </div>
                  <span>Nữ</span>
                </label>

                <label for="2" class="flex">
                  <input
                    type="radio"
                    id="2"
                    value="2"
                    v-model="p_employee.gender"
                  />
                  <div class="custom-radio radio-border">
                    <div class="radio-content"></div>
                  </div>
                  <span>Khác</span>
                </label>
              </div>
            </div>
            <div class="grid-item column-2" id="identityNo">
              <div class="input-title">Số CMND</div>
              <div class="input">
                <input type="text" v-model="p_employee.identityNo" />
              </div>
            </div>
            <div class="grid-item" id="identityDate">
              <div class="input-title">Ngày cấp</div>
              <div class="input">
                <input type="date" v-model="p_employee.identityDate" />
              </div>
            </div>
            <div class="grid-item column-3" id="identityPlace">
              <div class="input-title">Nơi cấp</div>
              <div class="input">
                <input type="text" v-model="p_employee.identityPlace" />
              </div>
            </div>
          </div>
        </div>
        <!--  -->
        <!--  -->
        <!-- -------------FORM-2------------------ -->
        <!--  -->
        <!--  -->
        <!--  -->
        <div class="form form-2">
          <div class="grid-item column-4" id="address">
            <div class="input-title">Địa chỉ</div>
            <div class="input">
              <input type="text" v-model="p_employee.address" />
            </div>
          </div>
          <div class="grid-item" id="phoneNumber">
            <div class="input-title">Điện thoại di động</div>
            <div class="input">
              <input type="text" v-model="p_employee.phoneNumber" />
            </div>
          </div>
          <div class="grid-item" id="fixedLine">
            <div class="input-title">Điện thoại cố định</div>
            <div class="input">
              <input type="text" v-model="p_employee.fixedLine" />
            </div>
          </div>
          <div class="grid-item" id="email">
            <div class="input-title">Email</div>
            <div class="input">
              <input type="text" v-model="p_employee.email" />
            </div>
          </div>
          <div class="grid-item"></div>
          <div class="grid-item" id="accountNumber">
            <div class="input-title">Tài khoản ngân hàng</div>
            <div class="input">
              <input type="text" v-model="p_employee.accountNumber" />
            </div>
          </div>
          <div class="grid-item" id="bankName">
            <div class="input-title">Tên ngân hàng</div>
            <div class="input">
              <input type="text" v-model="p_employee.bankName" />
            </div>
          </div>
          <div class="grid-item" id="bankBranch">
            <div class="input-title">Chi nhánh</div>
            <div class="input">
              <input type="text" v-model="p_employee.bankBranch" />
              <input
                name="End-Tabindex"
                style="height: 0; border: none; padding: 0"
                @focus="autoFocusInput"
              />
            </div>
          </div>
          <div class="grid-item"></div>
        </div>
      </div>
      <!-- BOTTOM dialog ----------------------------------------------------->
      <div class="dialog-bottom">
        <div class="btn btn-save" @click="btnSaveOnClick">Cất</div>
        <div class="btn btn-save-and-add" @click="btnSaveAndAddOnClick">
          Cất và thêm
        </div>
        <div class="btn btn-cancel" @click="btnCloseOnClick">Hủy</div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "EmployeeDetail",
  props: {
    p_isOpen: {
      typeof: Boolean,
      default: false,
    },
    p_employee: {
      typeof: Object,
      default: [],
    },
    p_departments: {
      typeof: Object,
      default: [],
    },
    p_formMode: {
      typeof: String,
      default: "add",
    },
  },
  data() {
    return {
      employee: [],
      p_gender: null,
      validate: {
        fullName: {
          check: false,
          error: "",
        },
        employeeCode: {
          check: false,
          error: "",
        },
        departmentId: {
          check: false,
          error: "",
        },
      },
      response_error: undefined,
    };
  },
  methods: {
    /*********************************************
     * Xử lý các SỰ KIỆN
     ***********************************************/
    //Nút Cất
    btnSaveOnClick() {
      var btnMode = "save"
      if (this.checkForm(this.p_employee)) {
        if (this.p_formMode == "edit") {
          this.putEmployee(btnMode);
        } else if (this.p_formMode == "add") {
          this.postEmployee(btnMode);
        }
      }
    },
    //Nút Cất và Thêm
    btnSaveAndAddOnClick() {
      var btnMode = "save&add"
      if (this.checkForm(this.p_employee)) {
        if (this.p_formMode == "edit") {
          this.putEmployee(btnMode);
        } else if (this.p_formMode == "add") {
          this.postEmployee(btnMode);
        }
      }
      
    },
    //Nút X, Hủy
    btnCloseOnClick() {
      this.closeDialog();
    },

    /**********************************************************
     * Các FUNCTION
     ******************************************************************************/
    //Đóng dialog
    closeDialog() {
      this.$emit("p_CloseDialog");
    },
    //Open Popup
    openPopup(mode, message) {
      this.$emit("p_openPopup", mode, message);
    },

    /**
     * Auto Focus vào Input mã nhân viên khi mở form
     * Created By Nguyễn Trung Hiếu
     * Created Date 06/4/2021
     */
    autoFocusInput() {
      console.log("focus");
      this.$refs.employeeCode.focus();
    },

    //Sửa nhân viên
    async putEmployee(btnMode) {
      //fix data before PUT
      if (this.p_employee.dateOfBirth == "") {
        this.p_employee.dateOfBirth = null;
      }
      if (this.p_employee.identityDate == "") {
        this.p_employee.identityDate = null;
      }
      var url = "https://localhost:44380/api/employee";
      await axios
        .put(url, this.p_employee)
        .then((res) => {
          console.log(res);
          this.loadAllComponent();
          if(btnMode == "save"){
            this.closeDialog();
          }
          else if(btnMode == "save&add"){
            this.$emit("p_btnAddOnClick");
            this.autoFocusInput();
          }
          this.openToast("success", "sửa thành công");
        })
        .catch((error) => {
          this.openPopup("alert", error.response.data);
        });
    },
    async postEmployee(btnMode) {
      //fix data before POST
      if (this.p_employee.dateOfBirth == "") {
        this.p_employee.dateOfBirth = null;
      }
      if (this.p_employee.identityDate == "") {
        this.p_employee.identityDate = null;
      }
      var url = "https://localhost:44380/api/employee";
      await axios
        .post(url, this.p_employee)
        .then((res) => {
          console.log(res);
          this.loadAllComponent();
          if(btnMode == "save"){
            this.closeDialog();
          }
          else if(btnMode == "save&add"){
            this.$emit("p_btnAddOnClick");
            this.autoFocusInput();
          }
          this.openToast("success", "Thêm mới thành công");
        })
        .catch((error) => {
          this.openPopup("alert", error.response.data.devMsg);
        });
    },

    //Load lại toàn bộ component
    loadAllComponent() {
      this.$emit("p_loadAllComponent");
    },

    /**
     * Check lại toàn bộ form dữ liệu, kiểm tra các trường có dấu *
     * Created By Nguyễn Trung Hiếu
     * Created Date 07/4/2021
     */
    checkForm(employee) {
      this.employeeCodeValidate(employee.employeeCode);
      this.fullNameValidate(employee.fullName);
      this.departmentIdValidate(employee.departmentId);
      if (this.employeeCodeValidate(employee.employeeCode)) {
        this.openPopup("alert", this.validate.employeeCode.error);
        return false;
      } else if (this.fullNameValidate(employee.fullName)) {
        this.openPopup("alert", this.validate.fullName.error);
        return false;
      } else if (this.departmentIdValidate(employee.departmentId)) {
        this.openPopup("alert", this.validate.departmentId.error);
        return false;
      } else {
        return true;
      }
    },

    openToast(mode,message){
      this.$emit("p_openToast",mode,message)
    },

    /*****************************************************************************
     * Định nghĩa các Validation
     * Created By Nguyễn Trung Hiếu
     * Created Date 06/4/2021
     *****************************************************************************************************/
    //--------Validate EmployeeCode (Mã nhân viên)--------
    employeeCodeValidate(employeeCode) {
      if (employeeCode == "" || employeeCode == null) {
        this.validate.employeeCode.check = true;
        this.validate.employeeCode.error = `Không được để trống "mã nhân viên"`;
        return true;
      } else {
        this.validate.employeeCode.check = false;
        this.validate.employeeCode.error = "";
        return false;
      }
    },
    //--------Validate Full Name (Họ và tên)--------
    fullNameValidate(fullName) {
      if (fullName == "" || fullName == null) {
        this.validate.fullName.check = true;
        this.validate.fullName.error = `Không được để trống "Họ và tên"`;
        return true;
      } else {
        this.validate.fullName.check = false;
        this.validate.fullName.error = "";
        return false;
      }
    },
    departmentIdValidate(departmentId) {
      if (departmentId == "" || departmentId == null) {
        this.validate.departmentId.check = true;
        this.validate.departmentId.error = `Không được để trống "Đơn vị"`;
        return true;
      } else {
        this.validate.departmentId.check = false;
        this.validate.departmentId.error = "";
        return false;
      }
    },
  },
  mounted() {
    this.autoFocusInput();
  },
};
</script>

<style scoped>
/* -----LAYOUT dialog----------------------------------------- */
.dialog {
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 10000;
  align-items: center;
  justify-content: center;
}
.dialog-open {
  display: flex;
}
.modal {
  position: absolute;
  background: #000000;
  opacity: 0.5;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
.dialog .dialog-detail {
  position: relative;
  width: 900px;
  background-color: #ffffff;
  border-radius: 4px;
  overflow: hidden;
}
.dialog-header {
  position: relative;
  height: 76px;
  top: 0;
  left: 0;
  right: 0;
  display: flex;
  align-items: center;
  padding-left: 32px;
}
.dialog-content {
  position: relative;
  top: 0;
  left: 0;
  right: 0;
  display: flex;
  flex-direction: column;
  padding-left: 32px;
  padding-right: 32px;
}
.dialog-bottom {
  position: relative;
  top: 0;
  left: 0;
  right: 0;
  display: flex;
  height: 80px;
  align-items: center;
}

/* ------------------ Các thành phần con --------------------------------- */
.popup-title {
  font-size: 24px;
  color: #111;
  font-weight: 700;
  font-family: inherit;
}
.dialog-type {
  display: flex;
  align-items: center;
}
.type-text {
  margin-left: 8px;
  cursor: pointer;
}
.dialog-type {
  margin-left: 24px;
}
.form {
  position: relative;
  width: 100%;
}
.form-1 {
  display: grid;
  grid-template-columns: calc(50% - 13px) auto;
  grid-gap: 26px;
}
.form-1 .grid-left {
  box-sizing: border-box;
  /* border: 1px solid black; */
  display: grid;
  grid-template-columns: 150px auto;
  grid-column-gap: 6px;
  grid-row-gap: 12px;
}
.form-1 .grid-right {
  box-sizing: border-box;
  /* border: 1px solid black; */
  display: grid;
  grid-template-columns: 160px auto 160px;
  grid-column-gap: 6px;
  grid-row-gap: 12px;
}
.form-2 {
  margin-top: 32px;
  display: grid;
  grid-template-columns: calc(25%) calc(25%) calc(25%) auto;
  grid-column-gap: 6px;
  grid-row-gap: 12px;
  padding-bottom: 40px;
  border-bottom: 1px solid #c9ccd2;
}
.input-title {
  height: 22px;
  display: flex;
  padding-left: 0px;
  font-weight: 600;
}
.column-2 {
  grid-column: span 2;
}
.column-3 {
  grid-column: span 3;
}
.column-4 {
  grid-column: span 4;
}
.select-gender {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 32px;
}

/* ---- */
/*--form required --*/
/* ---- */
.visible {
  display: block !important;
}
.required {
  z-index: 10;
  display: none;
  position: absolute;
  bottom: 34px;
  right: -80px;
  min-width: 180px;
  max-width: 250px;
}
.message {
  background: #ff4747;
  color: #ffffff;
  padding-left: 12px;
  padding-right: 12px;
  border-radius: 4px;
  padding-top: 6px;
  padding-bottom: 6px;
}
.tip {
  width: 0px;
  height: 0px;
  position: absolute;
  background: transparent;
  border: 10px solid transparent;
}
.tip-down {
  bottom: -19px;
  left: 80px;
  border-top-color: #ff4747;
}

/* -------Button,INPUT,SELECT------------------------------------------- */
.btn-save,
.btn-cancel {
  border: 1px solid #8d9096;
  box-sizing: border-box;
}
.btn-save:hover,
.btn-cancel:hover {
  background-color: #d2d3d6;
}
.btn-save-and-add {
  color: #fff;
  background: #2ca01c;
}
.btn-save-and-add:hover {
  background-color: #35bf22;
}
.btn-save {
  position: absolute;
  right: 162px;
}
.btn-cancel {
  position: absolute;
  left: 32px;
}
.btn-save-and-add {
  position: absolute;
  right: 32px;
}
.input {
  position: relative;
}
input {
  padding-left: 8px;
  padding-right: 8px;
  border-radius: 3px;
}
input:focus {
  outline: none;
  border: 1px solid #2ca01c;
}
input[type="radio"] {
  display: none;
}
input[type="radio"]:checked + .radio-border {
  border: 1px solid #2ca01c;
}
input[type="radio"]:checked + .custom-radio .radio-content {
  background-color: #2ca01c;
}
.custom-radio {
  margin-right: 8px;
}
.radio-border {
  border: 1px solid #babec5;
  border-radius: 50%;
  width: 18px;
  height: 18px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.radio-content {
  width: 12px;
  height: 12px;

  border-radius: 50%;
}
.validate {
  border: 1px solid red;
}
.validate:focus {
  border: 1px solid red;
}

/* -------ICON--------------------------------- */
.icon-close {
  position: absolute;
  right: 8px;
  top: 8px;
  background-position: -144px -144px;
  cursor: pointer;
}
.icon-question {
  background-position: -89px -144px;
  position: absolute;
  right: 38px;
  top: 8px;
  cursor: pointer;
}
</style>