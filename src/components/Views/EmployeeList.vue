<template>
  <div class="EmployeeList">
    <div class="loading" :class="{ displayNone: isDone }">
      <div class="modal"></div>
      <div class="lds-dual-ring"></div>
    </div>
    <div class="title-distance">
      <div class="title">Nhân viên</div>
      <div class="btn btn-add" @click="btnAddOnClick">Thêm mới nhân viên</div>
    </div>
    <div class="grid">
      <div class="grid-header">
        <div class="mi icon-load pointer" @click="loadAllComponent"></div>
        <div class="input-search">
          <input
            type="text"
            placeholder="tìm kiếm theo mã, tên nhân viên"
            v-model="filter"
            @input="filterData"
            @keyup.enter="filterData"
          />
          <div class="mi icon-search pointer" @click="filterData"></div>
        </div>
      </div>
      <div class="grid-content">
        <table>
          <thead>
            <tr>
              <th class="checkbox">
                <input type="checkbox" id="all" />
                <label for="all">
                  <div class="mi custom-checkbox"></div>
                </label>
                <div class="border-right"></div>
              </th>
              <th class="EmployeeCode">Mã nhân viên</th>
              <th class="FullName">Tên nhân viên</th>
              <th class="Gender">Giới tính</th>
              <th class="DateOfBirth">Ngày sinh</th>
              <th class="IdentityNo">Số CMND</th>
              <th class="Position">Chức danh</th>
              <th class="DepartmentName">Tên đơn vị</th>
              <th class="AccountNumber">Số tài khoản</th>
              <th class="BankName">Tên ngân hàng</th>
              <th class="BankBranch">Chi nhánh tk ngân hàng</th>
              <th class="Function">
                Chức năng
                <div class="border-left"></div>
              </th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="(employee, index) in employees"
              :key="employee.employeeId"
              @click="trOnClick(employee)"
              @dblclick="trDblOnClick(employee.employeeId)"
              :class="{
                selected: employee.employeeId == selectRow.employeeId,
              }"
            >
              <td class="checkbox">
                <input type="checkbox" :id="100 - index" />
                <label :for="100 - index">
                  <div class="mi custom-checkbox"></div>
                </label>
                <div class="border-right"></div>
              </td>
              <td class="EmployeeCode">{{ employee.employeeCode }}</td>
              <td class="FullName">{{ employee.fullName }}</td>
              <td class="Gender">{{ employee.genderName }}</td>
              <td class="DateOfBirth">
                {{ frontEndDateFormat(employee.dateOfBirth) }}
              </td>
              <td class="IdentityNo">{{ employee.identityNo }}</td>
              <td class="Position">{{ employee.positionName }}</td>
              <td class="DepartmentName">{{ employee.departmentName }}</td>
              <td class="AccountNumber">{{ employee.accountNumber }}</td>
              <td class="BankName">{{ employee.bankName }}</td>
              <td class="BankBranch">{{ employee.bankBranch }}</td>
              <td class="Function" :style="{ 'z-index': 100 - index }">
                <div class="function-content">
                  <div
                    class="btn-edit"
                    @click="btnEditOnClick(employee.employeeId)"
                  >
                    Sửa
                  </div>
                  <div
                    class="btn btn-toggle-select"
                    @click="btnToggleFunctionOnClick(employee)"
                  >
                    <div class="mi icon-toggle-select"></div>
                  </div>
                  <div
                    class="toggle-select"
                    :class="{
                      'toggle-selected':
                        employee.employeeId ==
                          selectToggleFunction.employeeId && is_active,
                    }"
                  >
                    <div class="toggle-select-item duplicate">Nhân bản</div>
                    <div
                      class="toggle-select-item delete"
                      @click="btnDeleteOnClick(employee)"
                    >
                      Xóa
                    </div>
                    <div class="toggle-select-item stop-use">Ngừng sử dụng</div>
                  </div>
                  <div class="border-left"></div>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="grid-bottom pagination-bar">
        <div class="left-pagination">
          <div class="total-record">
            Tổng số: <b>{{ totalRows }}</b> bản ghi
          </div>
        </div>
        <div class="right-pagination">
          <div class="record-in-page">
            <select
              class="record-in-page-value"
              v-model="pageSize"
              @change="loadAllComponent"
            >
              <option value="10" @click="test">10 bản ghi trên 1 trang</option>
              <option value="20">20 bản ghi trên 1 trang</option>
              <option value="30">30 bản ghi trên 1 trang</option>
              <option value="50">50 bản ghi trên 1 trang</option>
            </select>
          </div>
          <paginate
            :page-count="p_pageCount"
            :click-handler="pageNumberOnClick"
            :prev-text="'Trước'"
            :next-text="'Sau'"
            :container-class="'select-page'"
            :prev-link-class="'page-prev'"
            :next-link-class="'page-next'"
            :page-link-class="'page-link'"
            :page-class="'page-number'"
          >
          </paginate>
        </div>
      </div>
    </div>

    <EmployeeDetail
      :p_isOpen="dialog.isOpen"
      :p_employee="m_employee"
      :p_departments="departments"
      :p_formMode="dialog.formMode"
      v-if="dialog.isOpen"
      @p_CloseDialog="closeDialog"
      @p_loadAllComponent="loadAllComponent"
      @p_openPopup="openPopup"
      @p_btnAddOnClick="btnAddOnClick"
      @p_openToast="openToast"
    />
    <Toast
      :isShow="toast.isShow"
      :isHide="toast.isHide"
      :toastMode="toast.mode"
      :message="toast.message"
    />
    <Popup
      :p_isOpen="popup.isOpen"
      :p_message="popup.message"
      :p_mode="popup.mode"
      @p_deleteEmployee="deleteEmployee(selectRow.employeeId)"
      @p_closePopup="closePopup"
    />
  </div>
