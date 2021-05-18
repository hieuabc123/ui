<template>
  <div class="navbar">
      <div class="navbar-header">
          <div class="menu-option">
              <a href class="mi icon-menu-option "></a>
          </div>
          <div class="logo-container">
              <a href="/">
                  <img src="../../assets/img/Logo_Module_TiengViet_White.66947422.svg" height="100%"/>
              </a>
          </div>
      </div>
      <div class="navbar-content">
          <router-link to="/" class="menu-item">
            <div class="menu-item-icon mi mi-20 icon-dashboard"></div>
            <div class="menu-item-title">Tổng quan</div>
          </router-link>
          <router-link to="/" class="menu-item">
            <div class="menu-item-icon mi mi-20 icon-cash"></div>
            <div class="menu-item-title">Tiền mặt</div>
          </router-link>
          <router-link to="/" class="menu-item">
            <div class="menu-item-icon mi mi-20 icon-bank"></div>
            <div class="menu-item-title">Tiền gửi</div>
          </router-link>
          <router-link to="/" class="menu-item">
            <div class="menu-item-icon mi mi-20 icon-purchase"></div>
            <div class="menu-item-title">Mua hàng</div>
          </router-link>
          <router-link to="/" class="menu-item">
            <div class="menu-item-icon mi mi-20 icon-sale"></div>
            <div class="menu-item-title">Bán hàng</div>
          </router-link>
          <router-link to="/" class="menu-item">
            <div class="menu-item-icon mi mi-20 icon-invoice"></div>
            <div class="menu-item-title">Quản lý hóa đơn</div>
          </router-link>
          <router-link to="/" class="menu-item">
            <div class="menu-item-icon mi mi-20 icon-stock"></div>
            <div class="menu-item-title">Kho</div>
          </router-link>
          <router-link to="/" class="menu-item">
            <div class="menu-item-icon mi mi-20 icon-tool"></div>
            <div class="menu-item-title">Công cụ dụng cụ</div>
          </router-link>
          <router-link to="/" class="menu-item">
            <div class="menu-item-icon mi mi-20  icon-fixed-assets"></div>
            <div class="menu-item-title">Tài sản cố định</div>
          </router-link>
          <router-link to="/" class="menu-item">
            <div class="menu-item-icon mi mi-20 icon-tax"></div>
            <div class="menu-item-title">Thuế</div>
          </router-link>
          <router-link to="/" class="menu-item">
            <div class="menu-item-icon mi mi-20 icon-price"></div>
            <div class="menu-item-title">Giá thành</div>
          </router-link>
          <router-link to="/" class="menu-item">
            <div class="menu-item-icon mi mi-20 icon-general"></div>
            <div class="menu-item-title">Tổng hợp</div>
          </router-link>
          <router-link to="/" class="menu-item">
            <div class="menu-item-icon mi mi-20 icon-budget"></div>
            <div class="menu-item-title">Ngân sách</div>
          </router-link>
          <router-link to="/" class="menu-item">
            <div class="menu-item-icon mi mi-20 icon-report"></div>
            <div class="menu-item-title">Báo cáo</div>
          </router-link>
          <router-link to="/" class="menu-item">
            <div class="menu-item-icon mi mi-20 icon-finance"></div>
            <div class="menu-item-title">Phân tích tài chính</div>
          </router-link>
          <router-link to="/Employee" class="menu-item">
            <div class="menu-item-icon mi mi-20 icon-employee"></div>
            <div class="menu-item-title">Nhân viên</div>
          </router-link>
      </div>
  </div>
</template>

<script>
export default {
    name:"TheNavbar",

}
</script>

<style scoped>
/* test comment */
.navbar{
    position: absolute;
    border: 1px solid black;
    box-sizing: border-box;
    top: 0;
    left: 0;
    width: 180px;
    height: 100vh;
    background: #393a3d;
    overflow-y: auto;
}

/**1. -----navbar-header------------------------------ */
.navbar-header{
    height: 48px;
    display: flex;
    flex-direction: row;
    align-items: center;
    padding-left: 24px;
    padding-right: 24px;

}
.menu-option{
    width: 24px;
    height: 24px;
    margin-right: 10px;
    display: flex;
    align-items: center;
}
.logo-container{
    height: 32px;
}

/**-------navbar-content--------------------- */ 
.navbar-content{
    padding-top: 14px;
    
}
.navbar-content a{
    color: #fff;
}

.menu-item{
    display: flex;
    height: 42px;
    align-items: center;
    padding-left: 16px;
}

.menu-item-icon{
    margin-right: 8px;
}


/** ------ICON ---------- */
.icon-menu-option{
    background-position:-428px -92px;
    width: 16px;
    height: 16px;
}

.icon-dashboard{
    background-position :-33px -1630px;
	width: 18px;
	height: 20px;
}
.icon-cash{
    background-position: -78px -1631px;
	width: 14px;
	height: 18px;
}
.icon-bank{
    background-position: -121px -1633px;
	width: 16px;
	height: 16px;
}
.icon-purchase{
    background-position: -165px -1632px;
	width: 16px;
	height: 18px;
}
.icon-sale{
    background-position: -206px -1631px;
	width: 20px;
	height: 18px;
}
.icon-invoice{
  background-position: -254px -1632px;
	width: 14px;
	height: 17px;
}
.icon-stock{
  background-position: -294px -1632px;
	width: 18px;
	height: 17px;
}
.icon-tool{
  background-position: -339px -1633px;
	width: 18px;
	height: 17px;;
}
.icon-fixed-assets{
  background-position: -382px -1632px;
	width: 17px;
	height: 18px;
}
.icon-tax{
  background-position: -427px -1634px;
	width: 16px;
	height: 16px;
}
.icon-price{
  background-position: -471px -1633px;
	width: 17px;
	height: 17px;
}
.icon-general{
  background-position: -514px -1632px;
	width: 16px;
	height: 18px;
}
.icon-budget{
  background-position: -382px -1664px;
	width: 16px;
	height: 16px;
}
.icon-report{
  background-position: -552px -1636px;
	width: 14px;
	height: 13px;
}
.icon-finance{
  background-position: -207px -1667px;
	width: 17px;
	height: 14px;
}
.icon-employee{
  background-position: -120px -1664px;
	width: 17px;
	height: 17px;
}
</style>