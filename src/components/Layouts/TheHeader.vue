<template>
  <div class="header">
        <div class="mi-24 ">
            <div class="mi icon-toggle-menu"></div>
        </div>
        <div class="header-branch">
            <div class="header-branch-name">CÔNG TY TNHH SẢN XUẤT - THƯƠNG MẠI - DỊCH VỤ QUI PHÚC</div>
            <div class="header-branch-icon mi-14 ">
                <div class="mi icon-branch"></div>
            </div>
        </div>
        <div class="mi mi-24 icon-bell"></div>
        <div class="account-info">
            <div class="mi mi-32 icon-avatar">
            </div>
            <div class="user-name">Nguyễn Trung Hiếu</div>
            <div class="mi icon-dropdown"></div>
        </div>
  </div>
</template>

<script>
export default {

}
</script>

<style scoped>
.header{
    position: absolute;
    height: 48px;
    left: 180px;
    right: 0;
    top: 0;
    box-sizing: border-box;
    display: flex;
    background: #ffffff;
    align-items: center;
    padding-left: 8px;
}
.header-branch{
    display: flex;
    align-items: center;
    margin-left: 8px;
}
.header-branch-icon{
    margin-left: 8px;
}
.header-branch-name{
    font-size: 14px;
    color: #111;
    font-family: notosans-bold;
}
.account-info{
    margin-right: 8px;
    display: flex;
    align-items: center;
}


/* ---------ICON------------- */
.icon-toggle-menu{
    background-position: -847px -35px;
	width: 16px;
	height: 14px;
}
.icon-branch{
    background-position: -80px -360px;
	width: 14px;
	height: 14px;
    transform: rotate(90deg);
}
.icon-dropdown{
    background-position: -1078px -38px;
	width: 9px;
	height: 6px;
    margin-left: 8px;
}
.icon-avatar{
    margin-right: 8px;
    background-position: -352px -894px;
	width: 32px;
	height: 32px;
    border-radius: 50%;
}
.icon-bell{
    margin-left: auto;
    margin-right: 24px;
    background-position: -788px -30px;
	width: 22px;
	height: 25px;
}
</style>