</template>

<script>
import Paginate from "vuejs-paginate";
import axios from "axios";
import moment from "moment";
import EmployeeDetail from "./EmployeeDetail.vue";
import Toast from "../Templates/Toast.vue";
import Popup from "../Templates/Popup.vue";
export default {
  name: "EmployeeList",
  components: {
    Paginate,
    EmployeeDetail,
    Toast,
    Popup,
  },
  data() {
    return {
      employees: [],
      m_employee: [],
      departments: [],
      pageInt: 1,
      pageSize: 20,
      filter: "",
      totalRows: 0, //Tổng số dòng
      p_pageCount: 1, //Tính số trang
      selectRow: [],
      selectToggleFunction: [],
      is_active: false, // Active ẩn hiện toogle Function
      isDone: false, //Kiểm tra hoàn thành load dữ liệu
      dialog: {
        isOpen: false,
        formMode: "add",
      },

      popup: {
        isOpen: false,
        message: "",
        mode: "delete",
      },
      toast: {
        isShow: false,
        isHide: false,
        mode: "",
        message: "",
      },
    };
  },
  methods: {
    /**
     * <summary> Load lại toàn bộ trang Component</summary>
     * CreatedBy: Nguyễn Trung Hiếu (09/5/2021)
     ********************************************************/
    loadAllComponent() {
      this.loadGridContent();
      this.loadGridBottom();
    },
    /**
     * <summary> Load Grid Content</summary>
     * CreatedBy: Nguyễn Trung Hiếu (09/5/2021)
     ********************************************************/
    loadGridContent() {
      this.loadDataPagingFilter(); //dữ liệu Phân trang
    },
    /**
     * <summary> Load Grid Bottom</summary>
     * CreatedBy: Nguyễn Trung Hiếu (09/5/2021)
     ********************************************************/
    loadGridBottom() {
      this.loadTotalOfRows(); //load số bản ghi, và số trang
    },
    /**
     * <summary> Hàm tính toán số bản ghi và số trang</summary>
     * CreatedBy: Nguyễn Trung Hiếu (09/5/2021)
     ********************************************************/
    loadTotalOfRows() {
      var p_pageInt = 1;
      var p_pageSize = 100000;
      var p_filter = this.filter.toString();
      if (p_filter == null || p_filter == "") p_filter = "all";
      var url =
        "https://localhost:44380/api/employee/total?pageint=" +
        p_pageInt +
        "&pagesize=" +
        p_pageSize +
        "&filter=" +
        p_filter;
      axios
        .get(url)
        .then((res) => {
          this.totalRows = res.data; // Tổng số bản ghi
          this.p_pageCount =
            Math.floor(
              parseInt(this.totalRows) / parseInt(this.pageSize) - 0.01
            ) + 1; // Tính số trang = tổng số bản ghi / pageSize
          console.log(this.totalRows);
        })
        .catch((res) => {
          console.log(res);
        });
    },
    /**
     * <summary> Load dữ liệu phân trang (Pagination)</summary>
     * CreatedBy: Nguyễn Trung Hiếu (09/5/2021)
     ********************************************************/
    loadDataPagingFilter() {
      this.isDone = false;
      var p_pageInt = this.pageInt.toString();
      var p_pageSize = this.pageSize.toString();
      var p_filter = this.filter.toString();
      if (p_filter == null || p_filter == "") p_filter = "all";
      var url =
        "https://localhost:44380/api/employee/paging/filter?pageint=" +
        p_pageInt +
        "&pagesize=" +
        p_pageSize +
        "&filter=" +
        p_filter;
      axios
        .get(url)
        .then((res) => {
          this.employees = res.data;
          this.isDone = true;
          console.log(this.employees);
        })
        .catch((res) => {
          console.log(res);
        });
    },
    loadDataById(Id) {
      var url = "https://localhost:44380/api/employee/" + Id;
      axios
        .get(url)
        .then((res) => {
          this.m_employee = res.data;
          this.m_employee.dateOfBirth = this.backEndDateFormat(
            this.m_employee.dateOfBirth
          );
          this.m_employee.identityDate = this.backEndDateFormat(
            this.m_employee.identityDate
          );
        })
        .catch((error) => {
          console.log(error);
        });
    },
    /**
     * <summary> Xóa dữ liệu 1 dòng (Pagination)</summary>
     * CreatedBy: Nguyễn Trung Hiếu (09/5/2021)
     ********************************************************/
    deleteEmployee(employeeId) {
      var url = "https://localhost:44380/api/employee/" + employeeId;
      axios
        .delete(url)
        .then((res) => {
          this.loadAllComponent(); //Nếu xóa thành công load lại trang
          console.log(res.data);
          this.openToast("alert", "Xóa thành công 1 bản ghi!");
        })
        .catch((error) => {
          console.log(error);
        });
    },

    /****************************************************************
     * <summary> Xử lý các sự kiện </summary>
     * CreatedBy: Nguyễn Trung Hiếu (09/5/2021)
     *******************************************************************/
    //Test
    test() {
      alert("test");
    },
    //Click vào pageNumber (Phân trang)
    pageNumberOnClick(page) {
      this.pageInt = page;
      this.loadGridContent(); //load lại phần Content
    },

    //Click vào tr (DÒNG)
    trOnClick(employee) {
      this.selectRow = employee;
    },

    //Click vào toggle button ở cột Chức Năng
    btnToggleFunctionOnClick(employee) {
      this.selectToggleFunction = employee;
      this.is_active = !this.is_active;
    },
    //Click vào nút Xóa
    btnDeleteOnClick(employee) {
      this.is_active = false;
      var message =
        `Bạn có muốn xóa thông tin của nhân viên "` +
        employee.employeeCode +
        `" không?`;
      var mode = "delete";
      this.openPopup(mode, message);
    },
    // Click vào nút thêm mới nhân viên
    btnAddOnClick() {
      this.m_employee = this.getNewEmployee();
      this.openDialog("add");
    },
    // Double CLick vào dòng <tr></tr>
    trDblOnClick(employeeId) {
      this.loadDataById(employeeId); //load dữ liệu detail
      // load dữ liệu department
      this.openDialog("edit");
    },
    // Click vào nút sửa
    btnEditOnClick(employeeId) {
      this.loadDataById(employeeId); //load dữ liệu detail

      // load dữ liệu department
      this.openDialog("edit");
    },

    /****************************************************************************
     * Các FUNCTION
     *****************************************************************************/
    //Mở Dialog
    openDialog(formMode) {
      this.dialog.formMode = formMode;
      this.loadDepartments();
      this.dialog.isOpen = true;
    },
    //Đóng Dialog
    closeDialog() {
      this.dialog.isOpen = false;
    },
    //Mở Popup
    openPopup(p_mode, p_message) {
      this.popup.mode = p_mode;
      this.popup.message = p_message;
      this.popup.isOpen = true;
    },

    closePopup() {
      this.popup.isOpen = false;
    },
    //load dữ liệu Departments
    loadDepartments() {
      var url = "https://localhost:44380/api/department";
      axios
        .get(url)
        .then((res) => {
          this.departments = res.data;
        })
        .catch((res) => {
          console.log(res);
        });
        
    },
    getNewEmployee() {
      var newEmployee = {
        employeeCode: null,
        fullName: null,
        gender: 2,
        genderName: null,
        dateOfBirth: null,
        identityNo: null,
        identityDate: null,
        identityPlace: "",
        departmentId: "",
        departmentName: "",
        positionName: null,
        accountNumber: "",
        bankName: null,
        bankBranch: "",
        address: "",
        phoneNumber: null,
        fixedLine: "",
        email: null,
        createdDate: "",
        createdBy: "",
        modifiedDate: "",
        modifiedBy: "",
      };
      var url = "https://localhost:44380/api/employee/newemployeecode";
      axios
        .get(url)
        .then((res) => {
          newEmployee.employeeCode = res.data;
        })
        .catch((error) => {
          console.log(error);
        });
      return newEmployee;
    },
    filterData() {
      this.loadAllComponent();
    },

    /**
     * Toast Notification
     * Created By Nguyễn Trung Hiếu
     * Created Date 07/4/2021
     */
    openToast(mode, message) {
      this.showToast();
      setTimeout(this.hideToast, 2000);
      this.toast.message = message;
      this.toast.mode = mode;
    },
    showToast() {
      this.toast.isShow = true;
      this.toast.isHide = false;
    },
    hideToast() {
      this.toast.isShow = false;
      this.toast.isHide = true;
    },

    /****************************************************************************
     * Các fomat định dạng cho dữ liệu
     * Created By Nguyễn Trung Hiếu
     * Created Date 05/4/2021
     *****************************************************************************/
    // Fomat địnhh dạng DD/MM/YYYY cho table
    frontEndDateFormat(date) {
      return moment(date, "YYYY-MM-DD").format("DD/MM/YYYY");
    },
    // Fomat định dạng YYYY/MM/DD cho input[type = date]
    backEndDateFormat(date) {
      if (date == "" || date == null) return (date = "");
      else return moment(date, "YYYY-MM-DD").format("YYYY-MM-DD");
    },
  },
  created() {
    this.loadAllComponent();
  },
};
</script>

<style>
/* --------------------Layout------------------------- */
.title-distance {
  position: absolute;
  top: 0;
  left: 20px;
  right: 20px;
  height: 60px;
  display: flex;
  align-items: center;
}
.title {
  font-family: notosans-semibold;
  font-size: 24px;
}
.grid {
  position: absolute;
  top: 60px;
  left: 20px;
  right: 20px;
  bottom: 10px;
  background: #fff;
}
.grid-header {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 72px;
  display: flex;
  align-items: center;
  box-sizing: border-box;
}
.grid-content {
  position: absolute;
  top: 72px;
  left: 0;
  right: 0;
  bottom: 46px;
  box-sizing: border-box;
  /* border: 1px solid black; */
  overflow: auto;
}
.grid-bottom {
  position: absolute;
  height: 46px;
  left: 0;
  right: 0;
  bottom: 0;
  box-sizing: border-box;
  /* border: 1px solid green; */
}
/*---------------------- Các phần tử con của trang ---------------------------- */
.pagination-bar {
  display: flex;
  align-items: center;
}
.left-pagination {
  position: absolute;
  left: 0;
  height: 100%;
  display: flex;
  align-items: center;
}
.right-pagination {
  position: absolute;
  right: 0;
  height: 100%;
  display: flex;
  align-items: center;
}
.record-in-page {
  height: 32px;
  width: 190px;
}
.select-page {
  display: flex;
  align-items: center;
  height: 32px;
  margin-left: 16px;
  margin-right: 16px;
  padding-left: 0;
}

/* ---------------Table-------------------------------------------------------------- */
table {
  width: 2180px;
  min-width: 100%;
  box-sizing: border-box;
  border-collapse: collapse;
}

tr {
  border-bottom: 1px solid #c7c7c7;
  /* box-sizing: border-box; */
}
tbody tr:hover td {
  background: #f3f8f8;
}
tbody .selected td {
  background: #f9f9f9;
}
th:not(:last-child),
td:not(:last-child) {
  border-right: 1px dotted #c7c7c7;
}

td {
  padding: 8px;
  height: 48px;
  box-sizing: border-box;
}
th {
  background: #eceef1 !important;
  height: 34px;
  box-sizing: border-box;
}

.checkbox {
  padding: 0;
  width: 40px;
  position: sticky;
  left: 0;
  background: #fff;
}
.EmployeeCode {
  width: 150px;
}
.FullName {
  width: 250px;
}
.Gender {
  width: 120px;
}
.DateOfBirth {
  width: 150px;
}
.IdentityNo {
  width: 200px;
}
.Position {
  width: 250px;
}
.DepartmentName {
  width: 250px;
}
.AccountNumber {
  width: 150px;
}
.BankName {
  width: 250px;
}
.BankBranch {
  width: 250px;
}
.Function {
  padding: 0;
  width: 120px;
  position: sticky;
  right: 0;
  background: #fff;
}
.border-left {
  position: absolute;
  left: -1px;
  top: 0;
  bottom: 0;
  border-left: 1px dotted #c7c7c7;
  box-sizing: border-box;
}
.border-right {
  position: absolute;
  right: -1px;
  top: 0;
  bottom: 0;
  border-left: 1px dotted #c7c7c7;
  box-sizing: border-box;
}
.function-content {
  height: 100%;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.toggle-select {
  display: none;
  position: absolute;
  background: #fff;
  z-index: 1000;
  top: 35px;
  right: 35px;
  width: 120px;
  border: 1px solid #c7c7c7;
  border-radius: 4px;
}
.toggle-selected {
  display: block !important;
}
.toggle-select-item {
  height: 30px;
  display: flex;
  align-items: center;
  padding-left: 10px;
}
.toggle-select-item:hover {
  color: #13c128;
  background-color: #e8e9ec;
  cursor: pointer;
}

/* -----------Các button, active, hover,input---------------------------- */
input[type="checkbox"] {
  display: none;
}
input[type="checkbox"] + label {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 16px;
  height: 16px;
  border: 1px solid #afafaf;
  border-radius: 2px;
  cursor: pointer;
  margin: auto;
  background: #fff;
}
input[type="checkbox"] + label:hover {
  box-shadow: 0 0 5pt #d3d3d3;
}
input[type="checkbox"]:checked + label .custom-checkbox {
  background-position: -899px -316px;
  width: 11px;
  height: 8px;
}
.disabled {
  cursor: none;
  color: #9e9e9e;
}
.page-prev {
  margin-right: 16px;
}
.page-next {
  margin-left: 16px;
}
.page-index {
  display: flex;
  align-items: center;
}
.page-number {
  font-weight: 700;
  margin-left: 4px;
  margin-right: 4px;
}
.page-link {
  width: 21px;
  height: 21px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.input-search {
  position: absolute;
  display: flex;
  align-items: center;
  right: 60px;
  height: 32px;
  width: 230px;
}
.active {
  border: 1px solid #c7c7c7;
  border-radius: 2px;
}
.btn {
  padding-left: 24px;
  padding-right: 24px;
  height: 36px;
  display: flex;
  align-items: center;
  border-radius: 4px;
  cursor: pointer;
}
.btn-toggle-select {
  padding-left: 15px;
  padding-right: 15px;
  margin-left: 8px;
  height: 20px;
}
.btn-add {
  color: #fff;
  background: #2ca01c;
  margin-left: auto;
}
.btn-add:hover {
  background-color: #35bf22;
}
.btn-edit {
  margin-left: 20px;
  color: #0075c0;
  font-family: notosans-semibold;
  cursor: pointer;
}
.icon-load {
  position: absolute;
  right: 20px;
  background-position: -425px -201px;
  width: 20px;
  height: 23px;
}
.icon-load:hover {
  background-position: -1098px -90px;
}
.icon-search {
  background-position: -992px -360px;
  width: 16px;
  height: 16px;
  position: absolute;
  right: 8px;
}
.icon-toggle-select {
  background-position: -900px -365px;
  width: 8px;
  height: 5px;
}

/* loading CSS */
.loading {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 2000;
}
.lds-dual-ring {
  position: absolute;
  top: 40%;
  left: 50%;
  bottom: 50%;
  transform: translate(-50%, -50%);
  display: inline-block;
  width: 80px;
  height: 80px;
}
.lds-dual-ring:after {
  content: "";
  display: block;
  width: 64px;
  height: 64px;
  margin: 8px;
  border-radius: 50%;
  border: 6px solid #fff;
  border-color: #019160 transparent #019160 transparent;
  animation: lds-dual-ring 1.2s linear infinite;
}
@keyframes lds-dual-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
.modal {
  position: relative;
  background: #000000;
  opacity: 0.1;
  width: 100%;
  height: 100%;
}
.displayNone {
  display: none !important;
}
</style>